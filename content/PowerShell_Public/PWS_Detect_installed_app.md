---
title: 🐱‍💻 OxMdk
---
###### Mimecast Example:
```ps
$MCOUT = Get-WmiObject -Class Win32_product|Where-Object {$_.Name -match "Mimecast for Outlook 64-bit"}
$outlook = Get-Process -name OUTLOOK -ErrorAction SilentlyContinue

if($MCOUT.Version -eq $null){
    Write-Host "Mimecast for Outlook not installed"; exit 1
}elseif($MCOUT.Version -eq "7.10.0.72"){
        Write-Host $MCOUT.Name "Version:" $MCOUT.Version Installed; exit 0
}else{Write-Host $MCOUT.Name "Current Installed Version:" $MCOUT.Version "but latest is V7.10.0.72"; Stop-Process -Name OUTLOOK; exit 1
}
```
