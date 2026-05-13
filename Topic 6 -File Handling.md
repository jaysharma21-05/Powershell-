

---

## 🧠 File Handling kya hai?

> Script = Recipe Card — Files = Ingredients store karne ki jagah! PowerShell se files padho, likho, copy karo, delete karo — sab automatic!

```
Get-Content    = File padhna
Set-Content    = File mein likhna (replace)
Add-Content    = File mein add karna (append)
New-Item       = Nai file/folder banana
Copy-Item      = Copy karna
Move-Item      = Move karna
Remove-Item    = Delete karna
Export-Csv     = Data CSV mein save karna
Import-Csv     = CSV se data padhna
ConvertTo-Json = Object ko JSON mein banana
ConvertFrom-Json = JSON se object banana
```

---

## 📦 PART 1 — Files & Folders Manage Karna

### Files List Karo

```powershell
# Current folder
Get-ChildItem

# Specific folder
Get-ChildItem -Path C:\Users\jaysh\Documents

# Hidden files bhi
Get-ChildItem -Force

# Subfolders bhi
Get-ChildItem -Recurse

# Sirf .txt files
Get-ChildItem -Filter *.txt

# Sirf .log files — subfolders mein bhi
Get-ChildItem -Recurse -Filter *.log

# Size se sort karo
Get-ChildItem | Sort-Object Length -Descending

# Sirf files — folders nahi
Get-ChildItem | Where-Object PSIsContainer -eq $false

# Sirf folders — files nahi
Get-ChildItem | Where-Object PSIsContainer -eq $true
```

### Nai File/Folder Banana

```powershell
# ✅ File banana
New-Item -Path "C:\Users\jaysh\Desktop\test.txt" -ItemType File

# ✅ Folder banana
New-Item -Path "C:\Users\jaysh\Desktop\MyFolder" -ItemType Directory

# ⚠️ Yaad rakhna!
# ItemType File    = file banti hai
# ItemType Directory = folder banta hai
```

### Copy, Move, Delete

```powershell
# Copy karo
Copy-Item -Path "C:\test.txt" -Destination "C:\Backup\test.txt"

# Move karo
Move-Item -Path "C:\test.txt" -Destination "C:\Archive\test.txt"

# Delete karo — file
Remove-Item -Path "C:\test.txt"

# Delete karo — folder aur andar sab
Remove-Item -Path "C:\OldFolder" -Recurse

# Rename karo
Rename-Item -Path "C:\test.txt" -NewName "new_test.txt"
```

---

## 📦 PART 2 — File Read/Write

### File Padhna — Get-Content

```powershell
# Poori file padho
Get-Content -Path "test.txt"

# Pehli 10 lines
Get-Content -Path "test.txt" | Select-Object -First 10

# Last 10 lines
Get-Content -Path "test.txt" | Select-Object -Last 10

# ERROR wali lines dhundho
Get-Content -Path "log.txt" | Where-Object {$_ -like "*ERROR*"}

# Specific word wali lines
Get-Content -Path "log.txt" | Where-Object {$_ -like "*WARNING*"}

# Line count karo
Get-Content -Path "test.txt" | Measure-Object
```

### File Likhna

```powershell
# Set-Content — purana content replace ho jaata hai!
Set-Content -Path "test.txt" -Value "Hello Jay!"

# Add-Content — purana rehta hai, naya add hota hai
Add-Content -Path "test.txt" -Value "Ye nai line hai!"

# Multiple lines likhna
$lines = @(
    "Line 1 — PowerShell",
    "Line 2 — Cybersecurity",
    "Line 3 — Service Desk"
)
Set-Content -Path "test.txt" -Value $lines

# Date ke saath likhna
Add-Content -Path "log.txt" -Value "$(Get-Date) — Task Complete"
```

### ⚠️ Set-Content vs Add-Content

```
Set-Content = Overwrite — purana sab delete, naya likho
Add-Content = Append   — purana rehta hai, neeche add hota hai

Jaise:
Set-Content = Whiteboard saaf karke naya likhna
Add-Content = Whiteboard pe neeche aur likhna
```

---

## 📦 PART 3 — CSV Handling 🔥

> CSV = Excel jaisi file — rows aur columns — data store karne ke liye! Service Desk mein reports banana, Cybersecurity mein evidence save karna!

### CSV Export — Data Save Karo

```powershell
# Processes CSV mein save karo
Get-Process | Select-Object Name, CPU, WorkingSet |
Export-Csv -Path "processes.csv" -NoTypeInformation

# Services CSV mein save karo
Get-Service | Select-Object Name, Status |
Export-Csv -Path "services.csv" -NoTypeInformation

# ⚠️ -NoTypeInformation — zaroori hai!
# Bina iske pehli line mein garbage aata hai
```

### CSV Import — CSV Padho

```powershell
# CSV file padho
$data = Import-Csv -Path "processes.csv"

# Saara data dekho
$data

# Filter karo
$data | Where-Object Name -like "*chrome*"

# Sort karo
$data | Sort-Object CPU -Descending

# Count karo
$data | Measure-Object
```

### CSV Real World

