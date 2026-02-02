# PNG Steganography - Writeup

## Challenge Description
The flag is hidden inside this innocent-looking image.

## Solution

### Step 1: Extract Metadata
```bash
exiftool image.png
strings image.png
```

### Step 2: Use Steganography Tools
```bash
steghide extract -sf image.png
```

Or with LSB steganography:
```bash
stegsolve.jar image.png
```

### Step 3: Check Every Color Channel
The flag might be in:
- Red channel LSBs
- Green channel LSBs
- Blue channel LSBs
- Alpha channel

### Step 4: Extract with Python
```python
from PIL import Image
img = Image.open('image.png')
pixels = img.load()

# Extract LSBs from red channel
message = ""
for y in range(img.height):
    for x in range(img.width):
        r, g, b, a = pixels[x, y]
        message += str(r & 1)

# Convert binary to ASCII
flag = ""
for i in range(0, len(message), 8):
    byte = message[i:i+8]
    if len(byte) == 8:
        flag += chr(int(byte, 2))
print(flag)
```

## Tools
- `steghide`
- `stegsolve`
- `exiftool`
- Python PIL/Pillow

## Flag
`MCTF{LSB_STEGANOGRAPHY_DETECTED}`

## Key Concepts
- Least Significant Bit (LSB) encoding
- Image metadata
- Frequency analysis