

## 🧠 Pipeline kya hai?

> Pipeline = Factory Assembly Line Ek step ka output → Doosre step ka input!

```
Command1 | Command2 | Command3 | Command4
   ↑            ↑           ↑           ↑
Data la    Filter kar   Sort kar   Display kar

| = Conveyor Belt — output aage bhejta hai
```

**Key Rule:** Pipeline mein **Objects** travel karte hain — sirf text nahi!

---

## 📦 4 Main Pipeline Commands

### 1. Where-Object — Filtering 🔥

```powershell
Get-Process | Where-Object CPU -gt 10
Get-Process | Where-Object Name -eq "chrome"
Get-Process | Where-Object Name -like "*win*"
Get-Process | Where-Object WorkingSet -gt 50MB
Get-Service | Where-Object Status -eq "Running"
Get-Service | Where-Object Status -eq "Stopped"
```

#### Comparison Operators

|Operator|Matlab|Example|
|---|---|---|
|`-eq`|Equal|`-eq "chrome"`|
|`-ne`|Not equal|`-ne "idle"`|
|`-gt`|Greater than|`-gt 100`|
|`-lt`|Less than|`-lt 50`|
|`-ge`|Greater or equal|`-ge 10`|
|`-le`|Less or equal|`-le 5`|
|`-like`|Wildcard match|`-like "*win*"`|
|`-notlike`|No wildcard match|`-notlike "*chrome*"`|

---

### 2. Select-Object — Columns Choose Karo 🔥

```powershell
# Specific columns
Get-Process | Select-Object Name, CPU
Get-Process | Select-Object Name, WorkingSet

# First/Last
Get-Process | Select-Object -First 5
Get-Process | Select-Object -Last 5

# Clean way — ek saath!
Get-Process | Select-Object Name, CPU -First 5

# Unique values
Get-Process | Select-Object Name -Unique
```

#### ⚠️ Optimize Karo!

```powershell
# ❌ Redundant
Get-Process | Select-Object -First 10 | Select-Object Name, CPU

# ✅ Clean
Get-Process | Select-Object Name, CPU -First 10
```

---

### 3. Sort-Object — Sorting 🔥

```powershell
Get-Process | Sort-Object CPU               # Ascending
Get-Process | Sort-Object CPU -Descending   # Descending
Get-Process | Sort-Object Name              # Alphabetical
Get-Process | Sort-Object CPU, WorkingSet -Descending  # Multiple
```

---

### 4. Measure-Object — Count/Calculate

```powershell
Get-Process | Measure-Object               # Count
Get-Process | Measure-Object CPU -Sum -Average -Maximum -Minimum
```

---

## 🔗 Common Pipeline Combinations

```powershell
# Top 5 CPU wale
Get-Process | Sort-Object CPU -Descending | Select-Object Name, CPU -First 5

# Top 5 RAM wale
Get-Process | Sort-Object WorkingSet -Descending | Select-Object Name, WorkingSet -First 5

# Running services — naam se sort
Get-Service | Where-Object Status -eq "Running" | Sort-Object Name | Select-Object Name, Status

# Stopped services
Get-Service | Where-Object Status -eq "Stopped" | Select-Object Name, Status -First 10

# 10MB+ RAM wale — naam se sort
Get-Process | Where-Object WorkingSet -gt 10MB | Sort-Object Name | Select-Object Name, WorkingSet

# svc naam wale
Get-Process | Where-Object Name -like "*svc*" | Select-Object Name, CPU, WorkingSet

# Hidden files — size se sort
Get-ChildItem -Force | Sort-Object Length | Select-Object Name, Length -First 5
```

---

## 🔥 Real World Use

### Service Desk

```powershell
# Slow computer — CPU kaun kha raha hai?
Get-Process | Sort-Object CPU -Descending | Select-Object Name, CPU -First 10

# RAM full — kaun le raha hai?
Get-Process | Sort-Object WorkingSet -Descending | Select-Object Name, WorkingSet -First 10

# Koi service band toh nahi?
Get-Service | Where-Object Status -eq "Stopped" | Select-Object Name, Status

# Printer service chal rahi hai?
Get-Service | Where-Object Name -like "*print*" | Select-Object Name, Status

# System errors check karo
Get-EventLog -LogName System -EntryType Error -Newest 10
```

### Cybersecurity

```powershell
# Suspicious processes — unusual RAM
Get-Process | Where-Object WorkingSet -gt 200MB | Select-Object Name, Id, WorkingSet

# Unknown processes
Get-Process | Where-Object Name -notlike "*win*" | Sort-Object WorkingSet -Descending

# Attacker ne koi service band kiya?
Get-Service | Where-Object Status -eq "Stopped" | Select-Object Name, Status

# Hidden files — malware check
Get-ChildItem -Force -Recurse | Select-Object Name, Length

# Total processes count
Get-Process | Measure-Object
```

---

## 🏆 Golden Rules — Hamesha Yaad Rakhna!

```
1. Pipeline mein Objects travel karte hain — text nahi
2. Where-Object pehle lagao — unnecessary data filter karo
3. Sort-Object baad mein — filtered data pe sort karo
4. Select-Object last mein — sirf jo dikhana ho
5. Select-Object mein -First/-Last saath do — alag mat karo
6. Scripts mein Real Commands use karo — aliases nahi
7. Tab Completion use karo — [TAB] dabao!
8. Error aaye — poora padho, phir fix karo
```

---

# 🗂️ Master Quick Reference

## Commands Summary

|Command|Kaam|Example|
|---|---|---|
|`Get-Command`|Commands dhundho|`Get-Command -Name *service*`|
|`Get-Help`|Manual dekho|`Get-Help Stop-Service -Examples`|
|`Get-Alias`|Shortcuts decode|`Get-Alias ls`|
|`Get-Process`|Running apps|`Get-Process -Name chrome`|
|`Get-Service`|Services|`Get-Service -Name Spooler`|
|`Get-EventLog`|System history|`Get-EventLog -LogName System -Newest 10`|
|`Where-Object`|Filter|`Where-Object CPU -gt 10`|
|`Select-Object`|Columns|`Select-Object Name, CPU -First 5`|
|`Sort-Object`|Sort|`Sort-Object CPU -Descending`|
|`Measure-Object`|Count|`Measure-Object`|
|`Stop-Process`|Process band|`Stop-Process -Name notepad`|
|`Set-Alias`|Custom alias|`Set-Alias -Name jtop -Value Get-Process`|

## Errors Quick Fix

|Error|Matlab|Fix|
|---|---|---|
|`Parameter set cannot be resolved`|Conflicting params|Pipeline use karo|
|`is not recognized`|Command nahi mila|Spelling check, Get-Command se dhundho|
|`Alias is read-only`|Reserved alias naam|`-Force` ya alag naam|
|`cannot find Help files`|Help download nahi|`Update-Help -Force`|

---

## 🚀 Roadmap Progress

|Topic|Status|
|---|---|
|Topic 1 — Shell + Basic Commands|✅ Done|
|Topic 2 — Syntax + Parameters|✅ Done|
|Topic 3 — Pipeline Mastery|✅ Done|
|Topic 4 — Object Understanding|⏭️ Next|
|Topic 5 — Scripting Basics|⏳|
|Topic 6 — File & Data Handling|⏳|
|Topic 7 — Functions & Modules|⏳|
|Topic 8 — Error Handling|⏳|
|Topic 9 — System Administration|⏳|
|Topic 10 — Security & Automation|⏳|

---

_Notes by Jay Sharma | PowerShell Masterclass | Topics 1-3 Complete_