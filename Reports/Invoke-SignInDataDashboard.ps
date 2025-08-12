Connect-MgGraph -Scopes "AuditLog.Read.All"

$uri = "/beta/reports/authenticationMethods/userMfaSignInSummary"

$data = Invoke-MgGraphRequest -Uri $uri -OutputType PSObject | Select -Expand Value

# Sort data by date for proper visualization - handle different date formats
$sortedData = $data | Sort-Object { 
    # Try to convert the date string to DateTime object
    $dateStr = $_.createdDateTime
    if ($dateStr -match '(\d{2})/(\d{2})/(\d{4})') {
        # Format: dd/MM/yyyy or MM/dd/yyyy
        try {
            [DateTime]::ParseExact($dateStr, "dd/MM/yyyy HH:mm:ss", $null)
        } catch {
            try {
                [DateTime]::ParseExact($dateStr, "MM/dd/yyyy HH:mm:ss", $null)
            } catch {
                [DateTime]$dateStr
            }
        }
    } else {
        [DateTime]$dateStr
    }
}

# Display the data
$sortedData

# Generate HTML visualization
$htmlContent = @"
<!DOCTYPE html>
<html>
<head>
    <title>Sign-In Data Visualisation</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 20px;
            background-color: #f5f5f5;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 30px;
        }
        .chart-container {
            margin: 30px 0;
            height: 400px;
            position: relative;
        }
        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }
        .stat-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
        }
        .stat-card h3 {
            margin: 0 0 10px 0;
            font-size: 1.2em;
        }
        .stat-card .value {
            font-size: 2em;
            font-weight: bold;
        }
        .data-table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
        }
        .data-table th, .data-table td {
            border: 1px solid #ddd;
            padding: 12px;
            text-align: left;
        }
        .data-table th {
            background-color: #34495e;
            color: white;
        }
        .data-table tr:nth-child(even) {
            background-color: #f2f2f2;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📊 Sign-In Data Visualisation Dashboard</h1>
        
        <div class="stats-container">
"@

# Calculate statistics
$totalSignIns = ($sortedData.totalSignIns | Measure-Object -Sum).Sum
$totalSingleFactor = ($sortedData.singleFactorSignIns | Measure-Object -Sum).Sum
$totalMultiFactor = ($sortedData.multiFactorSignIns | Measure-Object -Sum).Sum
$avgDailySignIns = ($sortedData.totalSignIns | Measure-Object -Average).Average
$mfaPercentage = if ($totalSignIns -gt 0) { [math]::Round($totalMultiFactor/$totalSignIns*100, 1) } else { 0 }

$htmlContent += @"
            <div class="stat-card">
                <h3>Total Sign-ins</h3>
                <div class="value">$totalSignIns</div>
            </div>
            <div class="stat-card">
                <h3>MFA Usage</h3>
                <div class="value">$mfaPercentage%</div>
            </div>
            <div class="stat-card">
                <h3>Daily Average</h3>
                <div class="value">$([math]::Round($avgDailySignIns, 1))</div>
            </div>
            <div class="stat-card">
                <h3>Days Tracked</h3>
                <div class="value">$($sortedData.Count)</div>
            </div>
        </div>

        <div class="chart-container">
            <canvas id="signInChart"></canvas>
        </div>

        <div class="chart-container">
            <canvas id="mfaChart"></canvas>
        </div>

        <h2>📋 Raw Data</h2>
        <table class="data-table">
            <thead>
                <tr>
                    <th>Date</th>
                    <th>Total Sign-ins</th>
                    <th>Single Factor</th>
                    <th>Multi Factor</th>
                    <th>MFA %</th>
                </tr>
            </thead>
            <tbody>
"@

# Add table rows
foreach ($item in $sortedData) {
    # Convert date to consistent format
    $dateStr = $item.createdDateTime
    try {
        if ($dateStr -match '(\d{2})/(\d{2})/(\d{4})') {
            # Try dd/MM/yyyy first, then MM/dd/yyyy
            try {
                $dateObj = [DateTime]::ParseExact($dateStr, "dd/MM/yyyy HH:mm:ss", $null)
            } catch {
                $dateObj = [DateTime]::ParseExact($dateStr, "MM/dd/yyyy HH:mm:ss", $null)
            }
        } else {
            $dateObj = [DateTime]$dateStr
        }
        $date = $dateObj.ToString("yyyy-MM-dd")
    } catch {
        $date = $dateStr.Split(' ')[0]  # Fallback to just the date part
    }
    
    $mfaPercent = if ($item.totalSignIns -gt 0) { [math]::Round($item.multiFactorSignIns/$item.totalSignIns*100, 1) } else { 0 }
    $htmlContent += @"
                <tr>
                    <td>$date</td>
                    <td>$($item.totalSignIns)</td>
                    <td>$($item.singleFactorSignIns)</td>
                    <td>$($item.multiFactorSignIns)</td>
                    <td>$mfaPercent%</td>
                </tr>
"@
}

# Prepare chart data
$dates = $sortedData | ForEach-Object { 
    $dateStr = $_.createdDateTime
    try {
        if ($dateStr -match '(\d{2})/(\d{2})/(\d{4})') {
            try {
                $dateObj = [DateTime]::ParseExact($dateStr, "dd/MM/yyyy HH:mm:ss", $null)
            } catch {
                $dateObj = [DateTime]::ParseExact($dateStr, "MM/dd/yyyy HH:mm:ss", $null)
            }
        } else {
            $dateObj = [DateTime]$dateStr
        }
        "'" + $dateObj.ToString("yyyy-MM-dd") + "'"
    } catch {
        "'" + $dateStr.Split(' ')[0] + "'"
    }
}
$totalSignInsData = $sortedData | ForEach-Object { $_.totalSignIns }
$singleFactorData = $sortedData | ForEach-Object { $_.singleFactorSignIns }
$multiFactorData = $sortedData | ForEach-Object { $_.multiFactorSignIns }

$datesString = $dates -join ","
$totalSignInsString = $totalSignInsData -join ","
$singleFactorString = $singleFactorData -join ","
$multiFactorString = $multiFactorData -join ","

$htmlContent += @"
            </tbody>
        </table>
    </div>

    <script>
        // Chart 1: Total Sign-ins over time
        const ctx1 = document.getElementById('signInChart').getContext('2d');
        const signInChart = new Chart(ctx1, {
            type: 'line',
            data: {
                labels: [$datesString],
                datasets: [{
                    label: 'Total Sign-ins',
                    data: [$totalSignInsString],
                    borderColor: 'rgb(75, 192, 192)',
                    backgroundColor: 'rgba(75, 192, 192, 0.2)',
                    tension: 0.1,
                    fill: true
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    title: {
                        display: true,
                        text: 'Total Sign-ins Over Time'
                    }
                },
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });

        // Chart 2: MFA vs Single Factor comparison
        const ctx2 = document.getElementById('mfaChart').getContext('2d');
        const mfaChart = new Chart(ctx2, {
            type: 'line',
            data: {
                labels: [$datesString],
                datasets: [{
                    label: 'Single Factor Sign-ins',
                    data: [$singleFactorString],
                    borderColor: 'rgb(255, 99, 132)',
                    backgroundColor: 'rgba(255, 99, 132, 0.2)',
                    tension: 0.1
                }, {
                    label: 'Multi Factor Sign-ins',
                    data: [$multiFactorString],
                    borderColor: 'rgb(54, 162, 235)',
                    backgroundColor: 'rgba(54, 162, 235, 0.2)',
                    tension: 0.1
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    title: {
                        display: true,
                        text: 'Single Factor vs Multi Factor Sign-ins'
                    }
                },
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });
    </script>
</body>
</html>
"@

# Save HTML file
# Let user select the path
Add-Type -AssemblyName System.Windows.Forms
$saveDialog = New-Object System.Windows.Forms.SaveFileDialog
$saveDialog.Filter = "HTML files (*.html)|*.html|All files (*.*)|*.*"
$saveDialog.Title = "Save Sign-In Report"
$saveDialog.FileName = "SignInReport.html"
$saveDialog.InitialDirectory = $PSScriptRoot

if ($saveDialog.ShowDialog() -eq [System.Windows.Forms.DialogResult]::OK) {
    $htmlPath = $saveDialog.FileName
    $htmlContent | Out-File -FilePath $htmlPath -Encoding UTF8
    
    Write-Host "HTML report generated: $htmlPath" -ForegroundColor Green
    Write-Host "Opening report in default browser..." -ForegroundColor Yellow
    
    # Open the HTML file in default browser
    Start-Process $htmlPath
} else {
    Write-Host "Save cancelled by user." -ForegroundColor Yellow
}
