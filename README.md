# MemLab_3_DFIR

#  MemLabs Lab 3 - *The Evil’s Den*

**Memory Forensics Case Study (DFIR)**

##  Overview

This case study documents the forensic analysis of a Windows memory dump from **MemLabs Lab 3 – The Evil’s Den**.
The system contained evidence of a **malicious script used to encrypt sensitive information**, with part of the recovered data later hidden using **steganography**.

The challenge required reconstructing a **two-part secret** by correlating memory artifacts, reversing malicious logic, and extracting hidden data — all from volatile memory.

---

##  Objectives

* Identify suspicious user activity from memory artifacts
* Recover and analyze a malicious script executed on the system
* Reverse encryption logic applied to sensitive data
* Detect and extract steganographically hidden content
* Reconstruct the final secret **without relying on disk artifacts**

---

##  Tools & Technologies

* **Volatility 3** – Memory analysis and artifact extraction
* **Python** – Reverse engineering and decoding logic
* **XOR & Base64 analysis** – De-obfuscation techniques
* **Steghide** – Steganographic data extraction
* **AI-assisted analysis (Gemini CLI / “Athena”)** – Analyst-controlled workflow acceleration

> AI was used strictly as an **assistant**, not an autonomous decision-maker.
> All findings and commands were **manually reviewed and validated**.

---

##  Investigation Breakdown

### 1️ User & Process Attribution

Memory analysis revealed the active user account:

```
User: hello
```

Command-line arguments exposed two notable processes:

```text
notepad.exe → editing a Python script on the Desktop
notepad.exe → accessing a text file used as script output
```

This immediately suggested:

* Manual execution or testing of a script
* Output being written in plaintext after transformation

---

###  Malicious Script Recovery & Logic Analysis

The recovered Python script performed two sequential operations on user-supplied input:

1. **Character-wise XOR transformation**
2. **Base64 encoding of the transformed output**

This confirmed the presence of **custom, reversible obfuscation** rather than strong encryption.

---

### 3️. Encrypted Output Analysis (Part One)

The output file contained an encoded string produced by the script.

By:

* Reversing the Base64 encoding
* Applying the XOR operation with the discovered key

…the **first half of the protected data** was successfully reconstructed.

>  *The recovered value is intentionally redacted in this write-up.*

---

### 4️. Suspicious Media Discovery

File scanning of memory revealed an image stored on the Desktop, inconsistent with normal user activity.

Given the challenge context and tooling requirements, **steganographic concealment** was suspected.

---

### 5️. Steganographic Extraction (Part Two)

Using steganography analysis tools, hidden content was successfully extracted from the image.

This data:

* Completed the previously recovered fragment
* Confirmed intentional multi-stage data concealment

>  *The extracted content is intentionally redacted.*

---

##  Final Reconstruction (Obfuscated)

The recovered secret followed the format:

```text
inctf{********_****_***_***_******}
```

The complete value was reconstructed **only after correlating**:

* Script logic
* Encoded output
* Hidden image payload

---

##  Role of AI in the Investigation

AI (Gemini CLI, internally referred to as *Athena*) was used to:

* Speed up Volatility command execution
* Assist with pattern recognition across memory artifacts
* Reduce repetitive analyst workload

**Crucially:**

* No commands were executed blindly
* No conclusions were accepted without human validation

AI acted as a **force multiplier**, not a shortcut.

---

##  Key DFIR Takeaways

* Memory often contains **pre-encryption or pre-obfuscation artifacts**
* XOR-based schemes are trivial to reverse once identified
* Command-line arguments are high-value forensic evidence
* Steganography remains an effective secondary concealment layer
* AI can improve speed and accuracy **without undermining learning**

---

##  Series Progress

This case study represents **Lab 3** in a **6-part memory forensics series**.

Each lab increases in:

* Technical depth
* Analyst decision-making
* Realism of attacker tradecraft

 **Next:** *MemLabs Lab 4* — more persistence, less mercy.

<img width="939" height="563" alt="Screenshot 2026-02-10 151326" src="https://github.com/user-attachments/assets/4048ebc7-33da-41e9-a906-30792a8561ae" />
<img width="941" height="564" alt="image" src="https://github.com/user-attachments/assets/aa48068d-8c9e-4a99-9ee3-c6b1ec262cff" />
<img width="943" height="564" alt="image" src="https://github.com/user-attachments/assets/8b671ecc-f924-4fac-89d1-1b4c34453971" />

