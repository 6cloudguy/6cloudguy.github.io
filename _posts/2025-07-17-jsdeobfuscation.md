---
title: JavaScript Deobfuscation
date: 2025-07-17
categories: [Notes, HTB Academy]
tags: [HTB, Notes]
description: Notes for the 'JavaScript Deobfuscation' module from HTB Academy
---

### 🧠 Overview:
This module focused on understanding **JavaScript obfuscation**, why it's used, and how to analyze obfuscated code during web assessments.

---

## 🔎 Key Concepts:

### 🗂️ Where to Find JS Code:
- Inline in HTML (`<script>` tags)
- External files: Look for `.js` files in the page source or browser dev tools
- Network tab in dev tools or directory bruteforcing

### 🤐 What is Obfuscation?
- Process of making code difficult to read
- Commonly used to hide logic (e.g., login validation, token generation)
- Often used in malicious or poorly documented applications

---

## 🔧 Tools to Deobfuscate:

- [jsconsole.com](https://jsconsole.com/)
- [prettier.io](https://prettier.io/playground/)
- [jsnice.org](https://www.jsnice.org/)
- Browser Dev Tools (pretty print `{}` icon)

---

## 🧪 Common Commands Used

| Command                                                     | Description         |
| ----------------------------------------------------------- | ------------------- |
| `curl http://SERVER_IP:PORT/`                               | Basic GET request   |
| `curl -s http://SERVER_IP:PORT/ -X POST`                    | Silent POST request |
| `curl -s http://SERVER_IP:PORT/ -X POST -d "param1=sample"` | POST with data      |
| `echo hackthebox \| base64`                                 | Base64 encode       |
| `echo ENCODED_B64 \| base64 -d`                             | Base64 decode       |
| `echo hackthebox \| xxd -p`                                 | Hex encode          |
| `echo ENCODED_HEX \| xxd -p -r`                             | Hex decode          |
| `echo hackthebox \| tr 'A-Za-z' 'N-ZA-Mn-za-m'`             | ROT13 encode        |
| `echo ENCODED_ROT13 \| tr 'A-Za-z' 'N-ZA-Mn-za-m'`          | ROT13 decode        |

---

## 🧾 Summary:

This was a **simple and fast module** that introduced:
- How to **spot and retrieve** JS code
- How and **why JavaScript is obfuscated**
- How to **deobfuscate** and understand encoded logic
- Handy commands for decoding common encoding formats

Understanding JS obfuscation is critical for **client-side validation bypass, logic analysis, and web exploitation**.
