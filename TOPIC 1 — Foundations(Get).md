# ⚡ PowerShell Masterclass — Complete Notes

**Name:** Jay Sharma | **Course:** BCA 6th Sem | **Goal:** Cybersecurity + Service Desk **Topics Covered:** Topic 1 → Topic 2 → Topic 3

---

# 📌

## 🧠 Shell kya hota hai?

> Shell = Computer ka reception counter. Tum text mein baat karte ho, shell computer se karwata hai.

|Shell|Platform|Level|
|---|---|---|
|CMD|Windows only|Purana, limited|
|Bash|Linux/Mac|Powerful|
|**PowerShell**|Windows + Linux + Mac|Most powerful ✅|

**PowerShell ka fark CMD se:**

- CMD = sirf text return karta hai
- PowerShell = **Objects** return karta hai — manipulate kar sakte ho!

---

## 🖥️ PowerShell Open + Setup

```powershell
# Version check
$PSVersionTable.PSVersion
# 5.1 ya upar = Ready!

# PowerShell 7 install (recommended)
winget install Microsoft.PowerShell

# Help files download karo — EK BAAR ZAROOR!
Update-Help -Force
# Ya errors ignore karte hue:
Update-Help -Force -ErrorAction SilentlyContinue
```

**Open karne ke tarike:**

```
Start Menu → PowerShell → Right-click → Run as Administrator
Win + R → powershell → Enter
```

---

## 📦 Get-Command — PowerShell ka Search Engine

> **Matlab:** "Kya kya kar sakta hoon?" — Swiggy jaise commands dhundho

```powershell
# Saare commands
Get-Command

# Naam se dhundho — wildcard * use karo
Get-Command -Name *service*
Get-Command -Name Get-*
Get-Command -Name *process*

# Type se filter
Get-Command -CommandType Cmdlet      # Official commands
Get-Command -CommandType Alias       # Shortcuts
Get-Command -CommandType Function    # Custom functions
Get-Command -CommandType Application # .exe programs

# Module se filter
Get-Command -Module Microsoft.PowerShell.Management
Get-Command -Module NetTCPIP

# Verb se filter
Get-Command -Verb Get        # Sirf Get- wale
Get-Command -Verb Stop       # Sirf Stop- wale
Get-Command -Verb Start      # Sirf Start- wale

# Noun se filter
Get-Command -Noun Process    # Sirf -Process wale
Get-Command -Noun Service    # Sirf -Service wale

# Syntax shortcut
Get-Command -Name Get-Process -Syntax

# Pipeline ke saath
Get-Command | Measure-Object                          # Total count
Get-Command -Verb Stop | Where-Object CommandType -eq Cmdlet
Get-Command | Sort-Object Name | Select-Object -First 20
```

### Verb-Noun Pattern

```
Get  -  Process
↑          ↑
Verb      Noun

Har PowerShell command = Verb + Noun
Get-Process | Stop-Service | Start-Job | Remove-Item
```

### All Parameters

|Parameter|Kaam|
|---|---|
|`-Name`|Naam se dhundho (wildcard * ok)|
|`-CommandType`|Type se filter|
|`-Module`|Module se filter|
|`-Verb`|Verb se filter|
|`-Noun`|Noun se filter|
|`-Syntax`|Quick syntax|
|`-TotalCount`|Sirf N results|

---

## 📖 Get-Help — Built-in Manual

> **Matlab:** Har command ki manual booklet — offline bhi!

```powershell
# Basic info
Get-Help Get-Process

# 🔥 Sabse useful — examples
Get-Help Get-Process -Examples

# Poora manual
Get-Help Get-Process -Full

# Medium detail
Get-Help Get-Process -Detailed

# Browser mein kholo
Get-Help Get-Process -Online

# Saare parameters
Get-Help Get-Process -Parameter *

# Ek specific parameter
Get-Help Get-Process -Parameter Name

# Concept topics — Hidden Gem!
Get-Help about_*              # Saare topics list
Get-Help about_Pipelines      # Pipeline concept
Get-Help about_Variables      # Variables concept
Get-Help about_Functions      # Functions concept
```

### Flags Quick Reference

|Flag|Kab Use Karein|
|---|---|
|`-Examples`|🔥 Roz — copy paste karo|
|`-Full`|Deep dive|
|`-Parameter *`|Saare parameters|
|`-Online`|Latest official docs|
|`about_*`|Concepts samajhne|

---

## 🔄 Get-Alias — Shortcut Dictionary

> **Matlab:** PowerShell ka short form dictionary — ls ka matlab kya hai?

```powershell
# Saare aliases
Get-Alias

# Ek alias decode karo
Get-Alias ls
Get-Alias cd
Get-Alias kill

# Multiple ek saath
Get-Alias ls, cd, cls, cat, pwd

# Real command ke aliases jaano
Get-Alias -Definition Get-ChildItem
Get-Alias -Definition Get-Help

# Filter by naam
Get-Alias s*     # s se shuru wale
Get-Alias g*     # g se shuru wale

# Custom alias banao
Set-Alias -Name jtop -Value Get-Process
Set-Alias -Name x -Value y -Force    # Read-only overwrite
```

### Common Aliases — Yaad Kar Lo!

|Alias|Real Command|Category|
|---|---|---|
|`ls`|`Get-ChildItem`|File System|
|`cd`|`Set-Location`|File System|
|`pwd`|`Get-Location`|File System|
|`cp`|`Copy-Item`|File System|
|`mv`|`Move-Item`|File System|
|`rm`|`Remove-Item`|File System|
|`cat`|`Get-Content`|File System|
|`ps`|`Get-Process`|System|
|`kill`|`Stop-Process`|System|
|`cls`|`Clear-Host`|System|
|`echo`|`Write-Output`|System|
|`man`|`Get-Help`|Help|
|`gcm`|`Get-Command`|Help|
|`?`|`Where-Object`|Pipeline|
|`%`|`ForEach-Object`|Pipeline|
|`sort`|`Sort-Object`|Pipeline|
|`select`|`Select-Object`|Pipeline|
|`measure`|`Measure-Object`|Pipeline|
|`ft`|`Format-Table`|Formatting|

---

## 🔍 Get-Process — Task Manager PowerShell Version

```powershell
Get-Process                              # Saare processes
Get-Process -Name chrome                 # Specific naam
Get-Process -Name chrome,notepad         # Multiple
Get-Process -Name *win*                  # Wildcard
Get-Process -Id 1234                     # ID se

# Sorting
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 5

# Process band karo
Get-Process -Name notepad | Stop-Process
```

---

## 📋 Get-EventLog — System CCTV

```powershell
Get-EventLog -LogName System -EntryType Error -Newest 10
Get-EventLog -LogName Application -Newest 20
Get-EventLog -LogName System -EntryType Warning -Newest 5
```

---

