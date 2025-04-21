### Visualise the response data with HTML

```powershell
# Group data by day and calculate the daily totals
$dailyData = $response | Group-Object { $_.intervalStartDateTime.ToString("yyyy-MM-dd") } | ForEach-Object {
    [PSCustomObject]@{
        intervalStartDateTime = [DateTime]::Parse($_.Name)
        Value = ($_.Group | Measure-Object -Property Value -Sum).Sum
    }
}

# Sort by date to ensure proper ordering
$dailyData = $dailyData | Sort-Object intervalStartDateTime

# Create the HTML for the line graph
$htmlContent = @"
<!DOCTYPE html>
<html>
<head>
    <title>Conditional Access Blocked Sign-Ins</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-gradient"></script>
    <style>
        .chart-container {
            width: 900px;
            height: 500px;
            margin: 30px auto;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.08);
            background-color: white;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f9fafc;
            padding: 30px;
            color: #333;
            margin: 0;
        }
        h1 {
            text-align: center;
            color: #443e8b;
            font-weight: 600;
            margin-top: 40px;
            margin-bottom: 10px;
        }
        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: 16px;
        }
        .dashboard {
            max-width: 1100px;
            margin: 0 auto;
            padding: 20px;
            border-radius: 15px;
            background: linear-gradient(145deg, #f0f4ff, #ffffff);
        }
    </style>
</head>
<body>
    <div class="dashboard">
        <h1>Conditional Access Blocked Sign-Ins</h1>
        <div class="subtitle">Blocked sign-in data aggregated by day showing activity trends</div>
        <div class="chart-container">
            <canvas id="lineChart"></canvas>
        </div>
    </div>
    <script>
        // Data from PowerShell
        const dates = [$(($dailyData | ForEach-Object { "'$($_.intervalStartDateTime.ToString("yyyy-MM-dd"))'" }) -join ', ')];
        const values = [$(($dailyData | ForEach-Object { $_.Value }) -join ', ')];
        
        // Create the chart
        const ctx = document.getElementById('lineChart').getContext('2d');
        
        const purpleGradient = ctx.createLinearGradient(0, 0, 0, 400);
        purpleGradient.addColorStop(0, 'rgba(116, 90, 242, 0.5)');
        purpleGradient.addColorStop(1, 'rgba(116, 90, 242, 0.0)');
        
        new Chart(ctx, {
            type: 'line',
            data: {
                labels: dates,
                datasets: [{
                    label: 'Daily Values',
                    data: values,
                    borderColor: '#6259ca',
                    backgroundColor: purpleGradient,
                    borderWidth: 3,
                    tension: 0.4,
                    fill: true,
                    pointRadius: 6,
                    pointBackgroundColor: '#4a3bb3',
                    pointBorderColor: 'white',
                    pointBorderWidth: 2,
                    pointHoverRadius: 8,
                    pointHoverBackgroundColor: '#4a3bb3',
                    pointHoverBorderColor: 'white',
                    pointHoverBorderWidth: 2,
                    lineTension: 0.5,
                    cubicInterpolationMode: 'monotone'
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                interaction: {
                    mode: 'index',
                    intersect: false,
                },
                scales: {
                    y: {
                        beginAtZero: true,
                        grid: {
                            drawBorder: false,
                            color: 'rgba(200, 200, 200, 0.15)',
                        },
                        ticks: {
                            font: {
                                family: "'Segoe UI', sans-serif",
                                size: 12
                            },
                            color: '#555'
                        },
                        title: {
                            display: true,
                            text: 'Activity Count',
                            color: '#443e8b',
                            font: {
                                family: "'Segoe UI', sans-serif",
                                size: 14,
                                weight: 'bold'
                            }
                        }
                    },
                    x: {
                        grid: {
                            display: false
                        },
                        ticks: {
                            font: {
                                family: "'Segoe UI', sans-serif",
                                size: 12
                            },
                            color: '#555'
                        },
                        title: {
                            display: true,
                            text: 'Date',
                            color: '#443e8b',
                            font: {
                                family: "'Segoe UI', sans-serif",
                                size: 14,
                                weight: 'bold'
                            }
                        }
                    }
                },
                plugins: {
                    title: {
                        display: false
                    },
                    legend: {
                        display: true,
                        position: 'top',
                        labels: {
                            font: {
                                family: "'Segoe UI', sans-serif",
                                size: 13
                            },
                            color: '#443e8b',
                            boxWidth: 15,
                            usePointStyle: true,
                            pointStyle: 'circle'
                        }
                    },
                    tooltip: {
                        backgroundColor: 'rgba(74, 59, 179, 0.9)',
                        titleFont: {
                            family: "'Segoe UI', sans-serif",
                            size: 14
                        },
                        bodyFont: {
                            family: "'Segoe UI', sans-serif",
                            size: 13
                        },
                        padding: 12,
                        cornerRadius: 8,
                        displayColors: false,
                        callbacks: {
                            label: function(context) {
                                return 'Activity: ' + context.parsed.y;
                            }
                        }
                    }
                }
            }
        });
    </script>
</body>
</html>
"@

# Create a temporary HTML file
$htmlFilePath = "c:\temp\CABlockedSignInReport.html"
$htmlContent | Out-File -FilePath $htmlFilePath -Encoding utf8

# Open the HTML file in the default browser
Invoke-Item $htmlFilePath
```
