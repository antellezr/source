---
title: Script Documentation
weight: 1
---

# Downloads Organizer Script Documentation

## Purpose

This document describes the structure and functionality of a Python script that organizes files in the Downloads directory on macOS by categorizing them into predefined folders based on file size, type, and directory status.

## Audience

Individuals with basic programming knowledge who wish to automate file organization tasks.

## Overview

The script automates the organization of files within the **Downloads** directory. It:

1. Groups files exceeding a specified size into a dedicated folder.
2. Separates files without extensions into a separate folder.
3. Categorizes files with extensions into subdirectories based on their extension.
4. Moves all subdirectories into a designated folder.

This document provides a comprehensive explanation of how the script functions and how users can execute it.

{{% details "Show complete script" %}}
### Script Code

```python
import os
import shutil

# Define the root directory (Downloads folder) and the main folder structure
DOWNLOADS_DIR = os.path.expanduser("~/Downloads")
BIG_FILES_DIR = os.path.join(DOWNLOADS_DIR, "01-BIG FILES")
UNKNOWN_DIR = os.path.join(DOWNLOADS_DIR, "02-UNKNOWN")
FILE_TYPES_DIR = os.path.join(DOWNLOADS_DIR, "03-FILE TYPES")
FOLDERS_DIR = os.path.join(DOWNLOADS_DIR, "04-FOLDERS")

# Ensure the main directories exist
os.makedirs(BIG_FILES_DIR, exist_ok=True)
os.makedirs(UNKNOWN_DIR, exist_ok=True)
os.makedirs(FILE_TYPES_DIR, exist_ok=True)
os.makedirs(FOLDERS_DIR, exist_ok=True)

# Exclude the script file itself and main folders
SCRIPT_NAME = os.path.basename(__file__)
MAIN_FOLDERS = {"01-BIG FILES", "02-UNKNOWN", "03-FILE TYPES", "04-FOLDERS"}

def get_file_size(file_path):
    """Get the size of a file in bytes."""
    return os.path.getsize(file_path)

def move_file(file_path, target_dir):
    """Move a file to the target directory, creating the directory if needed."""
    os.makedirs(target_dir, exist_ok=True)
    shutil.move(file_path, target_dir)

def organize_files(directory):
    """Organize files in the given directory based on their extensions."""
    for item in os.listdir(directory):
        item_path = os.path.join(directory, item)

        # Skip the script itself and main folders
        if item == SCRIPT_NAME or item in MAIN_FOLDERS:
            continue

        # Move existing folders to 04-FOLDERS
        if os.path.isdir(item_path):
            move_file(item_path, FOLDERS_DIR)
            continue

        # Skip hidden files
        if item.startswith("."):
            continue

        # Handle files larger than 500 MB
        if get_file_size(item_path) > 500 * 1024 * 1024:
            move_file(item_path, BIG_FILES_DIR)
            continue

        # Handle files without extensions
        if "." not in item:
            move_file(item_path, UNKNOWN_DIR)
            continue

        # Organize by file extension
        ext = item.split(".")[-1].lower()
        ext_dir = os.path.join(FILE_TYPES_DIR, ext.upper())
        move_file(item_path, ext_dir)

def process_big_files():
    """Organize files inside the BIG_FILES_DIR by their extensions."""
    for item in os.listdir(BIG_FILES_DIR):
        item_path = os.path.join(BIG_FILES_DIR, item)

        # Skip directories and hidden files
        if os.path.isdir(item_path) or item.startswith("."):
            continue

        # Organize by file extension
        if "." in item:
            ext = item.split(".")[-1].lower()
            ext_dir = os.path.join(BIG_FILES_DIR, ext.upper())
            move_file(item_path, ext_dir)

# Run the script
if __name__ == "__main__":
    print("Organizing files in the Downloads folder...")
    organize_files(DOWNLOADS_DIR)
    print("Organizing files in the '01-BIG FILES' folder...")
    process_big_files()
    print("Organization complete.")

```

{{% /details %}}


## Prerequisites

Ensure the following before running the script:

* **Python**: Install Python version 3.6 or later.
* **Permissions**: Confirm you have read and write access to the Downloads directory.
* **Terminal**: Basic understanding of using the terminal or command prompt to execute Python scripts.

---

## Running the Script

To execute the script, follow these steps:

