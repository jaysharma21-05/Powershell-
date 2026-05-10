

---

## 🧠 Object kya hota hai?

> Har cheez PowerShell mein ek Object hai — Process, Service, String, File — sab!

```
Object = Properties + Methods

Property = Value — sirf DEKH sakte ho
           (.Name, .CPU, .WorkingSet, .StartTime)

Method   = Action — KUCH KARTA HAI
           (.Kill(), .Refresh())
```

### Analogy — Smartphone

```
Smartphone Object:
  Battery        ← Property (value)
  ScreenSize     ← Property (value)
  Call()         ← Method (action)
  TakePhoto()    ← Method (action)

Process Object:
  .Name          ← Property
  .CPU           ← Property
  .WorkingSet    ← Property (RAM)
  .StartTime     ← Property
  .Kill()        ← Method
  .Refresh()     ← Method
```

---

## 🔍 Get-Member — Object ka X-Ray Machine

> Kisi bhi object ke andar dekho — saari Properties + Methods ek second mein!

```powershell
# Basic X-Ray
Get-Process | Get-Member

# Sirf Properties
Get-Process | Get-Member -MemberType Property

# Sirf Methods
Get-Process | Get-Member -MemberType Method

# Ek specific member dhundho
Get-Process | Get-Member -Name "Name"

# Count karo — total members
Get-Process | Get-Member | Measure-Object

# Properties count
Get-Process | Get-Member -MemberType Property | Measure-Object

# Methods count
Get-Process | Get-Member -MemberType Method | Measure-Object
```

### Get-Process Object Stats — Aaj Discover Kiya!

```
Total Members  = 94
Properties     = 52
Methods        = 19
Baaki          = AliasProperty, CodeProperty, Event, NoteProperty,
                 ScriptProperty, PropertySet
```

---

## 📦 Saare MemberTypes — Explained

|MemberType|Matlab|Example|
|---|---|---|
|`Property`|Direct value stored|`WorkingSet`, `CPU`, `Id`|
|`AliasProperty`|Kisi property ka nickname|`Name` → `ProcessName` ka alias|
|`CodeProperty`|C# se calculated value|System special values|
|`NoteProperty`|Manually add ki gayi|Extra info|
|`ScriptProperty`|PS script se calculated|Dynamic value|
|`PropertySet`|Properties ka group|`PSResources` = CPU+RAM|
|`Method`|Action karta hai|`.Kill()`, `.Refresh()`|
|`Event`|Trigger hota hai kuch hone pe|Process exit event|

---

## ⚡ Properties Access Karna

```powershell
# () mein command, phir .Property
(Get-Process -Name explorer).Name
(Get-Process -Name explorer).CPU
(Get-Process -Name explorer).WorkingSet
(Get-Process -Name explorer).Id
(Get-Process -Name explorer).StartTime

# Variable mein store karo — clean tarika
$p = Get-Process -Name explorer
$p.Name
$p.CPU
$p.WorkingSet
$p.Id
$p.StartTime      # Sunday, May 10, 2026 11:19:27 AM
```

### Interesting Discovery!

```powershell
# Name = AliasProperty hai — ProcessName ka alias!
# Ye dono same hain:
(Get-Process -Name chrome).Name
(Get-Process -Name chrome).ProcessName
```

---

## ⚡ Methods Use Karna

```powershell
# Variable mein store karo
$p = Get-Process -Name notepad

# Method call karo — () zaroori hai!
$p.Kill()      # Process band karo
$p.Refresh()   # Live data update karo

# Method ke baad () = action call karna
# Property ke baad () nahi = value dekhna
```

### Property vs Method — Fark Yaad Rakhna!

```
$p.CPU        ← Property — value return karta hai
$p.Kill()     ← Method   — action karta hai, () zaroori!
```

---

## 🎨 Output Formatting

```powershell
# Table format — default, clean
Get-Process | Format-Table Name, CPU, WorkingSet

# List format — ek property ek line mein
Get-Process -Name explorer | Format-List

# List format — SAARI properties (scroll karo!)
Get-Process -Name explorer | Format-List *

# Wide format — sirf names, columns mein
Get-Process | Format-Wide Name
```

### Format Commands Quick Reference

|Command|Kab Use|Output|
|---|---|---|
|`Format-Table`|Multiple objects compare|Table|
|`Format-List *`|Ek object ki saari details|List|
|`Format-Wide`|Sirf ek property, wide|Wide columns|

### ⚠️ Important!

```
Format-List * bohot lamba output deta hai — scroll karo!
Explorer pe Format-List * = 52 properties dikhata hai
```

---

## 🔥 Get-Member — Doosre Objects pe bhi!

```powershell
# Service object
Get-Service | Get-Member

# EventLog object
Get-EventLog -LogName System -Newest 1 | Get-Member

# String object — haan string bhi object hai!
"Hello Jay" | Get-Member
```

---

## 🔥 Real World Use

### Service Desk

```powershell
# Process kab se chal rahi hai?
(Get-Process -Name explorer).StartTime

# Saare processes ki start time — suspicious check
Get-Process | Select-Object Name, StartTime | Sort-Object StartTime -Descending

# Process ka ID jaano — phir forcefully band karo
$p = Get-Process -Name notepad
$p.Id          # ID dekho
$p.Kill()      # Band karo
```

### Cybersecurity

```powershell
# Suspicious process ki StartTime check karo
(Get-Process -Name suspicious).StartTime

# Recent start hone wale processes — attack ke baad
Get-Process | Select-Object Name, StartTime | Sort-Object StartTime -Descending | Select-Object -First 10

# Process ki full detail — malware analysis
Get-Process -Name suspicious | Format-List *
```

---

## ✅ Topic 4 Summary

|Concept|Key Point|
|---|---|
|Object|Properties + Methods ka collection|
|Property|Value — `.Name`, `.CPU`, `.StartTime`|
|Method|Action — `.Kill()`, `.Refresh()`|
|Get-Member|Object ka X-Ray — saab dekho|
|Format-List *|Saari properties — scroll karo!|
|Format-Table|Clean table output|
|`$variable`|Object store karo — phir use karo|

## Golden Rules

```
1. Har cheez PowerShell mein Object hai
2. Property = value (no brackets)
3. Method = action (brackets zaroori!)
4. Get-Member = naya command seekha? X-Ray karo!
5. Format-List * = full detail chahiye
6. $p = Get-Process... → variable mein store = clean code
```

---

## 🚀 Aage Kya Hai

- **Topic 5 — Scripting Basics** ← Next!
    - Variables
    - Data types
    - Conditions (if, switch)
    - Loops (for, foreach, while)

---

_Notes by Jay Sharma | PowerShell Masterclass | Topic 4_