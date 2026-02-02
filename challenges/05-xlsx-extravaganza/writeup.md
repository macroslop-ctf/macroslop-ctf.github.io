# XLSX Extravaganza - Writeup

## Challenge Description
This Excel file is hiding something. Unzip it and find the secret.

## Solution

### Step 1: Understand XLSX Format
XLSX files are actually ZIP archives containing XML files.

```bash
unzip challenge.xlsx -d extracted/
```

### Step 2: Explore the Structure
```
extracted/
├── [Content_Types].xml
├── _rels/
├── xl/
│   ├── workbook.xml
│   ├── worksheets/
│   │   └── sheet1.xml
│   └── sharedStrings.xml
└── docProps/
    └── core.xml
```

### Step 3: Check Hidden Sheets
Examine `xl/workbook.xml`:
```xml
<sheet name="Hidden" sheetId="2" r:id="rId2" state="hidden"/>
```

### Step 4: Extract from Hidden Sheet
The flag is in `xl/worksheets/sheet2.xml`:
```xml
<v>MCTF{XLSX_IS_JUST_A_ZIP_FILE}</v>
```

## Tools
```bash
unzip, grep, xmllint
```

## Flag
`MCTF{XLSX_IS_JUST_A_ZIP_FILE}`

## Key Takeaway
Many "container" formats (DOCX, PPTX, XLSX) are ZIP archives. Always check what's inside!