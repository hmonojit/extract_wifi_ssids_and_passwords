$wifiProfiles = netsh wlan show profiles | Select-String "All User Profile" | ForEach-Object { $_.ToString().Split(":")[1].Trim() }

foreach ($profile in $wifiProfiles) {
    $profileDetails = netsh wlan show profile name=$profile key=clear
    $ssid = $profileDetails | Select-String "SSID name" | ForEach-Object { $_.ToString().Split(":")[1].Trim() }
    $password = $profileDetails | Select-String "Key Content" | ForEach-Object { $_.ToString().Split(":")[1].Trim() }

    if ($ssid -ne "" -and $password -ne "") {
        Add-Content -Path "wifi_passwords.txt" -Value ("SSID: $ssid")
        Add-Content -Path "wifi_passwords.txt" -Value ("Password: $password")
        Add-Content -Path "wifi_passwords.txt" -Value ""
    }
}

Write-Host "Done! Saved Wi-Fi SSIDs and passwords to wifi_passwords.txt"
