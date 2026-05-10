
## 🧠 Syntax Structure

```
CommandName  -ParameterName  ParameterValue
     ↑               ↑              ↑
  Kya karna      Kya dena        Kya value

Example:
Get-Process   -Name    chrome
Stop-Service  -Name    Spooler
Get-EventLog  -LogName System   -Newest  10
```

---

## 📦 Parameter Types

### 1. Named Parameters — Best Practice

```powershell
Get-Process -Name chrome
Stop-Service -Name Spooler
# Hamesha clear aur readable
```

### 2. Positional Parameters

```powershell
Get-Process chrome       # Positional — naam likhna zaroori nahi
Get-Process -Name chrome # Named — ye better hai
# Scripts mein hamesha named likho!
```

### 3. Switch Parameters — Value nahi deni

```powershell
Get-ChildItem -Recurse   # Subfolders bhi
Get-ChildItem -Force     # Hidden files bhi
# Switch = sirf likho — ON ho jaata hai!
```

### 4. Multiple Values — Comma se

```powershell
Get-Process -Name chrome,notepad,explorer
Get-EventLog -LogName System,Application
```

---

## ⚡ Tab Completion — Pro Tip!

```powershell
Get-Pro[TAB]              → Get-Process
Get-ChildItem -Rec[TAB]   → Get-ChildItem -Recurse
Stop-Ser[TAB]             → Stop-Service
```

**Fayda:** Time bachta hai + spelling mistakes nahi!

---

## 🔄 Aliases vs Real Commands

|Situation|Use|Kyun|
|---|---|---|
|Terminal pe quick kaam|Alias — `ls`, `cd`|Fast|
|Script likhna|Real Command|Readable + Safe|
|Team ke saath|Real Command|Sabko samajh aaye|
|Cybersecurity|Real Command|Professional|

---

## ⚠️ Common Errors

### Error 1 — Parameter Missing

```powershell
# ❌ Galat
Stop-Process chrome
# ✅ Sahi
Stop-Process -Name chrome
```

### Error 2 — Conflicting Parameters

```powershell
# ❌ Error — saath nahi chalte
Get-Command -Verb Stop -CommandType Cmdlet
# ✅ Pipeline use karo
Get-Command -Verb Stop | Where-Object CommandType -eq Cmdlet
```

### Error 3 — Alias Read-Only

```powershell
# ❌ Error
Set-Alias -Name gp -Value Get-Process
# ✅ Fix
Set-Alias -Name gp -Value Get-Process -Force
# Ya alag naam
Set-Alias -Name jtop -Value Get-Process
```

---

