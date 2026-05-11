# 

**Name:** Jay Sharma | **Course:** BCA 6th Sem | **Topic:** Scripting Basics

---

## 🧠 Script kya hota hai?

> Script = Recipe Card — ek baar likho, baar baar use karo! Ek file mein saare commands — ek baar run karo, sab kaam ho jaaye!

```
Without Script: Ek ek command manually
With Script:    Ek file run karo — sab automatic!
```

---

## 📦 PART 1 — Variables

> Variable = Data store karne ka dabba — `$` se shuru hota hai hamesha!

```powershell
# Variable banao
$naam = "Jay Sharma"
$age = 21
$isStudent = $true

# Use karo
Write-Output $naam
Write-Output "Mera naam $naam hai aur main $age saal ka hoon"
```

### Data Types

```powershell
# String — text
$name = "Jay"
$city = "Mumbai"

# Integer — number
$age = 21
$count = 100

# Boolean — true/false
$isAdmin = $true
$isStudent = $false

# Array — list of values
$fruits = @("Apple", "Mango", "Banana")
$numbers = @(1, 2, 3, 4, 5)

# Hashtable — key-value pairs
$person = @{
    Name = "Jay"
    Age  = 21
    City = "Mumbai"
}

# Type check karo
$name.GetType()      # String
$age.GetType()       # Int32
$isAdmin.GetType()   # Boolean
```

### Data Types Quick Reference

|Type|Example|GetType()|
|---|---|---|
|String|`$name = "Jay"`|String|
|Integer|`$age = 21`|Int32|
|Boolean|`$isAdmin = $true`|Boolean|
|Array|`$fruits = @("a","b")`|Object[]|
|Hashtable|`$p = @{Name="Jay"}`|Hashtable|

---

## 📦 PART 2 — Conditions

> If-Else = "Agar ye ho toh ye karo, warna ye karo"

### Basic If-Else

```powershell
$age = 21

if ($age -ge 18) {
    Write-Output "Tu adult hai!"
} else {
    Write-Output "Tu minor hai!"
}
```

### If — ElseIf — Else

```powershell
$marks = 75

if ($marks -ge 90) {
    Write-Output "Grade: A — Excellent!"
} elseif ($marks -ge 75) {
    Write-Output "Grade: B — Good!"
} elseif ($marks -ge 60) {
    Write-Output "Grade: C — Average"
} else {
    Write-Output "Grade: F — Fail"
}
```

### Switch — Multiple conditions clean tarike se

```powershell
$day = "Monday"

switch ($day) {
    "Monday"   { Write-Output "Aaj kaam karo!" }
    "Friday"   { Write-Output "Weekend aane wala hai!" }
    "Saturday" { Write-Output "Rest karo bhai!" }
    "Sunday"   { Write-Output "Kal se school!" }
    default    { Write-Output "Normal din hai" }
}
```

### Real World — RAM Check

```powershell
# Tune khud likha — Challenge 2! ✅
$ram = 85

if ($ram -gt 80) {
    Write-Output "RAM Critical!"
} else {
    Write-Output "RAM OK"
}
```

---

## 📦 PART 3 — Loops

> Loop = Ek kaam baar baar karo — manually repeat mat karo!

### For Loop — Fixed times repeat karo

```powershell
# Structure yaad rakhna:
for ($i = 1 ; $i -le 10 ; $i++)
#      ↑           ↑          ↑
#   Start      Condition   Increment

# 1 se 5 tak
for ($i = 1; $i -le 5; $i++) {
    Write-Output "Number: $i"
}

# Even numbers — tune khud likha! ✅
for ($i = 1; $i -le 10; $i++) {
    if ($i % 2 -eq 0) {
        Write-Output $i    # 2,4,6,8,10
    }
}
```

### ⚠️ Common Mistake — Tune Khud Fix Kiya!

