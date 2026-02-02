# Secret Macro - Writeup

## Challenge Description
Find the hidden flag in this harmless macro. Maybe look at the comments?

## Solution

The flag was hidden in the macro comments within the VBA code. By examining the macro source:

1. Open the file with a macro-enabled application
2. Access the VBA editor (Alt+F11)
3. Look through the module comments
4. The flag appears in a hidden comment: `MCTF{C0MMENTS_ARE_DANGEROUS}`

## Tools Used
- Microsoft Office (Word/Excel)
- VBA Editor

## Key Takeaway
Always sanitize and review macro code, even in comments. Comments can be used to hide sensitive information.