1. Save the script file as `organize_downloads.py` in your Downloads directory.
2. Open a terminal or command prompt.
3. Navigate to the Downloads directory:

   ```bash
   cd ~/Downloads
   ```

4. Run the script using the following command:

   ```bash
   python organize_downloads.py
   ```

   The script displays messages indicating its progress.

---

## Script Structure and Explanation

This section provides detailed information about the script’s structure and functionality.

### Initialization

The script begins by defining the **Downloads** directory and four subdirectories for file organization:

```python
DOWNLOADS_DIR = os.path.expanduser("~/Downloads")
BIG_FILES_DIR = os.path.join(DOWNLOADS_DIR, "01-BIG FILES")
UNKNOWN_DIR = os.path.join(DOWNLOADS_DIR, "02-UNKNOWN")
FILE_TYPES_DIR = os.path.join(DOWNLOADS_DIR, "03-FILE TYPES")
FOLDERS_DIR = os.path.join(DOWNLOADS_DIR, "04-FOLDERS")
```

Each folder serves a specific purpose:

* **01-BIG FILES**: Files larger than 500 MB.
* **02-UNKNOWN**: Files without extensions.
* **03-FILE TYPES**: Files categorized by their extensions.
* **04-FOLDERS**: Existing subdirectories from the Downloads directory.

#### Directory Creation

The script ensures these folders exist and prevents errors if the directories are already present.

```python
os.makedirs(directory_name, exist_ok=True)
```

---

### Utility Functions

#### `get_file_size`

This function calculates the size of a file in bytes and identifies files that exceed the 500 MB size threshold.

```python
def get_file_size(file_path):
    return os.path.getsize(file_path)
```

#### `move_file`

This function moves a file to a specified directory.

```python
def move_file(file_path, target_dir):
    os.makedirs(target_dir, exist_ok=True)
    shutil.move(file_path, target_dir)
```

---

### Main Functions

#### `organize_files` Function

This function processes the contents of the Downloads directory:

```python
def organize_files(directory):
    for item in os.listdir(directory):
        item_path = os.path.join(directory, item)
        
        if item == SCRIPT_NAME or item in MAIN_FOLDERS:
            continue

        if os.path.isdir(item_path):
            move_file(item_path, FOLDERS_DIR)
        elif item.startswith("."):
            continue
        elif get_file_size(item_path) > 500 * 1024 * 1024:
            move_file(item_path, BIG_FILES_DIR)
        elif "." not in item:
            move_file(item_path, UNKNOWN_DIR)
        else:
            ext = item.split(".")[-1].lower()
            ext_dir = os.path.join(FILE_TYPES_DIR, ext.upper())
            move_file(item_path, ext_dir)
```

The script follows a structured approach to sort and categorize files by:

1. **Skipping Specific Items**: The script skips:
   * The script file itself.
   * Predefined main folders.
   * Hidden files (files starting with a dot).

2. **Managing Large Files**: Files larger than 500 MB are moved to **01-BIG FILES**.

3. **Identifying Files Without Extensions**: These are moved to **02-UNKNOWN**.

4. **Categorizing Files with Extensions**: Files are categorized into extension-specific subdirectories under **03-FILE TYPES**.

5. **Subdirectory Handling**: Subdirectories are moved to **04-FOLDERS**.

#### `process_big_files` Function

After organizing files, the `process_big_files` function categorizes files within the **01-BIG FILES** folder by their extensions:

```python
def process_big_files():
    for item in os.listdir(BIG_FILES_DIR):
        item_path = os.path.join(BIG_FILES_DIR, item)
        
        if os.path.isdir(item_path) or item.startswith("."):
            continue

        if "." in item:
            ext = item.split(".")[-1].lower()
            ext_dir = os.path.join(BIG_FILES_DIR, ext.upper())
            move_file(item_path, ext_dir)
```

---

### Script Execution

The script’s execution starts with:

```python
if __name__ == "__main__":
    print("Organizing files in the Downloads folder...")
    organize_files(DOWNLOADS_DIR)
    print("Organizing files in the '01-BIG FILES' folder...")
    process_big_files()
    print("Organization complete.")
```

The script follows the next steps:

1. Calls `organize_files` to process the Downloads directory.
2. Calls `process_big_files` to handle large files.
3. Displays messages to indicate progress and completion.
