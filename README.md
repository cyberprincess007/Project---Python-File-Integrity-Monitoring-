# Project---Python-File-Integrity-Monitoring-
I built a Python program that monitors files for changes and logs any modifications. It works by calculating a unique hash for each file, checking it at regular intervals, and recording the date and time whenever the hash changes. This helps detect unauthorized or unexpected file changes in real time.
# File Integrity Monitor (FIM)

## Overview
For this project, I built a **File Integrity Monitor (FIM)** in Python to track changes to specific files. If a file changes, the program logs the event with a timestamp. This tool is inspired by the **Integrity** principle of the CIA Triad — knowing when files change is critical for detecting unauthorized activity and maintaining system trust.

This was a hands-on way for me to think like a security engineer. By monitoring files and logging changes in real time, I created a small but practical tool that could be used in a real-world security environment.

---

## What is a File Integrity Monitor (FIM)?
A **File Integrity Monitor** watches files for unauthorized changes. If a file changes unexpectedly — whether due to malware, insider threats, or accidental modification — the FIM detects it and alerts the system owner.

### Why I Built This:
- **Detect Unauthorized Changes:** Track when important files are changed without approval.
- **Prevent Security Breaches:** Catch early warning signs of compromise.
- **Meet Compliance Needs:** FIM tools are part of standards like PCI DSS and HIPAA.

---

## How My Script Works
Here’s the process my script follows:

1. Calculates a **hash** (digital fingerprint) of the monitored file.
2. Checks the file every few seconds.
3. If the hash changes, logs the event in a log file with a timestamp.
4. Keeps running so changes are detected in real time.

This way, I always know when — and which — files were modified.

---

## Setting It Up

### 1. Check for Python
I made sure Python was installed by running:
```bash
python --version
If not installed, I downloaded it from python.org and checked "Add Python to PATH" during installation.

---

## ** 2. Install Required Libraries**
The script uses Python’s built-in libraries:

hashlib — create file hashes

os — handle file paths and check if files exist

time — pause between checks

No external libraries were needed, but you can confirm built-in availability.

Project Structure
My folder looks like this:

bash
Copy
Edit
file-integrity-monitor/
│ - fim.py              # Main script
│ - logs/                # Stores file change logs
│ - watched_file.txt     # File being monitored
The fim.py script is the heart of the program. The logs folder stores any changes detected.

Writing the Script
Calculate File Hash
This function calculates a SHA-256 hash for the file:

python
Copy
Edit
import hashlib
import os
import time

def calculate_file_hash(file_path):
    """Calculate and return the hash of the file."""
    hash_object = hashlib.sha256()
    with open(file_path, 'rb') as file:
        file_data = file.read()
        hash_object.update(file_data)
    return hash_object.hexdigest()
Monitor for Changes
This continuously checks the file for modifications:

python
Copy
Edit
files_to_monitor = ['watched_file.txt']

def monitor_files():
    """Monitor files for changes by comparing their hashes."""
    previous_hashes = {}
    while True:
        for file_path in files_to_monitor:
            if os.path.exists(file_path):
                current_hash = calculate_file_hash(file_path)
                if file_path not in previous_hashes or previous_hashes[file_path] != current_hash:
                    print(f"File has changed: {file_path}")
                    previous_hashes[file_path] = current_hash
                    log_change(file_path)
                else:
                    print(f"No change detected for {file_path}")
            else:
                print(f"File does not exist: {file_path}")
        time.sleep(5)
Log the Changes
This creates a log entry when changes are found:

python
Copy
Edit
def log_change(file_path):
    """Log the changes to a file in the logs folder."""
    log_directory = 'logs'
    if not os.path.exists(log_directory):
        os.makedirs(log_directory)

    log_file_path = os.path.join(log_directory, 'file_changes.log')

    with open(log_file_path, 'a') as log_file:
        log_file.write(f"{time.ctime()}: {file_path} has changed.\n")
Running the Program
To start monitoring:

bash
Copy
Edit
python fim.py
Edit and save watched_file.txt.

The script will detect the change and record it in logs/file_changes.log.

Testing
When I modified the file:

The program printed a message to the console saying the file changed.

The change was logged with the date and time in file_changes.log.

Troubleshooting
If it doesn’t work:

Check Python is installed: python --version

Make sure the file path in files_to_monitor is correct.

Use print() statements to debug.

Look for typos in file names.

What I Learned
Building this project helped me understand:

How to work with file paths in Python.

How hashing helps track file changes.

How to log security events in a structured way.

Why file integrity monitoring matters in cybersecurity.
