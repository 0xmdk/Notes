---
title: 🐱‍💻 OxMdk
---

### PowerShell

###### OpenVPN Example:
* Step 1: `Place body of code on AWS:`
```ps
## Check for Current Installed Client
$ovpnapp = Get-Package -Provider Programs -IncludeWindowsInstaller -name "OpenVPN*" -ErrorAction SilentlyContinue

## if installed remove
if($ovpnapp.Version -eq $null){
    write-host "Installing OVPN Connect V3"
}elseif($ovpnapp.Version -eq "3.3.3"){
        write-host "You are currently on the latest Openvpn Connect version: v3.3.3";exit 0
}elseif($ovpnapp.Version -le "3.3.2"){
        Uninstall-Package -Name $ovpnapp.Name -Force -Confirm:$false
}else{
    exit 1
}

## Test Path
if(!(Test-Path -Path $HOME\Downloads\Ovpn-V3-install -PathType Container) -eq 'True'){
New-Item -Path $HOME\Downloads\Ovpn-V3-install -ItemType Directory
}

$ProgressPreference = 'SilentlyContinue'
## Download File
if ((Test-Path -Path $HOME\Downloads\Ovpn-V3-install\openvpn-connect-v3-windows.msi -PathType Leaf) -eq 'True'){
Remove-Item $HOME\Downloads\Ovpn-V3-install\openvpn-connect-v3-windows.msi -Force -Confirm:$false
}

Invoke-WebRequest -Uri "https://openvpn.net/downloads/openvpn-connect-v3-windows.msi" -OutFile $HOME\Downloads\Ovpn-V3-install\openvpn-connect-v3-windows.msi


## Create Bundle File
Out-File $HOME\Downloads\Ovpn-V3-install\bundled.ovpn -Encoding utf8

## Create Certificate File Contents
$NLB = "`r`n"
$ovpnfile = "client" + $NLB + "dev tun" + $NLB + "proto udp" + $NLB + "remote X.X.X.X 1194" + $NLB + "remote X.X.X.X 1194" + $NLB + "remote-random" + $NLB + "resolv-retry infinite" + $NLB + "nobind" + $NLB  +"user nobody" + $NLB  + "group nobody" + $NLB  + "persist-key"  + $NLB  + "persist-tun" + $NLB  + "<ca>" + $NLB  + "-----BEGIN CERTIFICATE-----" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+******************* `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "********************************************+*******************" `
+ $NLB + "******************************=" `
+ $NLB + "-----END CERTIFICATE-----" `
+ $NLB + "</ca>" + $NLB + "client-cert-not-required" + $NLB + "cipher AES-256-CBC" + $NLB + "verb 3" + $NLB + "verb 3" + $NLB + "auth-user-pass" + $NLB + "auth-retry interact" `
+ $NLB + "auth-nocache" + $NLB + "reneg-sec 23000"

## Add Contents to bundled.ovpn file
Add-Content -Path $HOME\Downloads\Ovpn-V3-install\bundled.ovpn -Value $ovpnfile

## Install OpenVPN 3 with config file
msiexec.exe /i $HOME\Downloads\Ovpn-V3-install\openvpn-connect-v3-windows.msi /qb

## Check for Outdated Chrome:
$chromeapp = Get-Package -Provider Programs -IncludeWindowsInstaller -name "Google Chrome" -ErrorAction SilentlyContinue

## if installed remove

foreach($chrominstall in $chromeapp){
    if($chrominstall.Version -le "96.0.4664.44"){
    Write-Host "Your Chrome Browser Needs Updating!!"
    }else{
    $chrominstall
    write-host "Your Chrome is Updated - Just Checking! `n "
    pause
    }
}
```

* Step 2: `Have Intune execude the code upon restart:`
```ps
powershell.exe -nop -c "iex(New-Object System.Net.Webclient).DownloadString('http://X.X.X.X:8089/OVPN/installovpn.ps1')"
```