```powershell
# Daily report — date ke saath file naam
$date = Get-Date -Format "yyyy-MM-dd"
Get-Service | Where-Object Status -eq Running |
Select-Object Name, Status |
Export-Csv -Path "services_$date.csv" -NoTypeInformation

# Top 10 CPU processes save karo
Get-Process | Sort-Object CPU -Descending |
Select-Object Name, CPU, WorkingSet -First 10 |
Export-Csv -Path "top_processes.csv" -NoTypeInformation
```

---

## 📦 PART 4 — JSON Handling 🔥

> JSON = Settings/Config files ka format — APIs mein bhi use hota hai! Cybersecurity mein system info save karna!

### JSON Export

```powershell
# Hashtable banao
$person = @{
    Name = "Jay Sharma"
    Age  = 21
    City = "Mumbai"
}

# JSON mein dekho
$person | ConvertTo-Json

# File mein save karo
$person | ConvertTo-Json | Set-Content -Path "person.json"

# System info JSON mein save karo
$sysInfo = @{
    Hostname = $env:COMPUTERNAME
    Username = $env:USERNAME
    Date     = Get-Date -Format "yyyy-MM-dd HH:mm"
}
$sysInfo | ConvertTo-Json | Set-Content "system_info.json"
```

### JSON Import

```powershell
# JSON file padho
$data = Get-Content -Path "person.json" | ConvertFrom-Json

# Properties access karo
$data.Name
$data.Age
$data.City
```

---

## 🔥 Real World Use

### Service Desk

```powershell
# Log file mein errors dhundho
Get-Content -Path "C:\Logs\app.log" |
Where-Object {$_ -like "*ERROR*"} |
Set-Content -Path "C:\Logs\errors_only.txt"

# Daily report CSV mein
$date = Get-Date -Format "yyyy-MM-dd"
Get-Service | Where-Object Status -eq Running |
Select-Object Name, Status |
Export-Csv -Path "C:\Reports\services_$date.csv" -NoTypeInformation

# Purani files delete karo — 30 din se zyada purani
Get-ChildItem -Path "C:\Logs" -Filter *.log |
Where-Object LastWriteTime -lt (Get-Date).AddDays(-30) |
Remove-Item
```

### Cybersecurity

```powershell
# Suspicious processes evidence save karo
Get-Process | Where-Object WorkingSet -gt 200MB |
Select-Object Name, Id, CPU, WorkingSet, StartTime |
Export-Csv -Path "C:\Evidence\suspicious_$(Get-Date -Format 'yyyyMMdd').csv" -NoTypeInformation

# System snapshot JSON mein
$snapshot = @{
    Hostname   = $env:COMPUTERNAME
    Username   = $env:USERNAME
    Date       = Get-Date -Format "yyyy-MM-dd HH:mm"
    Processes  = (Get-Process | Measure-Object).Count
    Services   = (Get-Service | Where-Object Status -eq Running | Measure-Object).Count
}
$snapshot | ConvertTo-Json | Set-Content "snapshot.json"

# Hidden files dhundho — malware check
Get-ChildItem -Force -Recurse |
Where-Object Name -like ".*" |
Select-Object Name, FullName, Length |
Export-Csv "hidden_files.csv" -NoTypeInformation
```

---

## ✅ Commands Quick Reference

|Command|Kaam|Example|
|---|---|---|
|`Get-ChildItem`|Files list karo|`Get-ChildItem -Filter *.txt`|
|`New-Item`|File/Folder banao|`New-Item -ItemType File`|
|`Copy-Item`|Copy karo|`Copy-Item -Path x -Destination y`|
|`Move-Item`|Move karo|`Move-Item -Path x -Destination y`|
|`Remove-Item`|Delete karo|`Remove-Item -Recurse`|
|`Get-Content`|File padho|`Get-Content log.txt`|
|`Set-Content`|File likho (replace)|`Set-Content -Value "text"`|
|`Add-Content`|File mein add karo|`Add-Content -Value "text"`|
|`Export-Csv`|CSV save karo|`Export-Csv -NoTypeInformation`|
|`Import-Csv`|CSV padho|`Import-Csv -Path file.csv`|
|`ConvertTo-Json`|JSON banao|`$obj \| ConvertTo-Json`|
|`ConvertFrom-Json`|JSON padho|`Get-Content \| ConvertFrom-Json`|

## Golden Rules

```
1. New-Item -ItemType File    = file
   New-Item -ItemType Directory = folder
2. Set-Content = replace (overwrite)
   Add-Content = append (add karo)
3. Export-Csv mein -NoTypeInformation hamesha!
4. Import-Csv ke baad Where-Object se filter karo
5. Date file naam mein — Get-Date -Format "yyyy-MM-dd"
6. WorkingSet -gt 50MB — MB likhna zaroori! (sirf 50 nahi)
7. Remove-Item folder ke liye -Recurse zaroori
```

---

## 🚀 Aage Kya Hai

- **Topic 7 — Functions & Modules** ← Next!
    - Functions banana
    - Parameters pass karna
    - Modules create/use karna

---

_Notes by Jay Sharma | PowerShell Masterclass | Topic 6_