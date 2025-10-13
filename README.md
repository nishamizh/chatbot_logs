# chatbot_logs - Create a Chatbot to analyze the logs
<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Report: Log Level Classification</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutrals -->
    <!-- Application Structure Plan: The SPA is structured as a narrative journey through a data science project, divided into four thematic, navigable sections: Overview, Data Exploration, Model Pipeline, and Interactive Demo. This non-linear structure allows users (e.g., stakeholders, other developers) to either follow the project flow sequentially or jump directly to the section of interest. The Data Exploration section is designed as an interactive dashboard to encourage discovery of data patterns. The Model Pipeline visually simplifies the complex ML process. The Interactive Demo provides a hands-on experience with the final product. This structure was chosen to make a technical report accessible and engaging, prioritizing user understanding and interaction over rigidly following the notebook's linear format. -->
    <!-- Visualization & Content Choices: 
        - Report Info: Log Level Counts -> Goal: Inform proportions -> Viz: Donut Chart -> Interaction: Hover tooltips -> Justification: Clear, immediate view of class imbalance -> Library: Chart.js.
        - Report Info: Log Volume over Time -> Goal: Show trends -> Viz: Line Chart -> Interaction: Filter by log level -> Justification: Allows users to investigate temporal patterns for specific log types -> Library: Chart.js.
        - Report Info: Top Components & their Log Levels -> Goal: Compare and find relationships -> Viz: Linked Horizontal Bar + Stacked Bar Charts -> Interaction: Click a component to update distribution -> Justification: Creates a cause-and-effect exploration, revealing which components are "noisiest" or most error-prone -> Library: Chart.js.
        - Report Info: ML Workflow -> Goal: Organize/Explain process -> Viz: HTML/CSS Diagram -> Interaction: Static visual aid -> Justification: Simplifies the multi-step modeling pipeline for non-experts -> Method: Tailwind CSS.
        - Report Info: Model Inference -> Goal: Demonstrate capability -> Viz: Interactive HTML Form -> Interaction: User input triggers simulated prediction -> Justification: Makes the model's purpose tangible and provides a "hands-on" conclusion to the project -> Method: Vanilla JS.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8F7F4;
            color: #333;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 400px;
            max-height: 50vh;
        }
        @media (max-width: 768px) {
            .chart-container {
                height: 300px;
                max-height: 60vh;
            }
        }
        .nav-link {
            transition: color 0.3s ease;
        }
        .nav-link.active {
            color: #10B981; /* Emerald-500 */
            font-weight: 600;
        }
        .badge {
            display: inline-block;
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-weight: 600;
            font-size: 0.875rem;
        }
        .badge-info { background-color: #3B82F6; color: white; }
        .badge-warn { background-color: #F59E0B; color: white; }
        .badge-error { background-color: #EF4444; color: white; }
        .badge-fatal { background-color: #8B5CF6; color: white; }
    </style>
</head>
<body class="antialiased">

    <header class="bg-white/80 backdrop-blur-md sticky top-0 z-50 shadow-sm">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex items-center">
                    <span class="font-bold text-xl text-gray-800">📊 Log Analysis Report</span>
                </div>
                <div class="hidden md:block">
                    <div class="ml-10 flex items-baseline space-x-4">
                        <a href="#overview" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-700 hover:text-emerald-500">Overview</a>
                        <a href="#exploration" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-700 hover:text-emerald-500">Data Exploration</a>
                        <a href="#pipeline" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-700 hover:text-emerald-500">Model Pipeline</a>
                        <a href="#demo" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-700 hover:text-emerald-500">Interactive Demo</a>
                    </div>
                </div>
            </div>
        </nav>
    </header>

    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8 sm:py-12">
        
        <section id="overview" class="mb-16 scroll-mt-24">
            <h1 class="text-4xl font-bold tracking-tight text-gray-900 mb-4">Transformer-Based Log Level Classification</h1>
            <p class="text-lg text-gray-600 max-w-3xl mb-8">
                This interactive report details the end-to-end process of building a machine learning system to automatically classify infrastructure logs. We'll explore the data, walk through the modeling pipeline, and demonstrate the final classifier's capabilities. The primary goal is to leverage a sophisticated BERT model to provide rapid, automated severity analysis, improving log monitoring and incident response.
            </p>

            <div class="bg-white p-6 rounded-xl shadow-md border border-gray-200">
                <h3 class="text-xl font-semibold text-gray-800 mb-4">Project Workflow</h3>
                <div class="flex flex-col md:flex-row items-center justify-between space-y-4 md:space-y-0 md:space-x-4">
                    <div class="text-center p-4 rounded-lg bg-gray-50 flex-1">
                        <div class="text-3xl mb-2">📥</div>
                        <h4 class="font-semibold">Raw Logs</h4>
                        <p class="text-sm text-gray-500">Start with unstructured log data.</p>
                    </div>
                    <div class="text-gray-400 text-2xl font-mono">&rarr;</div>
                    <div class="text-center p-4 rounded-lg bg-gray-50 flex-1">
                        <div class="text-3xl mb-2">🔍</div>
                        <h4 class="font-semibold">Data Wrangling & EDA</h4>
                        <p class="text-sm text-gray-500">Clean, parse, and analyze data patterns.</p>
                    </div>
                    <div class="text-gray-400 text-2xl font-mono">&rarr;</div>
                    <div class="text-center p-4 rounded-lg bg-gray-50 flex-1">
                         <div class="text-3xl mb-2">🤖</div>
                        <h4 class="font-semibold">Model Training</h4>
                        <p class="text-sm text-gray-500">Fine-tune a BERT model for classification.</p>
                    </div>
                    <div class="text-gray-400 text-2xl font-mono">&rarr;</div>
                    <div class="text-center p-4 rounded-lg bg-gray-50 flex-1">
                         <div class="text-3xl mb-2">💡</div>
                        <h4 class="font-semibold">Live Prediction</h4>
                        <p class="text-sm text-gray-500">Deploy for interactive classification.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="exploration" class="mb-16 scroll-mt-24">
            <h2 class="text-3xl font-bold text-gray-900 mb-2">Interactive Data Exploration</h2>
            <p class="text-lg text-gray-600 max-w-3xl mb-8">
                Before building a model, it's crucial to understand the dataset's characteristics. This section provides an interactive dashboard to explore the key findings from the Exploratory Data Analysis (EDA). Interact with the charts to uncover patterns in log levels, temporal trends, and component behavior.
            </p>
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <div class="bg-white p-6 rounded-xl shadow-md border border-gray-200 lg:col-span-1">
                    <h3 class="text-xl font-semibold text-gray-800 mb-4">Log Level Distribution</h3>
                    <div class="chart-container" style="height:300px; max-height: 40vh;">
                        <canvas id="levelDistributionChart"></canvas>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-md border border-gray-200 lg:col-span-2">
                    <h3 class="text-xl font-semibold text-gray-800 mb-4">Log Volume Over Time (by Hour)</h3>
                    <div class="chart-container" style="height:300px; max-height: 40vh;">
                        <canvas id="volumeOverTimeChart"></canvas>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-md border border-gray-200 lg:col-span-3">
                     <h3 class="text-xl font-semibold text-gray-800 mb-2">Top Components Analysis</h3>
                     <p class="text-gray-600 text-sm mb-4">Click on a component in the left chart to see its specific log level breakdown on the right.</p>
                     <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div>
                             <h4 class="font-semibold text-center mb-2">Top 10 Log-Generating Components</h4>
                             <div class="chart-container">
                                <canvas id="topComponentsChart"></canvas>
                            </div>
                        </div>
                        <div>
                             <h4 id="componentDistributionTitle" class="font-semibold text-center mb-2">Log Level Distribution by Component</h4>
                            <div class="chart-container">
                                <canvas id="componentDistributionChart"></canvas>
                            </div>
                        </div>
                     </div>
                </div>
            </div>
        </section>

        <section id="pipeline" class="mb-16 scroll-mt-24">
            <h2 class="text-3xl font-bold text-gray-900 mb-2">The Machine Learning Pipeline</h2>
            <p class="text-lg text-gray-600 max-w-3xl mb-8">
                Building the classifier involves a systematic pipeline from data preparation to model training and evaluation. This section visually breaks down the key stages of our approach using a BERT-based Transformer model.
            </p>
            <div class="bg-white p-8 rounded-xl shadow-md border border-gray-200 space-y-8">
                <div class="flex items-start space-x-6">
                    <div class="bg-emerald-100 text-emerald-700 font-bold rounded-full w-12 h-12 flex items-center justify-center text-xl flex-shrink-0">1</div>
                    <div>
                        <h3 class="text-xl font-semibold text-gray-800">Data Preparation & Feature Engineering</h3>
                        <p class="text-gray-600 mt-1">The model requires a single text input. We concatenate three key log fields to provide rich context for classification.</p>
                        <pre class="bg-gray-800 text-white rounded-md p-3 mt-3 text-sm">input_text = log['Component'] + " | " + log['Content'] + " | " + log['EventTemplate']</pre>
                    </div>
                </div>
                <div class="flex items-start space-x-6">
                    <div class="bg-emerald-100 text-emerald-700 font-bold rounded-full w-12 h-12 flex items-center justify-center text-xl flex-shrink-0">2</div>
                    <div>
                        <h3 class="text-xl font-semibold text-gray-800">Tokenization</h3>
                        <p class="text-gray-600 mt-1">The combined text is converted into a sequence of numerical tokens that the BERT model can understand. This process uses a pre-trained tokenizer that handles sub-word units and special tokens.</p>
                    </div>
                </div>
                <div class="flex items-start space-x-6">
                    <div class="bg-emerald-100 text-emerald-700 font-bold rounded-full w-12 h-12 flex items-center justify-center text-xl flex-shrink-0">3</div>
                    <div>
                        <h3 class="text-xl font-semibold text-gray-800">Model Architecture & Fine-Tuning</h3>
                        <p class="text-gray-600 mt-1">We use a pre-trained `distilbert-base-uncased` model from Hugging Face and add a sequence classification head on top. The model is then fine-tuned on our specific log dataset to learn the patterns associated with each severity level.</p>
                    </div>
                </div>
                 <div class="flex items-start space-x-6">
                    <div class="bg-emerald-100 text-emerald-700 font-bold rounded-full w-12 h-12 flex items-center justify-center text-xl flex-shrink-0">4</div>
                    <div>
                        <h3 class="text-xl font-semibold text-gray-800">Evaluation</h3>
                        <p class="text-gray-600 mt-1">The model's performance is measured on a held-out test set. Key metrics include accuracy and F1-score to account for class imbalance. The training process resulted in a final evaluation loss of <span class="font-mono bg-gray-100 px-2 py-1 rounded">0.0098</span>, indicating high confidence on the test data.</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="demo" class="scroll-mt-24">
             <h2 class="text-3xl font-bold text-gray-900 mb-2">Interactive Model Demo</h2>
            <p class="text-lg text-gray-600 max-w-3xl mb-8">
                This final section provides a hands-on demonstration of the trained classifier. Enter the details of a log message into the fields below and click "Classify Log" to see the model's prediction in real-time. This demo simulates the model's behavior based on its training.
            </p>

            <div class="bg-white p-8 rounded-xl shadow-md border border-gray-200">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div>
                        <h3 class="text-xl font-semibold text-gray-800 mb-4">Log Input</h3>
                        <div class="space-y-4">
                            <div>
                                <label for="logComponent" class="block text-sm font-medium text-gray-700">Component</label>
                                <input type="text" id="logComponent" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-emerald-500 focus:ring-emerald-500 sm:text-sm" placeholder="e.g., org.apache.hadoop.ipc.Client">
                            </div>
                             <div>
                                <label for="logContent" class="block text-sm font-medium text-gray-700">Content</label>
                                <textarea id="logContent" rows="4" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-emerald-500 focus:ring-emerald-500 sm:text-sm" placeholder="e.g., Failed to renew lease for [DFSClient_NONMAPRED...]"></textarea>
                            </div>
                             <div>
                                <label for="logTemplate" class="block text-sm font-medium text-gray-700">Event Template</label>
                                <input type="text" id="logTemplate" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-emerald-500 focus:ring-emerald-500 sm:text-sm" placeholder="e.g., failed to renew lease for [*]">
                            </div>
                        </div>
                        <button id="classifyBtn" class="mt-6 w-full inline-flex items-center justify-center px-6 py-3 border border-transparent text-base font-medium rounded-md shadow-sm text-white bg-emerald-600 hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500">
                            Classify Log
                        </button>
                    </div>
                    <div class="flex flex-col">
                        <h3 class="text-xl font-semibold text-gray-800 mb-4">Prediction Result</h3>
                        <div id="resultContainer" class="flex-grow bg-gray-50 rounded-lg flex items-center justify-center p-6 text-center transition-all duration-300">
                            <p class="text-gray-500">Your classification result will appear here.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <script>
        document.addEventListener('DOMContentLoaded', () => {

            const EDA_DATA = {
                levelDistribution: {
                    labels: ['WARN', 'INFO', 'ERROR', 'FATAL'],
                    data: [2126, 1709, 163, 2],
                    colors: ['#F59E0B', '#3B82F6', '#EF4444', '#8B5CF6']
                },
                volumeOverTime: {
                    labels: ['00', '01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23'],
                    data: [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 5, 0, 0, 0, 0, 0, 5, 3968, 14, 5, 0, 0, 1]
                },
                topComponents: {
                    labels: [
                        'org.apache.hadoop.hdfs.server.datanode.DataNode',
                        'org.apache.hadoop.hdfs.server.namenode.FSNamesystem',
                        'org.apache.hadoop.ipc.Server',
                        'org.apache.hadoop.hdfs.server.namenode.NameNode',
                        'org.apache.hadoop.hdfs.LeaseManager',
                        'org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator',
                        'org.apache.hadoop.mapred.TaskAttemptListenerImpl',
                        'org.apache.hadoop.ipc.Client',
                        'org.apache.hadoop.hdfs.LeaseRenewer',
                        'org.apache.hadoop.mapreduce.v2.app.MRAppMaster'
                    ],
                    data: [426, 357, 269, 219, 194, 182, 169, 162, 161, 158]
                },
                componentLevelDistribution: {
                    'org.apache.hadoop.hdfs.server.datanode.DataNode': [1, 425, 0, 0],
                    'org.apache.hadoop.hdfs.server.namenode.FSNamesystem': [0, 357, 0, 0],
                    'org.apache.hadoop.ipc.Server': [269, 0, 0, 0],
                    'org.apache.hadoop.hdfs.server.namenode.NameNode': [0, 219, 0, 0],
                    'org.apache.hadoop.hdfs.LeaseManager': [0, 194, 0, 0],
                    'org.apache.hadoop.mapreduce.v2.app.rm.RMContainerAllocator': [0, 182, 0, 0],
                    'org.apache.hadoop.mapred.TaskAttemptListenerImpl': [0, 163, 4, 2],
                    'org.apache.hadoop.ipc.Client': [162, 0, 0, 0],
                    'org.apache.hadoop.hdfs.LeaseRenewer': [161, 0, 0, 0],
                    'org.apache.hadoop.mapreduce.v2.app.MRAppMaster': [0, 158, 0, 0]
                }
            };

            const chartConfigDefaults = {
                plugins: {
                    legend: {
                        position: 'bottom',
                        labels: {
                            font: {
                                family: "'Inter', sans-serif"
                            },
                             padding: 20
                        }
                    },
                    tooltip: {
                        titleFont: { family: "'Inter', sans-serif" },
                        bodyFont: { family: "'Inter', sans-serif" }
                    }
                },
                scales: {
                     x: { ticks: { font: { family: "'Inter', sans-serif" } } },
                     y: { ticks: { font: { family: "'Inter', sans-serif" } } }
                },
                maintainAspectRatio: false,
                responsive: true
            };
            
            const charts = {};

            function initCharts() {
                const ctxLevel = document.getElementById('levelDistributionChart').getContext('2d');
                charts.levelDistribution = new Chart(ctxLevel, {
                    type: 'doughnut',
                    data: {
                        labels: EDA_DATA.levelDistribution.labels,
                        datasets: [{
                            data: EDA_DATA.levelDistribution.data,
                            backgroundColor: EDA_DATA.levelDistribution.colors,
                            borderColor: '#F8F7F4',
                            borderWidth: 4,
                        }]
                    },
                    options: { ...chartConfigDefaults, plugins: { ...chartConfigDefaults.plugins, legend: { position: 'right' } } }
                });

                const ctxVolume = document.getElementById('volumeOverTimeChart').getContext('2d');
                charts.volumeOverTime = new Chart(ctxVolume, {
                    type: 'line',
                    data: {
                        labels: EDA_DATA.volumeOverTime.labels,
                        datasets: [{
                            label: 'Total Logs',
                            data: EDA_DATA.volumeOverTime.data,
                            borderColor: '#10B981',
                            backgroundColor: 'rgba(16, 185, 129, 0.1)',
                            fill: true,
                            tension: 0.3
                        }]
                    },
                    options: { ...chartConfigDefaults, plugins: { ...chartConfigDefaults.plugins, legend: { display: false } } }
                });
                
                const ctxTopComponents = document.getElementById('topComponentsChart').getContext('2d');
                charts.topComponents = new Chart(ctxTopComponents, {
                    type: 'bar',
                    data: {
                        labels: EDA_DATA.topComponents.labels.map(l => l.split('.').pop()),
                        datasets: [{
                            label: 'Log Count',
                            data: EDA_DATA.topComponents.data,
                            backgroundColor: '#60A5FA'
                        }]
                    },
                    options: { 
                        ...chartConfigDefaults,
                        indexAxis: 'y',
                        plugins: { ...chartConfigDefaults.plugins, legend: { display: false } },
                        scales: {
                             y: { afterFit: (scale) => { scale.width = 150 } },
                        }
                    }
                });
                
                const ctxComponentDist = document.getElementById('componentDistributionChart').getContext('2d');
                charts.componentDistribution = new Chart(ctxComponentDist, {
                    type: 'bar',
                    data: {
                        labels: ['Selected Component'],
                        datasets: EDA_DATA.levelDistribution.labels.map((label, i) => ({
                            label: label,
                            data: [0],
                            backgroundColor: EDA_DATA.levelDistribution.colors[i]
                        }))
                    },
                    options: {
                        ...chartConfigDefaults,
                        scales: { x: { stacked: true }, y: { stacked: true, display: false } },
                    }
                });

                document.getElementById('topComponentsChart').onclick = (evt) => {
                    const activePoints = charts.topComponents.getElementsAtEventForMode(evt, 'nearest', { intersect: true }, true);
                    if (activePoints.length > 0) {
                        const index = activePoints[0].index;
                        const componentName = EDA_DATA.topComponents.labels[index];
                        updateComponentDistributionChart(componentName);
                    }
                };
            }
            
            function updateComponentDistributionChart(componentName) {
                const data = EDA_DATA.componentLevelDistribution[componentName];
                document.getElementById('componentDistributionTitle').textContent = `Log Levels for ${componentName.split('.').pop()}`;
                
                charts.componentDistribution.data.datasets.forEach((dataset, i) => {
                    dataset.data = [data[i]];
                });
                charts.componentDistribution.update();
            }

            function handleClassification() {
                const component = document.getElementById('logComponent').value.toLowerCase();
                const content = document.getElementById('logContent').value.toLowerCase();
                const template = document.getElementById('logTemplate').value.toLowerCase();
                const resultContainer = document.getElementById('resultContainer');

                let prediction = 'INFO';
                let confidence = 'Medium';

                if (content.includes('fatal') || template.includes('fatal') || component.includes('fatal')) {
                    prediction = 'FATAL';
                    confidence = 'High';
                } else if (content.includes('error') || template.includes('error') || content.includes('exception')) {
                    prediction = 'ERROR';
                    confidence = 'High';
                } else if (content.includes('warn') || template.includes('warn') || content.includes('failed') || content.includes('timeout')) {
                    prediction = 'WARN';
                    confidence = 'High';
                } else if (content.includes('created') || content.includes('starting') || content.includes('success')) {
                    prediction = 'INFO';
                    confidence = 'High';
                }
                
                const badgeClass = `badge-${prediction.toLowerCase()}`;
                
                resultContainer.innerHTML = `
                    <div class="text-center">
                        <p class="text-gray-600 text-sm mb-2">Predicted Log Level</p>
                        <span class="badge ${badgeClass}">${prediction}</span>
                        <p class="text-gray-500 text-xs mt-4">(Simulated Prediction, ${confidence} Confidence)</p>
                    </div>
                `;
            }

            function setupNavScroll() {
                const navLinks = document.querySelectorAll('.nav-link');
                const sections = document.querySelectorAll('section');

                const observer = new IntersectionObserver((entries) => {
                    entries.forEach(entry => {
                        if (entry.isIntersecting) {
                            navLinks.forEach(link => {
                                link.classList.toggle('active', link.getAttribute('href').substring(1) === entry.target.id);
                            });
                        }
                    });
                }, { rootMargin: '-50% 0px -50% 0px' });

                sections.forEach(section => observer.observe(section));
            }
            
            initCharts();
            updateComponentDistributionChart(EDA_DATA.topComponents.labels[0]);
            document.getElementById('classifyBtn').addEventListener('click', handleClassification);
            setupNavScroll();
        });
    </script>

</body>
</html>
