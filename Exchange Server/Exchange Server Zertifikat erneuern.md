Prüfen ob Zertifikat abgelaufen ist
```powershell
(Get-AuthConfig).CurrentCertificateThumbprint `
    | Get-ExchangeCertificate `
    | Format-List
```

Zertifikat erstellen
```powershell
New-ExchangeCertificate -KeySize 2048 `
    -PrivateKeyExportable $true `
    -SubjectName "cn=Microsoft Exchange Server Auth Certificate" `
    -FriendlyName "Microsoft Exchange Server Auth Certificate" `
    -DomainName @()
```

Zertifikat updaten
```powershell
Set-AuthConfig -NewCertificateThumbprint 'THUMBPRINT COMES HERE' -NewCertificateEffectiveDate (Get-Date)
Set-AuthConfig -PublishCertificate
Set-AuthConfig -ClearPreviousCertificate
```

IIS neu starten
```powershell
Restart-WebAppPool MSExchangeOWAAppPool
Restart-WebAppPool MSExchangeECPAppPool
# Alternative: iisreset
```