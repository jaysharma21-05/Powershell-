# 

**Name:** Jay Sharma | **Course:** BCA 6th Sem | **Topic:** Functions & Modules

---

## 🧠 Function kya hota hai?

> Function = Apna khud ka command banana! Ek baar likho — baar baar use karo!

```
Bina Function = Har baar same code likhna 😫
Function      = Ek baar likho, ek word se call karo! 🔥
```

### Analogy

```
Function = Restaurant ka button
           Ek baar dabao — dish ready!

Module   = Poori recipe book
           Saari functions ek jagah!
```

---

## 📦 PART 1 — Basic Function

```powershell
# Sabse simple function
function Say-Hello {
    Write-Output "Hello Jay! PowerShell Master!"
}

# Call karo
Say-Hello
```

### Function with Logic

```powershell
function Get-SystemInfo {
    Write-Output "=== System Info ==="
    Write-Output "Computer: $env:COMPUTERNAME"
    Write-Output "User: $env:USERNAME"
    Write-Output "Date: $(Get-Date -Format 'yyyy-MM-dd HH:mm')"
    Write-Output "Processes: $((Get-Process | Measure-Object).Count)"
    Write-Output "Running Services: $((Get-Service | Where-Object Status -eq Running | Measure-Object).Count)"
}

Get-SystemInfo
```

---

## 📦 PART 2 — Parameters

> Parameter = Function ko input dena!

### Basic Parameter

```powershell
function Get-TopProcesses {
    param(
        $Count = 5    # Default value = 5
    )
    Get-Process | Sort-Object CPU -Descending |
    Select-Object Name, CPU -First $Count
}

Get-TopProcesses           # Default — top 5
Get-TopProcesses -Count 10 # Top 10
Get-TopProcesses -Count 3  # Top 3
```

### Multiple Parameters

```powershell
function Get-FilteredProcesses {
    param(
        $MinRAM = 50MB,
        $Count  = 5
    )
    Get-Process |
    Where-Object WorkingSet -gt $MinRAM |
    Sort-Object WorkingSet -Descending |
    Select-Object Name, WorkingSet -First $Count
}

Get-FilteredProcesses
Get-FilteredProcesses -MinRAM 100MB -Count 3
```

### Mandatory Parameter — Zaroori Input

```powershell
function Get-ServiceStatus {
    param(
        [Parameter(Mandatory=$true)]
        [string]$ServiceName
    )
    $service = Get-Service -Name $ServiceName
    Write-Output "Service: $($service.Name)"
    Write-Output "Status: $($service.Status)"
}

Get-ServiceStatus -ServiceName "Spooler"
Get-ServiceStatus -ServiceName "WinDefend"
```

### Parameter Types

|Type|Example|Kab Use|
|---|---|---|
|Default value|`$Count = 5`|Optional input|
|Multiple|`$MinRAM, $Count`|Multiple inputs|
|Mandatory|`[Parameter(Mandatory=$true)]`|Zaroori input|
|Typed|`[string]$Name`|Type enforce karo|

---

## 📦 PART 3 — Return Values

```powershell
function Get-ProcessCount {
    $count = (Get-Process | Measure-Object).Count
    return $count
}

# Value store karo
$total = Get-ProcessCount
Write-Output "Total processes: $total"

# Condition mein use karo
if((Get-ProcessCount) -gt 200) {
    Write-Output "Bohot zyada processes!"
} else {
    Write-Output "Normal hai!"
}
```

---

## 📦 PART 4 — Real World Functions

### Service Desk Function

```powershell
function Get-HealthReport {
    param($TopCount = 5)

    Write-Output "========================================="
    Write-Output "    HEALTH REPORT — $(Get-Date -Format 'yyyy-MM-dd')"
    Write-Output "========================================="

    Write-Output "`n--- Top $TopCount CPU Processes ---"
    Get-Process | Sort-Object CPU -Descending |
    Select-Object Name, CPU -First $TopCount |
    Format-Table -AutoSize

    Write-Output "`n--- Summary ---"
    Write-Output "Total Processes: $((Get-Process | Measure-Object).Count)"
    Write-Output "Running Services: $((Get-Service | Where-Object Status -eq Running | Measure-Object).Count)"
    Write-Output "Stopped Services: $((Get-Service | Where-Object Status -eq Stopped | Measure-Object).Count)"
}

Get-HealthReport
Get-HealthReport -TopCount 10
```

---

## 📦 PART 5 — Modules

> Module = Functions ka collection — .psm1 file mein!

```
.psm1 = PowerShell Module file (functions)
.psd1 = Module manifest (info — optional)
```

### Module Banana — Step by Step

```powershell
# Step 1 — Folder banao
New-Item -Path "C:\Users\jaysh\Documents\JayModule" -ItemType Directory

