<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Academic Performance Dashboard</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* Clean, minimalist, white background */
        body { 
            font-family: 'Inter', sans-serif; 
            background-color: #ffffff; 
            color: #0f172a; 
            margin: 0; 
            padding: 40px 20px; 
        }
        .header-container {
            text-align: center;
            margin-bottom: 50px;
        }
        h1 { 
            color: #0a192f; 
            margin-bottom: 8px; 
            font-size: 2.2em;
            font-weight: 700;
            letter-spacing: -0.5px;
        }
        p.subtitle { 
            color: #64748b; 
            margin-top: 0; 
            font-size: 1.1em; 
            font-weight: 400;
        }
        .dashboard-container { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(450px, 1fr)); 
            gap: 30px; 
            max-width: 1200px; 
            margin: 0 auto; 
        }
        .card { 
            background: #ffffff; 
            padding: 25px; 
            border-radius: 8px; 
            border: 1px solid #e2e8f0;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.03); 
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .card:hover { 
            transform: translateY(-4px); 
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
        }
        canvas { max-width: 100%; }
        
        @media (max-width: 600px) {
            .dashboard-container { grid-template-columns: 1fr; }
            body { padding: 20px 10px; }
        }
    </style>
</head>
<body>

    <div class="header-container">
        <h1>Academic Performance Dashboard</h1>
        <p class="subtitle">MS Data Science | 3,046 Students | 17 Programs | 2010 - 2014</p>
    </div>

    <div class="dashboard-container">
        <div class="card">
            <canvas id="yearChart"></canvas>
        </div>
        
        <div class="card">
            <canvas id="programChart"></canvas>
        </div>

        <div class="card">
            <canvas id="genderChart"></canvas>
        </div>

        <div class="card">
            <canvas id="bandChart"></canvas>
        </div>
    </div>

    <script>
        // Data Configuration
        const years = ['2010', '2011', '2012', '2013', '2014'];
        const avgCGPA = [3.54, 3.38, 3.49, 3.46, 3.61];
        const avgSGPA = [3.26, 3.08, 3.16, 3.06, 3.07];

        const programs = ['CHE', 'EEE', 'CHM', 'BCH', 'MCE', 'CEN', 'PET', 'ICE'];
        const progCGPA = [3.67, 3.64, 3.56, 3.54, 3.53, 3.52, 3.52, 3.50];

        // Global Font Family for Charts
        Chart.defaults.font.family = "'Inter', sans-serif";
        Chart.defaults.color = '#475569';

        // 1. CGPA & SGPA Trend by Year (Line Chart)
        new Chart(document.getElementById('yearChart'), {
            type: 'line',
            data: {
                labels: years,
                datasets: [
                    { label: 'Avg CGPA', data: avgCGPA, borderColor: '#1d4ed8', backgroundColor: '#1d4ed8', tension: 0.4, borderWidth: 3, pointRadius: 4 },
                    { label: 'Avg SGPA', data: avgSGPA, borderColor: '#94a3b8', backgroundColor: '#94a3b8', tension: 0.4, borderWidth: 2, borderDash: [5, 5], pointRadius: 4 }
                ]
            },
            options: { 
                responsive: true, 
                plugins: { title: { display: true, text: 'CGPA & SGPA Trend by Year', font: { size: 16, weight: '600' }, color: '#0f172a', padding: 20 } } 
            }
        });

        // 2. Top Programs (Horizontal Bar Chart)
        new Chart(document.getElementById('programChart'), {
            type: 'bar',
            data: {
                labels: programs,
                datasets: [{ label: 'Average CGPA', data: progCGPA, backgroundColor: '#2563eb', borderRadius: 4 }]
            },
            options: { 
                indexAxis: 'y', 
                responsive: true, 
                plugins: { 
                    title: { display: true, text: 'Top 8 Programs by CGPA', font: { size: 16, weight: '600' }, color: '#0f172a', padding: 20 }, 
                    legend: { display: false } 
                }, 
                scales: { x: { min: 3.2, grid: { display: false } }, y: { grid: { display: false } } } 
            }
        });

        // 3. Gender Comparison (Grouped Bar Chart)
        new Chart(document.getElementById('genderChart'), {
            type: 'bar',
            data: {
                labels: ['Female', 'Male'],
                datasets: [
                    { label: 'Avg CGPA', data: [3.68, 3.39], backgroundColor: '#1e3a8a', borderRadius: 4 },
                    { label: 'Avg SGPA', data: [3.14, 3.11], backgroundColor: '#60a5fa', borderRadius: 4 }
                ]
            },
            options: { 
                responsive: true, 
                plugins: { title: { display: true, text: 'Performance by Gender', font: { size: 16, weight: '600' }, color: '#0f172a', padding: 20 } }, 
                scales: { y: { min: 2.5, grid: { display: false } }, x: { grid: { display: false } } } 
            }
        });

        // 4. Student Distribution (Bar Chart)
        new Chart(document.getElementById('bandChart'), {
            type: 'bar',
            data: {
                labels: ['1.5-2.0', '2.0-2.5', '2.5-3.0', '3.0-3.5', '3.5-4.0', '4.0-4.5', '4.5-5.0'],
                datasets: [{ label: 'Number of Students', data: [55, 221, 477, 666, 847, 548, 232], backgroundColor: '#1d4ed8', borderRadius: 4 }]
            },
            options: { 
                responsive: true, 
                plugins: { 
                    title: { display: true, text: 'Student CGPA Distribution', font: { size: 16, weight: '600' }, color: '#0f172a', padding: 20 }, 
                    legend: { display: false } 
                },
                scales: { x: { grid: { display: false } }, y: { grid: { display: false } } }
            }
        });
    </script>
</body>
</html>
