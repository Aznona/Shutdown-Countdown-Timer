# Shutdown-Countdown-Timer
# The UI for command shutdown -s -t where value of -t is undefined and user may choose it.

# Create a form with a numeric up-down control to allow the user to specify the value for -t

$form = New-Object System.Windows.Forms.Form
$form.Text = "Shutdown Timer"
$form.Width = 300
$form.Height = 120
$form.FormBorderStyle = [System.Windows.Forms.FormBorderStyle]::FixedDialog
$form.MaximizeBox = $false
$form.StartPosition = [System.Windows.Forms.FormStartPosition]::CenterScreen

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10, 15)
$label.Size = New-Object System.Drawing.Size(260, 20)
$label.Text = "Specify the value for the -t parameter:"
$form.Controls.Add($label)

$numericUpDown = New-Object System.Windows.Forms.NumericUpDown
$numericUpDown.Location = New-Object System.Drawing.Point(10, 40)
$numericUpDown.Size = New-Object System.Drawing.Size(260, 20)
$numericUpDown.Minimum = 1
$numericUpDown.Maximum = 3600
$numericUpDown.Value = 30
$form.Controls.Add($numericUpDown)

$okButton = New-Object System.Windows.Forms.Button
$okButton.Location = New-Object System.Drawing.Point(110, 75)
$okButton.Size = New-Object System.Drawing.Size(80, 25)
$okButton.Text = "OK"
$okButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $okButton
$form.Controls.Add($okButton)

$cancelButton = New-Object System.Windows.Forms.Button
$cancelButton.Location = New-Object System.Drawing.Point(200, 75)
$cancelButton.Size = New-Object System.Drawing.Size(80, 25)
$cancelButton.Text = "Cancel"
$cancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $cancelButton
$form.Controls.Add($cancelButton)

# Show the form and execute the shutdown command with the specified parameters if the user clicked OK
if ($form.ShowDialog() -eq [System.Windows.Forms.DialogResult]::OK) {
    $tValue = $numericUpDown.Value
    $shutdownTime = $host.UI.PromptForTime("Select the time to shutdown:", "Shutdown Time", (Get-Date).AddSeconds($tValue), [System.Windows.Forms.MessageBoxButtons]::OKCancel)

    # Execute the shutdown command with the specified parameters
    if ($shutdownTime -eq "OK") {
        & shutdown /s /t $tValue
    }
}