# Step 2 — Functions likho
$content = @'
function Get-SystemInfo {
    Write-Output "Computer: $env:COMPUTERNAME"
    Write-Output "User: $env:USERNAME"
}

function Get-TopProcesses {
    param($Count = 5)
    Get-Process | Sort-Object CPU -Descending |
    Select-Object Name, CPU -First $Count
}
'@

Set-Content -Path "C:\Users\jaysh\Documents\JayModule\JayModule.psm1" -Value $content

# Step 3 — Import karo — POORA PATH DO!
Import-Module "C:\Users\jaysh\Documents\JayModule\JayModule.psm1" -Force

# Step 4 — Confirm
Get-Module

# Step 5 — Use karo!
Get-SystemInfo
Get-TopProcesses -Count 3
```

### Module Commands

```powershell
# Loaded modules list
Get-Module

# Module ke functions
Get-Command -Module JayModule

# Module remove karo
Remove-Module JayModule

# Dobara import
Import-Module "path\JayModule.psm1" -Force

# Function add karo module mein
Add-Content -Path "JayModule.psm1" -Value $newFunction

# Reload karo
Remove-Module JayModule
Import-Module "path\JayModule.psm1" -Force
```

---

## 🔥 Tune Aaj Banaya — CyberModule!

```powershell
# CyberModule — 4 functions:

function Get-SuspiciousProcesses {
    param($MinRAM = 200MB)
    Write-Output "=== Suspicious Processes (RAM > $($MinRAM/1MB)MB) ==="
    Get-Process | Where-Object WorkingSet -gt $MinRAM |
    Select-Object Name, Id, CPU, WorkingSet, StartTime |
    Format-Table -AutoSize
}

function Get-StoppedServices {
    Write-Output "=== Stopped Services ==="
    Get-Service | Where-Object Status -eq Stopped |
    Select-Object Name, Status | Format-Table -AutoSize
    Write-Output "Total: $((Get-Service | Where-Object Status -eq Stopped | Measure-Object).Count)"
}

function Get-NetworkConnections {
    Write-Output "=== Active Network Connections ==="
    Get-NetTCPConnection | Where-Object State -eq Established |
    Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State |
    Format-Table -AutoSize
}

function Get-FullAudit {
    param($TopCount = 5)
    Write-Output "========================================="
    Write-Output "   CYBER AUDIT — $(Get-Date -Format 'yyyy-MM-dd HH:mm')"
    Write-Output "   Computer: $env:COMPUTERNAME | User: $env:USERNAME"
    Write-Output "========================================="
    Get-SuspiciousProcesses
    Get-StoppedServices
    Get-NetworkConnections
}
```

---

## ⚠️ Common Mistakes — Yaad Rakhna!

```powershell
# 1. Import mein poora path do!
Import-Module "C:\full\path\module.psm1" -Force  # ✅
Import-Module module.psm1                          # ❌ Error!

# 2. String mein $() use karo property ke liye
"RAM: $($MinRAM/1MB)MB"   # ✅
"RAM: $(MinRAM/1MB)MB"    # ❌ $ missing!

# 3. Spelling check karo
StartTime   # ✅
startime    # ❌ Tune aaj ye galti ki!

# 4. Module update karne ke baad reload karo
Remove-Module JayModule
Import-Module "path\JayModule.psm1" -Force
```

---

## ✅ Topic 7 Summary

|Concept|Key Point|
|---|---|
|Function|`function Naam { code }`|
|Parameter|`param($Var = default)`|
|Mandatory|`[Parameter(Mandatory=$true)]`|
|Return|`return $value`|
|Module|`.psm1` file — functions ka group|
|Import|`Import-Module "full\path\file.psm1" -Force`|
|List|`Get-Module`|
|Functions list|`Get-Command -Module ModuleName`|
|Remove|`Remove-Module ModuleName`|

## Golden Rules

```
1. Function naam = Verb-Noun pattern (Get-Report, Check-RAM)
2. Parameters mein default values do
3. Mandatory parameter = zaroori input
4. Import-Module mein POORA PATH do!
5. Module update ke baad Remove + Import karo
6. $() string mein property access ke liye zaroori
7. Spelling check karo — StartTime, WorkingSet etc.
```

---

## 🚀 Aage Kya Hai

- **Topic 8 — Error Handling & Debugging** ← Next!
    - try-catch-finally
    - Debug tools
    - Logs check karna

---

_Notes by Jay Sharma | PowerShell Masterclass | Topic 7_
