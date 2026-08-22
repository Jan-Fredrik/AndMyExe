<p align="center">
  <img src="images/AndMyExe_img2.png" alt="AndMyExe Logo" width="350">
</p>


# AndMyExe

**AndMyExe** is a simple Python tool for creating Windows `.exe` files with customizable version information and file metadata.

The program guides you through the process, asks what information should be included in the executable, generates the required Windows version information, and builds the final `.exe` using **PyInstaller**.

No manual PyInstaller setup is required — AndMyExe checks whether PyInstaller is installed and can install it for you if needed.

## Features

* Interactive command-line setup
* Automatically checks for PyInstaller
* Offers to install PyInstaller if it is missing
* Generates Windows version information automatically
* Customizable executable metadata:

  * Company Name
  * File Description
  * File Version
  * Product Version
  * Internal Name
  * Original Filename
  * Product Name
* Supports Windows-style four-part version numbers
* Option to package everything into a single `.exe` using `--onefile`
* Option to hide the console window using `--noconsole`
* Automatically builds the executable after configuration
* Places the finished executable in the `dist/` directory

## Requirements

* Windows
* Python 3
* pip

You do **not** need to install PyInstaller manually.

When AndMyExe starts, it checks whether PyInstaller is available.

If it is missing, you will be asked:

```text
PyInstaller is not installed.
Do you want to install PyInstaller now? (y/n)
```

If you choose yes, AndMyExe automatically runs:

```bash
python -m pip install pyinstaller
```

After the installation is complete, the program continues normally.

## Usage

Clone the repository:

```bash
git clone <repository-url>
cd AndMyExe
```

Run AndMyExe:

```bash
python create_exe_details
```

The program will first check for PyInstaller and then ask for the metadata you want to include in the executable.

Example:

```text
=== Generate VersionInfo and Build EXE ===

Company Name: Example Company
FileDescription: Example Application
FileVersion: 1.0.0.0
ProductVersion: 1.0.0.0
InternalName: ExampleApp
OriginalFilename: ExampleApp.exe
ProductName: Example Application

Build everything into one .exe file? (--onefile) (y/n): y
Hide the console window? (--noconsole) (y/n): n
```

After answering the questions, AndMyExe generates the version information and starts PyInstaller automatically.

When the build is complete, the executable can be found inside:

```text
dist/
```

## Metadata

AndMyExe allows you to configure several pieces of version information for the executable.

### Company Name

The company or organization associated with the executable.

Example:

```text
Example Company
```

### File Description

A short description of the application.

Example:

```text
Example Application
```

### File Version

The version number of the executable.

Example:

```text
1.0.0.0
```

### Product Version

The version number of the product.

By default, this can use the same value as the File Version.

Example:

```text
1.0.0.0
```

### Internal Name

The internal name of the application.

Example:

```text
ExampleApp
```

### Original Filename

The original filename associated with the executable.

Example:

```text
ExampleApp.exe
```

### Product Name

The name of the product or application.

Example:

```text
Example Application
```

## Version Numbers

Windows executable versions normally contain four numeric values:

```text
Major.Minor.Build.Revision
```

For example:

```text
1.0.0.0
2.5.12.0
3.1.4.7
```

AndMyExe automatically converts shorter version numbers into the four-part format required by the generated version information.

For example:

```text
1.2
```

becomes:

```text
1.2.0.0
```

## Build Options

Before building the executable, AndMyExe gives you two additional options.

### `--onefile`

Packages the application and its dependencies into a single `.exe` file.

```text
Build everything into one .exe file? (--onefile)
```

Choosing **yes** adds PyInstaller's:

```bash
--onefile
```

option.

### `--noconsole`

Prevents a console window from appearing when the executable is started.

```text
Hide the console window? (--noconsole)
```

Choosing **yes** adds:

```bash
--noconsole
```

This option is mainly useful for applications with a graphical user interface.

## How It Works

The basic AndMyExe workflow is:

```text
Start AndMyExe
       │
       ▼
Check for PyInstaller
       │
       ├── Installed ──────────────┐
       │                           │
       └── Missing                 │
             │                     │
             ▼                     │
       Ask to install              │
             │                     │
             ▼                     │
       Install with pip            │
             │                     │
             └─────────────────────┘
                       │
                       ▼
                Enter metadata
                       │
                       ▼
             Generate version_info.txt
                       │
                       ▼
              Select build options
                       │
                       ▼
                 Run PyInstaller
                       │
                       ▼
                    dist/
                       │
                       ▼
                 Finished .exe
```

## Project Structure

```text
AndMyExe/
├── create_exe_details
├── change_name.py
├── version_info.txt
├── *.spec
├── build/
├── dist/
├── .gitignore
└── README.md
```

### `create_exe_details`

The main AndMyExe script.

It:

1. Checks whether PyInstaller is installed.
2. Offers to install PyInstaller if necessary.
3. Collects executable metadata from the user.
4. Generates `version_info.txt`.
5. Asks which PyInstaller build options should be used.
6. Runs PyInstaller.
7. Creates the final executable.

### `change_name.py`

The Python file that AndMyExe packages into an executable.

Modify or replace this file to change what the generated executable actually does.

### `version_info.txt`

Automatically generated version information containing the metadata entered by the user.

This file is passed to PyInstaller when building the executable.

### `build/`

Temporary files generated by PyInstaller during the build process.

### `dist/`

Contains the finished executable.

The `build/` and `dist/` directories are normally excluded from Git.

## Viewing Executable Information

After building the `.exe`, its embedded version information can be inspected in Windows by right-clicking the file and opening:

```text
Properties
└── Details
```

## Manual PyInstaller Installation

If the automatic installation fails, PyInstaller can still be installed manually:

```bash
python -m pip install pyinstaller
```

Then run AndMyExe again:

```bash
python create_exe_details
```

## Important Note

The information added by AndMyExe is **file metadata**.

Changing fields such as the Product Name, Company Name, Internal Name, or Original Filename does not digitally sign the executable and does not verify the identity of its creator or publisher.

For trusted software distribution, executables should be digitally signed using an appropriate code-signing certificate.

