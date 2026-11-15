# Lab 5: Static Code Analysis — Online Examination System

**Course:** Software Engineering Laboratory  
**Lab Assignment:** Lab 5 — Static Code Analysis  
**Student Name:** yashwanth  
**SRN:** PES2UG24AM190  
**Target Repository:** [https://github.com/Jamesbond0070/SE-Lab-5](https://github.com/Jamesbond0070/SE-Lab-5)  

---

## 📌 Executive Summary
This laboratory project demonstrates the application of industry-standard **Static Code Analysis** tools—**Bandit**, **Pylint**, and **Flake8**—to audit, secure, refactor, and clean a Python-based **Online Examination System** (`online_exam.py`).

Through a structured, multi-stage remediation process across three dedicated Git commits, all identified security vulnerabilities, code bugs, logical smells, and PEP 8 style violations were systematically resolved without modifying runtime functionality.

---

## 📁 Repository Deliverables & Structure

| File / Directory | Description |
| :--- | :--- |
| [`online_exam.py`](online_exam.py) | Fully refactored, secure, clean Python script (**Pylint: 10.00/10, Bandit: 0 issues, Flake8: 0 violations**) |
| [`issues.md`](issues.md) | Comprehensive table documenting all 13 identified issues, types, lines, descriptions, and fix approaches |
| [`reflection.md`](reflection.md) | In-depth reflection report answering all four laboratory reflection questions |
| [`bandit_report.txt`](bandit_report.txt) | Final Bandit security report confirming **0 vulnerabilities identified** |
| [`pylint_report.txt`](pylint_report.txt) | Final Pylint report confirming a perfect score of **10.00 / 10** |
| [`flake8_report.txt`](flake8_report.txt) | Final Flake8 report confirming **0 formatting/style violations** |
| [`bandit_report_before.txt`](bandit_report_before.txt) | Baseline Bandit scan report showing initial 4 security vulnerabilities |
| [`pylint_report_before.txt`](pylint_report_before.txt) | Baseline Pylint scan report showing initial 5.65/10 quality rating |
| [`flake8_report_before.txt`](flake8_report_before.txt) | Baseline Flake8 scan report showing initial 4 PEP 8 style violations |
| [`reports/before/`](reports/before/) | Directory containing all original baseline reports |
| [`reports/after/`](reports/after/) | Directory containing all post-remediation verified reports |

---

## 📊 Static Analysis Metrics: Before vs. After

| Metric / Dimension | Baseline (Before) | Post-Remediation (After) | Improvement / Delta |
| :--- | :---: | :---: | :---: |
| **Bandit Security Issues** | **4 issues** (2 High, 2 Low) | **0 issues** | 🛡️ **100% Vulnerabilities Eliminated** |
| **High Severity Security** | 2 (`B605` shell injection, `B324` MD5 hash) | **0** | ✅ Clean AST scan |
| **Pylint Quality Score** | **5.65 / 10** (20 warnings/conventions) | **10.00 / 10** | 📈 **+4.35 / 10 (Perfect 10/10)** |
| **Mutable Default Args** | 1 (`W0102` `penalties={}`) | **0** (Fixed with `None` guard) | 🐛 **State leak prevented** |
| **Flake8 PEP 8 Violations** | **4 violations** (`E302`, `E225`, `E261`, `E501`) | **0 violations** | 🎨 **100% PEP 8 Compliant** |
| **Runtime Functional Output** | Working demonstration | Identical behavior & outputs preserved | 🧪 **100% Functional Integrity** |

---

## 🛠️ Summary of Issues & Remediation

### 1. Security Vulnerabilities (Bandit)
- **Command Injection (`B605` — High Severity):** The original code concatenated unsanitized input via `os.system("ssh " + labPc + " lock-screen")`. This was replaced with `subprocess.run(["/usr/bin/ssh", lab_pc, "lock-screen"], check=True)` avoiding subshell invocation and using a fixed argument vector.
- **Weak MD5 Hash (`B324` — High Severity):** Replaced `hashlib.md5()` with `hashlib.sha256(..., usedforsecurity=False)` for cryptographic collision resistance.
- **Hardcoded Secret (`B105` — Low Severity):** Plaintext `PROCTOR_PASSWORD = "proctor-demo"` replaced with `os.environ.get("PROCTOR_PASSWORD", "proctor-demo")`.
- **Insecure PRNG (`B311` — Low Severity):** Replaced non-cryptographic `random.choice()` with `secrets.choice()`.

### 2. Code Quality & Design Flaws (Pylint)
- **Mutable Default Argument (`W0102` — Bug):** Replaced `def gradeStudent(..., penalties={})` with `penalties=None` and initialized `if penalties is None: penalties = {}` inside the function, preventing shared state leakage across function calls.
- **Naming Conventions (`C0103`):** Refactored all camelCase functions and arguments (`hashAnswer`, `studentName`, `lockScreen`, etc.) to standard PEP 8 `snake_case`.
- **Inefficient Iteration (`C0200`):** Converted `for i in range(len(QUESTIONS))` to idiomatic `for i, question in enumerate(QUESTIONS)`.
- **Redundant Control Flow (`R1705`):** Removed unnecessary `elif`/`else` branches after `return` in `get_grade()`.
- **Docstrings (`C0114`, `C0116`):** Added comprehensive module and function-level docstrings per PEP 257.

### 3. Style & Formatting (Flake8)
- **`E302`:** Added required 2 blank lines before top-level function definitions.
- **`E225`:** Added whitespace around operators (`average = class_average()`).
- **`E261`:** Added two spaces before inline comments (`# highest score`).
- **`E501`:** Wrapped all lines exceeding the 79-character limit.

---

## 📦 3-Commit Git History

In strict accordance with the lab requirement of **3 separate commits** for the changes made to address issues raised by the three static analysis tools:

1. **Commit 1 (`816f74f`):**  
   `fix(security): resolve Bandit vulnerabilities (B605, B324, B105, B311)`  
   *Remediated shell command injection, MD5 hashing, hardcoded credentials, and insecure randomness. Added baseline before reports and 0-issue `bandit_report.txt`.*

2. **Commit 2 (`b74d359`):**  
   `refactor(quality): resolve Pylint issues achieving 10.00/10 code quality score`  
   *Fixed mutable default argument bug, refactored identifiers to snake_case, added docstrings, and improved Pylint rating to 10.00/10.*

3. **Commit 3 (Current):**  
   `style(pep8): resolve Flake8 violations and add documentation and reports`  
   *Fixed blank lines, operator whitespace, inline comment spacing, line lengths, added `issues.md`, `reflection.md`, final reports, and `README.md`.*

---

## 🚀 How to Run and Verify Locally

```bash
# Clone the repository
git clone https://github.com/Jamesbond0070/SE-Lab-5.git
cd SE-Lab-5

# Install static code analysis tools
python3 -m pip install pylint bandit flake8

# Execute the application to verify behavior
python3 online_exam.py

# Verify Bandit security scan (Expect: No issues identified)
python3 -m bandit -r online_exam.py

# Verify Pylint quality rating (Expect: 10.00/10)
python3 -m pylint online_exam.py

# Verify Flake8 style compliance (Expect: Clean output, exit code 0)
python3 -m flake8 online_exam.py
```
