# STICH

<p align="center">
  <img src="stich.png" alt="STICH Logo" width="200"/>
</p>

<p align="center">
  <strong>Secure Text Interleaving & Code Hashing</strong>
</p>

<p align="center">
  <em>Proprietary Python Code Obfuscation Tool</em>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#core-purpose">Core Purpose</a> •
  <a href="#features">Features</a> •
  <a href="#installation">Installation</a> •
  <a href="#usage">Usage</a> •
  <a href="#technical-specifications">Technical Specs</a>
</p>

---

## Overview

**STICH** (Secure Text Interleaving & Code Hashing) is a proprietary Python code obfuscation tool designed to protect source code from unauthorized reading and copying while maintaining full executability. Built with a modern PyQt6 interface matching the OMNISEC design language, STICH provides an intuitive way to encode entire tools or individual Python files using custom encoding rules that cannot be decoded with standard Base64 decoders.

## Core Purpose

STICH addresses a fundamental challenge in software distribution: **protecting intellectual property while maintaining functionality**. Unlike traditional encryption that renders files non-executable, STICH transforms Python source code into an obfuscated but fully runnable format.

### Why STICH?

- **Code Protection**: Prevent casual reading and unauthorized copying of your source code
- **Full Compatibility**: Encoded files run normally with Python interpreter
- **Import Support**: Encoded modules can be imported and used by other scripts
- **Bulk Processing**: Encode entire tool directories with a single click
- **Reversible**: Decode files when you need to modify the original source

## Features

### 🔐 Advanced Encoding System
- **Zlib Compression**: Source code compressed at maximum level (9)
- **XOR Encryption**: SHA-256 derived key from proprietary salt
- **Reversed Base64**: Completely inverted alphabet (Z-A, z-a, 9-0)
- **Magic Header**: STICH_V1 signature for format verification

### 📁 Multi-File & Tool Support
- **ADD FILES**: Select and process multiple Python files at once
- **TOOL Button**: Encode/decode entire tool directories
- **Smart Naming**: Automatic `-enc.py` and `-dec.py` output naming
- **Duplicate Handling**: Creates `filename(1).py`, `filename(2).py` for conflicts

### 🎨 Modern Interface
- **OMNISEC Design**: Green-themed UI matching the SCHUT/OMNISEC ecosystem
- **Visual Indicators**: ENCODE/DECODE status indicators with click-to-switch
- **Logo Overlay**: Semi-transparent background branding
- **Live Clock**: Real-time display with centiseconds precision

### 🛡️ Security Features
- **Obfuscated Variables**: Random variable names in wrapper code
- **Proprietary Salt**: Custom encryption salt prevents standard decoding
- **Import Compatibility**: Wrapper preserves module namespace for imports

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         STICH                               │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │   StichUI   │──│ StichEncoder│──│  Output Generator   │  │
│  │  (PyQt6)    │  │  (Engine)   │  │  (Wrapper/Files)    │  │
│  └─────────────┘  └─────────────┘  └─────────────────────┘  │
├─────────────────────────────────────────────────────────────┤
│                    Encoding Pipeline                         │
│  ┌──────┐  ┌──────┐  ┌──────────┐  ┌─────────┐  ┌────────┐  │
│  │Source│→ │ Zlib │→ │XOR Crypt │→ │Rev-B64  │→ │Wrapper │  │
│  │ Code │  │Compr.│  │(SHA-256) │  │ Encode  │  │ Gen.   │  │
│  └──────┘  └──────┘  └──────────┘  └─────────┘  └────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## Installation

### Prerequisites

- **Python**: 3.8 or higher
- **Operating System**: Linux, Windows, macOS

### Dependencies

```bash
pip install PyQt6
```

### Quick Start

```bash
# Clone or download STICH
git clone https://github.com/funbinet/stich.git

# Navigate to directory
cd stich

# Install dependencies
pip install -r requirements.txt

# Launch STICH
python3 stich.py
```

## Usage

### Encoding Individual Files

