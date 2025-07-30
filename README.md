# CVE-2025-50572: Data Extraction → Command Execution via CSV Injection in Archer

**Author:** Shuruq Hamdi

---

## Description

Archer RSA (v6.11..00204.10014) exports  data to CSV without proper cell escaping. An attacker can inject a spreadsheet formula (e.g. beginning with `=`) that, when opened in MS Excel or similar, will execute system commands.

---

## Affected Target/Versions

- Archer RSA
- v6.11..00204.10014

---

## Impact

- Remote code execution on the victim’s machine when they open a crafted CSV  

---

## Scope of Vuln

- Although this example uses the Device Registration form, any form field in the system that is exported to CSV without proper escaping can be exploited in the same way.
---

## Technical Details
```diff
1. Access the Device Registration form: Open the application’s Device Registration page (or any form in the system that later exports to CSV).

2. Inject the payload: In the Device Name field, enter a formula payload beginning with =. For example: =CMD|' /C calc'!A0.

3. Save the entry: Complete any other required fields and click Save. The malicious formula is now stored in the database.

4. Export to CSV: When an authorized user selects Export → CSV for device records, the exported file includes the injected payload in the Device Name column.

5. Open the CSV: The user opens the downloaded CSV file in a spreadsheet application (e.g., Microsoft Excel, LibreOffice, Google Sheets).

6. Trigger execution: The spreadsheet interprets the cell starting with = as a formula and executes the embedded command (e.g., launching calc.exe).
