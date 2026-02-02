# 🚩 Writeup: Secret Macro

### Challenge Information
* **Challenge:** Secret Macro
* **Category:** Forensics / Macros
* **Description:** Find the hidden flag in this harmless macro. Maybe look at the comments?

---

### 🕵️‍♂️ Methodology

The objective was to analyze a VBA macro embedded within a Microsoft Office document to locate hidden information.

1. **Inspection:** I opened the provided file in a macro-enabled application (Microsoft Word/Excel).

2. **Accessing Source Code:** I bypassed the standard interface and accessed the Visual Basic for Applications (VBA) editor using the shortcut `Alt + F11`.

3. **Code Review:** Navigating through the Project Explorer, I examined the code modules associated with the document.

4. **Discovery:** While the executable code was benign, a review of the comments revealed the flag hidden within a commented-out line.

### 🚩 The Flag

```text
MCTF{C0MMENTS_ARE_DANGEROUS}