1. Launch STICH
2. Click **ENCODE** indicator to select encode mode (green indicator active)
3. Click **ADD FILES** to select one or more Python files
4. Click the **ENCODE** button
5. Encoded files saved as `filename-enc.py` in the same directory

### Decoding Files

1. Click **DECODE** indicator to switch mode
2. Select encoded files using **ADD FILES**
3. Click the **DECODE** button
4. Decoded files saved as `filename-dec.py`

### Encoding Entire Tools

1. Click the **TOOL** button (folder icon)
2. Select the root folder of your Python tool
3. Choose **ENCODE** or **DECODE** mode
4. Click the action button
5. Creates a new folder `toolname-enc/` with all Python files processed

### File Naming

| Operation | Input | Output |
|-----------|-------|--------|
| Encode File | `script.py` | `script-enc.py` |
| Decode File | `script-enc.py` | `script-dec.py` |
| Encode Tool | `mytool/` | `mytool-enc/` |
| Decode Tool | `mytool-enc/` | `mytool-dec/` |
| Duplicate | `script-enc.py` exists | `script-enc(1).py` |

## Technical Specifications

### Encoding Constants

| Component | Value |
|-----------|-------|
| Magic Header | `STICH_V1` |
| Salt | `FunBiNet_Proprietary_2026` |
| Compression | zlib level 9 |
| Hash Algorithm | SHA-256 |
| Base64 Alphabet | Reversed (Z-A, z-a, 9-0, +/) |

### Generated Wrapper

The encoded output is a Python script containing:
- Obfuscated import statements
- Base64 decoder with reversed alphabet
- XOR decryption routine
- Zlib decompression
- Dynamic execution in module namespace

### Why Standard Decoders Fail

1. **Reversed Alphabet**: Standard Base64 produces garbage
2. **XOR Layer**: Even with correct Base64, output is encrypted
3. **Compression**: Raw decoded data is compressed
4. **Magic Check**: Decoder validates STICH_V1 header

## Security Considerations

⚠️ **Important Notes:**

- STICH provides **obfuscation**, not military-grade encryption
- Designed to deter casual reading and prevent easy copying
- Determined attackers with Python knowledge can reverse-engineer
- Always keep backups of original source files
- Not suitable for protecting highly sensitive cryptographic keys

## UI Components

| Component | Function |
|-----------|----------|
| **Title** | "STICH" with OMNISEC styling |
| **Clock** | Live time (HH:MM:SS:CS) |
| **Author** | Click FUNBINET for GitHub |
| **ENCODE** | Mode indicator (click to select) |
| **DECODE** | Mode indicator (click to select) |
| **ADD FILES** | Multi-file selection |
| **TOOL** | Select tool folder |
| **CLEAR** | Remove all selected files |
| **Action Button** | Execute encode/decode |
| **Help (?)** | Documentation dialog |
| **Close (X)** | Exit application |

## Troubleshooting

### "Not STICH Encoded"
The file lacks the STICH_V1 magic header or was not encoded with STICH.

### "Invalid STICH Format"
The encoded payload is corrupted or was modified after encoding.

### Import Errors in Encoded Files
Ensure the original file has no syntax errors before encoding.

### Permission Denied on Tool Folders
Run STICH with appropriate permissions or check folder ownership.

## Author

<p align="center">
  <strong>FUNBINET</strong><br>
  Cybersecurity Student at Chuka University
</p>

<p align="center">
  <a href="https://github.com/funbinet">GitHub</a> •
  <a href="https://codeberg.org/funbinet">Codeberg</a> •
  <a href="https://dancan.tech">Website</a>
</p>

<p align="center">
  📧 funbinet@gmail.com
</p>

## License

**Proprietary** - All rights reserved.

This software is proprietary to FUNBINET. Unauthorized copying, modification, distribution, or use of this software is strictly prohibited without explicit written permission.

---

<p align="center">
  <em>STICH v1.0.0 - Proprietary Python Code Protection</em>
</p>

<p align="center">
  Part of the <strong>OMNISEC</strong> Security Toolkit
</p>
