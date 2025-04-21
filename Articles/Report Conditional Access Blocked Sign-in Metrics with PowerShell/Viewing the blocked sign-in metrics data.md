Return all the blocked sign-ins between the 1st of April 2025 and the 21st of April 2025, aggregated by 30-minute intervals.

```Powershell
$endDate = (Get-Date).ToString("yyyy-MM-ddTHH:mm:ssZ")
$startDate = (Get-Date).AddDays(-30).ToString("yyyy-MM-ddTHH:mm:ssZ")
$interval = "30"

$Uri = "/beta/reports/serviceActivity/getMetricsForConditionalAccessBlockedSignIn(inclusiveIntervalStartDateTime=$startDate,exclusiveIntervalEndDateTime=$endDate,aggregationIntervalInMinutes=$interval)"
$response = Invoke-MgGraphRequest -Method GET -Uri $Uri | Select -Expand value
```

Output snippet
```PowerShell
{
    "intervalStartDateTime": "2025-04-01T00:30:00Z",
    "value": 0
},
{
    "intervalStartDateTime": "2025-04-01T01:00:00Z",
    "value": 0
},
{
    "intervalStartDateTime": "2025-04-01T01:30:00Z",
    "value": 0
}
```