```powershell
# ❌ Galat — -ge matlab bada ya equal — loop nahi chalega!
for ($i = 1; $i -ge 10; $i++)

# ✅ Sahi — -le matlab chhota ya equal
for ($i = 1; $i -le 10; $i++)
```

### Foreach Loop — List ke har item pe

```powershell
# Array ke har item pe
$fruits = @("Apple", "Mango", "Banana")

foreach ($fruit in $fruits) {
    Write-Output "Fruit: $fruit"
}

# Processes pe foreach
$processes = Get-Process | Select-Object -First 5
foreach ($p in $processes) {
    Write-Output "Process: $($p.Name) — CPU: $($p.CPU)"
}
```

### While Loop — Jab tak condition true ho

```powershell
$count = 1

while ($count -le 5) {
    Write-Output "Count: $count"
    $count++    # $count = $count + 1
}
```

### Pipeline ForEach — Short tarika

```powershell
Get-Process | ForEach-Object {
    Write-Output "Process: $($_.Name)"
}
# $_ = current object jo pipeline mein aa raha hai
```

### Loops Quick Reference

|Loop|Kab Use|Example|
|---|---|---|
|`for`|Fixed times — 1 se 10 tak|`for ($i=1; $i -le 10; $i++)`|
|`foreach`|List/Array ke har item pe|`foreach ($x in $array)`|
|`while`|Jab tak condition true ho|`while ($count -le 5)`|
|`ForEach-Object`|Pipeline mein|`Get-Process \| ForEach-Object`|

---

## 🔥 Real World Script — Sab Saath!

```powershell
# Service Desk — CPU Monitor Script
$processes = Get-Process | Sort-Object CPU -Descending | Select-Object -First 5

Write-Output "=== Top 5 CPU Processes ==="

foreach ($p in $processes) {
    if ($p.CPU -gt 100) {
        Write-Output "⚠️ HIGH CPU: $($p.Name) — $($p.CPU)"
    } else {
        Write-Output "✅ Normal: $($p.Name) — $($p.CPU)"
    }
}
```

```powershell
# Cybersecurity — Stopped Services Alert
$stopped = Get-Service | Where-Object Status -eq Stopped

Write-Output "=== Stopped Services ==="
foreach ($s in $stopped) {
    Write-Output "❌ Stopped: $($s.Name)"
}
Write-Output "Total Stopped: $($stopped.Count)"
```

---

## 💡 Important Syntax Rules

```powershell
# Variable andar string mein use karna
$name = "Jay"
Write-Output "Hello $name"          # Simple variable
Write-Output "CPU: $($p.CPU)"       # Object property — $() use karo!

# Increment/Decrement
$i++    # $i = $i + 1
$i--    # $i = $i - 1

# Modulo — remainder nikalna
$i % 2 -eq 0    # Even check
$i % 2 -ne 0    # Odd check

# Boolean values
$true   # True
$false  # False
```

---

## ✅ Topic 5 Summary

|Concept|Key Point|
|---|---|
|Variable|`$` se shuru — `$name = "Jay"`|
|String|Text — `"Jay"`|
|Integer|Number — `21`|
|Boolean|`$true` / `$false`|
|Array|`@("a","b","c")`|
|Hashtable|`@{Key = "Value"}`|
|if-else|Condition check|
|switch|Multiple conditions clean|
|for|Fixed times loop|
|foreach|List pe loop|
|while|Condition true tak loop|
|`$_`|Pipeline current object|
|`$()`|String ke andar expression|

## Golden Rules

```
1. Variable hamesha $ se shuru hota hai
2. String mein object property = $($p.Name) likho
3. For loop mein -le use karo — -ge nahi!
4. foreach = array/list ke liye best
5. while = condition based loop
6. $_ = pipeline ka current object
7. $true/$false = boolean values
```

---

## 🚀 Aage Kya Hai

- **Topic 6 — File & Data Handling** ← Next!
    - Files/folders manage karna
    - Read/write files
    - JSON & CSV handling

---

_Notes by Jay Sharma | PowerShell Masterclass | Topic 5_