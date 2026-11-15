# Lab 5: Static Code Analysis — Identified Issues & Fixes

**Student Name:** yashwanth  
**SRN:** PES2UG24AM190  
**Target File:** `online_exam.py`  
**Tools Employed:** Pylint 4.1.4, Bandit 1.9.4, Flake8 7.5.0  

---

## 📋 Comprehensive Issue Documentation Table

The table below documents all issues identified during the initial baseline scan across **Bandit**, **Pylint**, and **Flake8**, prioritized by security and bug severity (High & Medium first, followed by Code Quality, Design, and PEP 8 Formatting).

| Issue | Type | Line(s) | Description | Fix Approach |
| :--- | :---: | :---: | :--- | :--- |
| **Command Injection Vulnerability** (`B605`) | Security (High) | 56 | Calling `os.system("ssh " + labPc + " lock-screen")` initiates a subshell with untrusted string concatenation, allowing arbitrary shell command injection. | Replace `os.system` with `subprocess.run(["/usr/bin/ssh", lab_pc, "lock-screen"], check=True)` avoiding shell execution and using a strict argument list. |
| **Weak Cryptographic Hash** (`B324`) | Security (High) | 16 | Use of weak, collision-prone `hashlib.md5()` for answer validation hashing. | Upgrade to `hashlib.sha256()` with explicit `usedforsecurity=False` to provide collision-resistant hashing for non-cryptographic answer matching. |
| **Hardcoded Password String** (`B105`) | Security (Low) | 11 | Plaintext credential `PROCTOR_PASSWORD = "proctor-demo"` hardcoded directly in the source file. | Externalize secret configuration using `os.environ.get("PROCTOR_PASSWORD", "proctor-demo")` to prevent credentials from being committed to version control. |
| **Insecure Pseudo-Random Generator** (`B311`) | Security (Low) | 20 | Use of standard pseudo-random number generator `random.choice()` which is non-cryptographic and predictable. | Replace `random.choice()` with `secrets.choice()` from Python's standard `secrets` module for cryptographically strong random selection. |
| **Dangerous Mutable Default Argument** (`W0102`) | Bug (High) | 23 | Function signature `gradeStudent(studentName, responses, penalties={})` uses `{}` as default; this dictionary persists across calls, leaking state between students. | Replace default parameter with `penalties=None` and initialize `if penalties is None: penalties = {}` inside the function body. |
| **Invalid Identifier Naming Style** (`C0103`) | Code Quality | 15, 19, 23, 35, 45, 54 | Function and argument names (`hashAnswer`, `pickBonusQuestion`, `gradeStudent`, `studentName`, `getGrade`, `classAverage`, `lockScreen`, `labPc`) use camelCase instead of PEP 8 snake_case. | Rename all functions, parameters, and global dictionary (`results` -> `RESULTS`) to adhere to standard Python `snake_case` naming conventions. |
| **Suboptimal Loop Iteration** (`C0200`) | Code Smell | 25 | Iterating through collection indices using `for i in range(len(QUESTIONS))` instead of direct enumeration. | Refactor loop to `for i, question in enumerate(QUESTIONS):` for idiomatic, readable, and efficient iteration. |
| **Unnecessary `elif` After `return`** (`R1705`) | Code Smell | 37 | Redundant `elif` and `else` branches present after an immediate `return` statement in `getGrade()`. | Simplify control flow into clean guard clauses with sequential `return` statements, eliminating unnecessary indentation and keywords. |
| **Missing Module & Function Docstrings** (`C0114`, `C0116`) | Documentation | 1, 15, 19, 23, 35, 45, 51, 54, 59 | Entire module and all declared functions lack docstrings explaining their purpose, parameters, and return types. | Add comprehensive PEP 257-compliant docstrings to the module header and every function. |
| **Expected 2 Blank Lines** (`E302`) | Style / PEP 8 | 54 | Only 1 blank line found before top-level function definition `lockScreen` instead of the required 2 blank lines. | Insert an additional blank line before `lock_screen` to satisfy PEP 8 top-level separation. |
| **Missing Whitespace Around Operator** (`E225`) | Style / PEP 8 | 67 | Missing surrounding spaces around assignment operator in `average=classAverage()`. | Add spaces before and after the `=` operator: `average = class_average()`. |
| **Inline Comment Spacing** (`E261`) | Style / PEP 8 | 69 | Single space before inline comment `# highest score` instead of at least two spaces required by PEP 8. | Ensure at least two spaces precede `# highest score`. |
| **Line Length Exceeds Limit** (`E501`) | Style / PEP 8 | 70 | Line 70 (93 characters) exceeds the maximum PEP 8 line length limit of 79 characters. | Break long string formatting across multiple lines using an intermediate variable and formatted f-string. |

---

## 🎯 Verification & Results Summary

| Analysis Tool | Baseline (Before) | Final (After) | Status |
| :--- | :---: | :---: | :---: |
| **Bandit** | **4 issues** (2 High, 2 Low) | **0 issues** (No issues identified) | ✅ **100% Resolved** |
| **Pylint** | **5.65 / 10** (20 warnings/conventions) | **10.00 / 10** (0 warnings) | ✅ **Perfect Score** |
| **Flake8** | **4 violations** (E302, E225, E261, E501) | **0 violations** (Clean exit code 0) | ✅ **100% Clean** |
| **Functional Output** | Working demonstration | Identical behavior & outputs preserved | ✅ **Verified** |
