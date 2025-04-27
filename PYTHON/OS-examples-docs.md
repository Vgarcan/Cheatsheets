# 📚 Mastering Python's `os` Module: 30 Practical Projects (Beginner to Advanced)

## Table of Contents

- [📚 Mastering Python's `os` Module: 30 Practical Projects (Beginner to Advanced)](#-mastering-pythons-os-module-30-practical-projects-beginner-to-advanced)
  - [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
- [Beginner Projects (1-10)](#beginner-projects-1-10)
  - [1. List all files in a directory](#1-list-all-files-in-a-directory)
  - [2. Create a folder if it doesn't exist](#2-create-a-folder-if-it-doesnt-exist)
  - [3. Create a basic web project structure](#3-create-a-basic-web-project-structure)
  - [4. Delete files with a specific extension](#4-delete-files-with-a-specific-extension)
  - [5. Rename multiple files](#5-rename-multiple-files)
  - [6. Copy files to another folder (backup)](#6-copy-files-to-another-folder-backup)
  - [7. Organize downloads by file type](#7-organize-downloads-by-file-type)
  - [8. Check if a file exists](#8-check-if-a-file-exists)
  - [9. Monitor directory changes](#9-monitor-directory-changes)
  - [10. Remove empty folders](#10-remove-empty-folders)
- [🛠️ Intermediate Projects (11-20)](#️-intermediate-projects-11-20)
  - [11. **Find and replace text in multiple files**](#11-find-and-replace-text-in-multiple-files)
  - [12. **Move specific files to a backup folder**](#12-move-specific-files-to-a-backup-folder)
  - [13. **Synchronize two folders**](#13-synchronize-two-folders)
  - [14. **Create a daily backup folder with timestamp**](#14-create-a-daily-backup-folder-with-timestamp)
  - [15. **List large files in a directory**](#15-list-large-files-in-a-directory)
  - [16. **Check file permissions**](#16-check-file-permissions)
  - [17. **Generate a report of all file sizes**](#17-generate-a-report-of-all-file-sizes)
  - [18. **Identify duplicate filenames**](#18-identify-duplicate-filenames)
  - [19. **Change file extensions**](#19-change-file-extensions)
  - [20. **Simulate a "Trash Bin"**](#20-simulate-a-trash-bin)
- [🧠 Advanced Projects (21-30)](#-advanced-projects-21-30)
  - [21. **Create a script that zips an entire folder**](#21-create-a-script-that-zips-an-entire-folder)
  - [22. **Extract all files from a zip archive**](#22-extract-all-files-from-a-zip-archive)
  - [23. **Set environment variables programmatically**](#23-set-environment-variables-programmatically)
  - [24. **Create an installer script for a Python project**](#24-create-an-installer-script-for-a-python-project)
  - [25. **Create a dynamic sitemap of all HTML files**](#25-create-a-dynamic-sitemap-of-all-html-files)
  - [26. **Schedule a script to run periodically (cronjob simulation)**](#26-schedule-a-script-to-run-periodically-cronjob-simulation)
  - [27. **Create a "Safe Delete" script with confirmation**](#27-create-a-safe-delete-script-with-confirmation)
  - [28. **Create a file integrity checker (hash comparison)**](#28-create-a-file-integrity-checker-hash-comparison)
  - [29. **Mirror a directory structure without copying files**](#29-mirror-a-directory-structure-without-copying-files)
  - [30. **Detect and remove broken symbolic links**](#30-detect-and-remove-broken-symbolic-links)
- [📜 Final Summary](#-final-summary)
- [🎯 Conclusion](#-conclusion)

# Introduction

The `os` module is a fundamental building block in Python for interacting with the operating system. It provides capabilities for file and directory management, environment interaction, and executing system commands. Mastering `os` will make you a much more capable developer, ready to automate and simplify a wide range of everyday tasks.

This guide is divided into three difficulty levels:
- **Beginner (1-10)**: Basic and frequent real-world tasks.
- **Intermediate (11-20)**: More complex operations combining multiple actions.
- **Advanced (21-30)**: Robust automation and real-world simulations.

All exercises are useful, practical, and designed to strengthen your development skills.

---

# Beginner Projects (1-10)

## 1. List all files in a directory

**Objective:** Use `os.listdir()` to list all files and folders in a given path.

**Code:**
```python
import os

directory = "./"

for file in os.listdir(directory):
    print(file)
```

**Applications:** File analysis, resource loading.

[Back to top](#table-of-contents)

---

## 2. Create a folder if it doesn't exist

**Objective:** Prevent errors by checking folder existence before creation.

**Code:**
```python
import os

folder_name = "new_folder"

if not os.path.exists(folder_name):
    os.mkdir(folder_name)
    print(f"Created {folder_name}")
else:
    print(f"{folder_name} already exists")
```

**Applications:** Dynamic project setups.

[Back to top](#table-of-contents)

---

## 3. Create a basic web project structure

**Objective:** Automate creating common project folders.

**Code:**
```python
import os

folders = ["static", "templates", "app", "app/models", "app/views"]

for folder in folders:
    os.makedirs(folder, exist_ok=True)
    print(f"Created {folder}")
```

**Applications:** Faster project scaffolding.

[Back to top](#table-of-contents)

---

## 4. Delete files with a specific extension

**Objective:** Remove unwanted or temporary files automatically.

**Code:**
```python
import os

directory = "./"

for file in os.listdir(directory):
    if file.endswith(".tmp"):
        os.remove(os.path.join(directory, file))
        print(f"Deleted {file}")
```

**Applications:** Project cleanup.

[Back to top](#table-of-contents)

---

## 5. Rename multiple files

**Objective:** Batch rename files for organization.

**Code:**
```python
import os

directory = "./"
prefix = "updated_"

for file in os.listdir(directory):
    if file.endswith(".txt"):
        old_path = os.path.join(directory, file)
        new_path = os.path.join(directory, prefix + file)
        os.rename(old_path, new_path)
        print(f"Renamed {file}")
```

**Applications:** Dataset or file organization.

[Back to top](#table-of-contents)

---

## 6. Copy files to another folder (backup)

**Objective:** Backup files automatically to a safe location.

**Code:**
```python
import os
import shutil

source = "./"
destination = "./backup/"

os.makedirs(destination, exist_ok=True)

for file in os.listdir(source):
    if file.endswith(".py"):
        shutil.copy(os.path.join(source, file), destination)
        print(f"Copied {file}")
```

**Applications:** Backup critical work.

[Back to top](#table-of-contents)

---

## 7. Organize downloads by file type

**Objective:** Sort files automatically into categorized folders.

**Code:**
```python
import os
import shutil

downloads = "./downloads"
folders = {"pdf": "pdfs", "jpg": "images", "png": "images", "zip": "archives"}

for file in os.listdir(downloads):
    file_path = os.path.join(downloads, file)
    if os.path.isfile(file_path):
        ext = file.split(".")[-1]
        if ext in folders:
            dest = os.path.join(downloads, folders[ext])
            os.makedirs(dest, exist_ok=True)
            shutil.move(file_path, dest)
            print(f"Moved {file}")
```

**Applications:** Keeping Downloads clean.

[Back to top](#table-of-contents)

---

## 8. Check if a file exists

**Objective:** Avoid errors by validating file presence.

**Code:**
```python
import os

file_path = "config.yaml"

if os.path.isfile(file_path):
    print(f"{file_path} exists!")
else:
    print(f"{file_path} does not exist.")
```

**Applications:** Loading config files safely.

[Back to top](#table-of-contents)

---

## 9. Monitor directory changes

**Objective:** Detect file creation or deletion.

**Code:**
```python
import os
import time

folder = "./"
old = set(os.listdir(folder))

print("Monitoring changes...")

try:
    while True:
        time.sleep(5)
        new = set(os.listdir(folder))
        if new != old:
            print(f"Changes detected: {new - old}")
        old = new
except KeyboardInterrupt:
    print("Stopped monitoring.")
```

**Applications:** Simple file monitoring systems.

[Back to top](#table-of-contents)

---

## 10. Remove empty folders

**Objective:** Clean up project clutter.

**Code:**
```python
import os

root = "./"

for foldername, subfolders, filenames in os.walk(root, topdown=False):
    if not subfolders and not filenames:
        os.rmdir(foldername)
        print(f"Deleted empty folder: {foldername}")
```

**Applications:** Maintain clean project structures.

[Back to top](#table-of-contents)

---

# 🛠️ Intermediate Projects (11-20)

---

## 11. **Find and replace text in multiple files**

**Objective:**  
Batch update contents across multiple files (useful in website migrations or bulk corrections).

**Code:**
```python
import os

directory = "./"
search_text = "old_text"
replace_text = "new_text"

for file in os.listdir(directory):
    if file.endswith(".txt"):
        file_path = os.path.join(directory, file)
        with open(file_path, 'r') as f:
            content = f.read()
        content = content.replace(search_text, replace_text)
        with open(file_path, 'w') as f:
            f.write(content)
        print(f"Updated {file}")
```

✅ **Applications:** Updating templates, fixing bugs in multiple files.

---

## 12. **Move specific files to a backup folder**

**Objective:**  
Move instead of copy to clean the original folder after backup.

**Code:**
```python
import os
import shutil

source = "./"
backup = "./backup/"

os.makedirs(backup, exist_ok=True)

for file in os.listdir(source):
    if file.endswith(".log"):
        shutil.move(os.path.join(source, file), backup)
        print(f"Moved {file} to {backup}")
```

✅ **Applications:** Organize logs, save storage space.

---

## 13. **Synchronize two folders**

**Objective:**  
Copy only missing files from one folder to another.

**Code:**
```python
import os
import shutil

source = "./source"
destination = "./destination"

os.makedirs(destination, exist_ok=True)

for file in os.listdir(source):
    src_file = os.path.join(source, file)
    dest_file = os.path.join(destination, file)
    if not os.path.exists(dest_file):
        shutil.copy(src_file, dest_file)
        print(f"Copied {file} to destination")
```

✅ **Applications:** Website deployment, backup utilities.

---

## 14. **Create a daily backup folder with timestamp**

**Objective:**  
Use dynamic folder names based on date.

**Code:**
```python
import os
import shutil
from datetime import datetime

today = datetime.now().strftime("%Y-%m-%d")
backup_folder = f"./backup_{today}"

os.makedirs(backup_folder, exist_ok=True)

# Example copying
shutil.copy("important_file.txt", backup_folder)

print(f"Backup created at {backup_folder}")
```

✅ **Applications:** Automatic daily backups.

---

## 15. **List large files in a directory**

**Objective:**  
Find files bigger than a certain size.

**Code:**
```python
import os

directory = "./"
threshold_size = 1 * 1024 * 1024  # 1 MB

for file in os.listdir(directory):
    file_path = os.path.join(directory, file)
    if os.path.isfile(file_path) and os.path.getsize(file_path) > threshold_size:
        print(f"{file} is larger than 1 MB")
```

✅ **Applications:** Storage analysis, server maintenance.

---

## 16. **Check file permissions**

**Objective:**  
Verify if a file is readable, writable, and executable.

**Code:**
```python
import os

file_path = "script.py"

print(f"Readable: {os.access(file_path, os.R_OK)}")
print(f"Writable: {os.access(file_path, os.W_OK)}")
print(f"Executable: {os.access(file_path, os.X_OK)}")
```

✅ **Applications:** Server security checks.

---

## 17. **Generate a report of all file sizes**

**Objective:**  
Create a simple inventory report.

**Code:**
```python
import os

directory = "./"
report_file = "report.txt"

with open(report_file, 'w') as report:
    for file in os.listdir(directory):
        file_path = os.path.join(directory, file)
        if os.path.isfile(file_path):
            size = os.path.getsize(file_path)
            report.write(f"{file}: {size} bytes\n")

print(f"Report generated: {report_file}")
```

✅ **Applications:** Storage management, audits.

---

## 18. **Identify duplicate filenames**

**Objective:**  
Detect files with the same name even if they are in different folders.

**Code:**
```python
import os

directory = "./"
seen = set()

for root, dirs, files in os.walk(directory):
    for file in files:
        if file in seen:
            print(f"Duplicate found: {file}")
        else:
            seen.add(file)
```

✅ **Applications:** Cleanup, data deduplication.

---

## 19. **Change file extensions**

**Objective:**  
Bulk change `.txt` files to `.md` or similar.

**Code:**
```python
import os

directory = "./"

for file in os.listdir(directory):
    if file.endswith(".txt"):
        base = os.path.splitext(file)[0]
        new_name = base + ".md"
        os.rename(os.path.join(directory, file), os.path.join(directory, new_name))
        print(f"Renamed {file} to {new_name}")
```

✅ **Applications:** Converting documentation formats.

---

## 20. **Simulate a \"Trash Bin\"**

**Objective:**  
Move deleted files into a \"trash\" folder instead of deleting permanently.

**Code:**
```python
import os
import shutil

trash_bin = "./trash"
os.makedirs(trash_bin, exist_ok=True)

file_to_delete = "old_note.txt"

if os.path.exists(file_to_delete):
    shutil.move(file_to_delete, trash_bin)
    print(f"Moved {file_to_delete} to trash bin")
else:
    print(f"{file_to_delete} not found")
```

✅ **Applications:** Safer file management, undo delete functionality.

---

# 🧠 Advanced Projects (21-30)

---

## 21. **Create a script that zips an entire folder**

**Objective:**  
Compress a full project directory into a `.zip` archive.

**Code:**
```python
import os
import shutil

source_folder = "./my_project"
output_zip = "project_backup"

# Create a zip archive
shutil.make_archive(output_zip, 'zip', source_folder)

print(f"Created zip archive: {output_zip}.zip")
```

✅ **Applications:** Project delivery, backup automation, code deployment.

---

## 22. **Extract all files from a zip archive**

**Objective:**  
Unpack compressed files automatically.

**Code:**
```python
import os
import shutil

zip_file = "project_backup.zip"
destination_folder = "./extracted/"

os.makedirs(destination_folder, exist_ok=True)

shutil.unpack_archive(zip_file, destination_folder)

print(f"Extracted {zip_file} into {destination_folder}")
```

✅ **Applications:** Automated deployment, unpacking resources.

---

## 23. **Set environment variables programmatically**

**Objective:**  
Modify environment variables within Python.

**Code:**
```python
import os

# Setting environment variable
os.environ['MY_SECRET'] = 'supersecret123'

# Accessing environment variable
print(f"My secret is: {os.getenv('MY_SECRET')}")
```

✅ **Applications:** Managing secrets, configuring apps securely.

---

## 24. **Create an installer script for a Python project**

**Objective:**  
Set up project folders, copy files, and prepare a structure automatically.

**Code:**
```python
import os
import shutil

folders = ["logs", "data", "src", "tests"]

for folder in folders:
    os.makedirs(folder, exist_ok=True)

shutil.copy("main.py", "src/main.py")

print("Project structure created and main.py copied to src/")
```

✅ **Applications:** Streamlining project onboarding.

---

## 25. **Create a dynamic sitemap of all HTML files**

**Objective:**  
Scan folders and list all `.html` files for a sitemap.

**Code:**
```python
import os

directory = "./"
sitemap = "sitemap.txt"

with open(sitemap, 'w') as f:
    for root, dirs, files in os.walk(directory):
        for file in files:
            if file.endswith(".html"):
                path = os.path.join(root, file)
                f.write(path.replace("\\", "/") + "\n")

print(f"Sitemap generated at {sitemap}")
```

✅ **Applications:** SEO tasks, project documentation.

---

## 26. **Schedule a script to run periodically (cronjob simulation)**

**Objective:**  
Simulate a basic task scheduler in Python.

**Code:**
```python
import time
import os

def task():
    print("Running scheduled task...")
    # Example task: Create a timestamped file
    with open(f"log_{int(time.time())}.txt", 'w') as f:
        f.write("Scheduled task executed.")

try:
    while True:
        task()
        time.sleep(3600)  # Wait 1 hour
except KeyboardInterrupt:
    print("Scheduler stopped.")
```

✅ **Applications:** Backups, health checks, automation.

---

## 27. **Create a \"Safe Delete\" script with confirmation**

**Objective:**  
Ask confirmation before deleting anything.

**Code:**
```python
import os

file_to_delete = "important_document.txt"

if os.path.exists(file_to_delete):
    confirm = input(f"Are you sure you want to delete {file_to_delete}? (yes/no): ").strip().lower()
    if confirm == "yes":
        os.remove(file_to_delete)
        print(f"{file_to_delete} deleted.")
    else:
        print("Deletion cancelled.")
else:
    print(f"{file_to_delete} does not exist.")
```

✅ **Applications:** Prevent accidental data loss.

---

## 28. **Create a file integrity checker (hash comparison)**

**Objective:**  
Ensure a file hasn’t been tampered with by verifying its checksum.

**Code:**
```python
import hashlib

def get_file_hash(file_path):
    hasher = hashlib.sha256()
    with open(file_path, 'rb') as f:
        while chunk := f.read(4096):
            hasher.update(chunk)
    return hasher.hexdigest()

original_hash = "expected_sha256_hash_here"
file_to_check = "important_file.txt"

if get_file_hash(file_to_check) == original_hash:
    print("File integrity verified.")
else:
    print("WARNING: File has been modified!")
```

✅ **Applications:** Secure file transfers, malware detection.

---

## 29. **Mirror a directory structure without copying files**

**Objective:**  
Replicate folder structure only (empty folders).

**Code:**
```python
import os

source = "./source_project"
destination = "./destination_structure"

for root, dirs, files in os.walk(source):
    for dir_ in dirs:
        structure = os.path.join(destination, os.path.relpath(os.path.join(root, dir_), source))
        os.makedirs(structure, exist_ok=True)

print("Folder structure mirrored successfully.")
```

✅ **Applications:** Skeleton setups for projects, backups without heavy files.

---

## 30. **Detect and remove broken symbolic links**

**Objective:**  
Find and clean up invalid symlinks.

**Code:**
```python
import os

directory = "./"

for root, dirs, files in os.walk(directory):
    for file in files:
        path = os.path.join(root, file)
        if os.path.islink(path) and not os.path.exists(os.readlink(path)):
            print(f"Broken link detected: {path}")
            os.remove(path)
            print(f"Removed broken link: {path}")
```

✅ **Applications:** Maintenance of Linux/Unix servers, project directories.

---

# 📜 Final Summary

| Level          | Status        |
|----------------|---------------|
| Beginner (1-10) | ✅ Completed  |
| Intermediate (11-20) | ✅ Completed |
| Advanced (21-30) | ✅ Completed |

---

# 🎯 Conclusion

Congratulations!  
By completing these 30 projects, you have learned **how to manipulate the filesystem**, **secure files**, **automate tasks**, and **build real tools** using only **Python's standard library**.  
This skill set will make you much faster, safer, and more effective when managing projects and systems.
