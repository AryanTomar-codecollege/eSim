# eSim 2.5 — Ubuntu 25.04 Compatibility Fixes

**Author:** Aryan Tomar  
**GitHub:** AryanTomar-codecollege  
**Fellowship:** FOSSEE Summer Fellowship 2026 — Screening Task 4  
**Institution:** Dronacharya College of Engineering  

---

## Overview

This repository contains fixes for dependency and compatibility issues encountered while installing eSim 2.5 on Ubuntu 25.04 (Plucky Puffin). The installation was performed inside VirtualBox on a Windows host machine. A total of 8 bugs were identified, 5 were fixed, and 3 were documented with proposed solutions.

---

## System Configuration

- **Host OS:** Windows 11
- **VM Software:** Oracle VirtualBox
- **Guest OS:** Ubuntu 25.04 (Plucky Puffin)
- **RAM Allocated:** 4096 MB
- **Disk Allocated:** 25 GB
- **eSim Version:** 2.5

---

## Bugs Found and Fixes Applied

---

### Bug 1 — Unsupported Ubuntu Version in Main Installer

**File:** `install-eSim.sh`

**Error:**
Detected Ubuntu Version:
Unsupported Ubuntu version: 25.04 ()

**Root Cause:** The main installer script uses a case statement to detect the Ubuntu version. Ubuntu 25.04 was not listed as a supported version, causing installation to abort immediately.

**Fix Applied:** Added a new case entry for Ubuntu 25.04 pointing to the existing 24.04 script as a compatible fallback.

**Status:** Fixed ✅

---

### Bug 2 — KiCad 6.0 PPA Has No Release for Ubuntu 25.04

**File:** `Ubuntu/install-eSim-scripts/install-eSim-24.04.sh`

**Error:**
Err:5 https://ppa.launchpadcontent.net/kicad/kicad-6.0-releases/ubuntu plucky Release
404 Not Found E: The repository does not have a Release file.

**Root Cause:** The installer assigns KiCad 6.0 PPA for all Ubuntu versions other than 24.04. KiCad 6.0 PPA has no release for Ubuntu 25.04, resulting in a 404 error.

**Fix Applied:** Changed the else branch to use KiCad 8.0 PPA which supports Ubuntu 25.04.

**Status:** Fixed ✅

---

### Bug 3 — KiCad Cannot Install Due to Missing libgit2-1.8

**File:** `Ubuntu/install-eSim-scripts/install-eSim-24.04.sh`

**Error:**
kicad : Depends: libgit2-1.8 (>= 1.8.0) but it is not installable
E: Unable to correct problems, you have held broken packages.

**Root Cause:** Both the KiCad PPA package and Ubuntu 25.04's own KiCad package strictly require libgit2-1.8. Ubuntu 25.04 ships libgit2-1.9 and skipped version 1.8 entirely, creating an unresolvable dependency conflict.

**Proposed Fix:**
- Solution 1: Compile and install libgit2-1.8 from source alongside libgit2-1.9
- Solution 2: Wait for KiCad package maintainers to update dependency requirements for libgit2-1.9

**Status:** Workaround Applied ⚠️

---

### Bug 4 — Unsupported Ubuntu Version in NGHDL Installer

**File:** `Ubuntu/install-nghdl.sh`

**Error:**
Detected Ubuntu Version:
Unsupported Ubuntu version: 25.04 ()

**Root Cause:** The NGHDL sub-installer has the same version check problem as Bug 1. Ubuntu 25.04 is not listed in the NGHDL installer's supported versions.

**Fix Applied:** Added Ubuntu 25.04 support to install-nghdl.sh and repacked nghdl.zip with the fixed scripts inside so the fix persists after extraction.

**Status:** Fixed ✅

---

### Bug 5 — libcanberra-gtk-module Unavailable in Ubuntu 25.04

**File:** `Ubuntu/install-nghdl-24.04.sh`

**Error:**
Package 'libcanberra-gtk-module' has no installation candidate

**Root Cause:** The package libcanberra-gtk-module has been removed in Ubuntu 25.04 repositories. The NGHDL installer tries to install it, causing installation to abort.

**Fix Applied:** Commented out the libcanberra installation line in the script.

**Status:** Fixed ✅

---

### Bug 6 — GHDL tar File Path Error

**File:** `Ubuntu/install-nghdl-24.04.sh`

**Error:**
tar: ghdl-4.1.0.tar.gz: Cannot open: No such file or directory
tar: Error is not recoverable: exiting now

**Root Cause:** The NGHDL installer uses a relative path to locate ghdl-4.1.0.tar.gz. When called from the parent directory, the relative path becomes invalid.

**Fix Applied:** Run the NGHDL installer directly from inside the nghdl directory to resolve the path correctly.

**Status:** Documented ✅

---

### Bug 7 — GHDL 4.1.0 Incompatible with LLVM 20

**File:** `Ubuntu/install-nghdl-24.04.sh`

**Error:**
Unhandled version llvm 20.1.2

**Root Cause:** GHDL 4.1.0 only supports LLVM up to version 17 or 18. Ubuntu 25.04 ships LLVM 20.1.2 which is not handled by GHDL's build configuration.

**Proposed Fix:** Upgrade to a newer GHDL version that supports LLVM 20, or manually install an older LLVM version alongside LLVM 20.

**Status:** Workaround Applied ⚠️

---

### Bug 8 — Installer Overwrites Manual Fixes on Every Run

**File:** `Ubuntu/install-eSim-scripts/install-eSim-24.04.sh`

**Issue:** The main installer uses unzip -o to extract nghdl.zip on every run. The -o flag forces overwrite of all existing files, destroying any manual patches made between runs.

**Root Cause:** No mechanism exists to preserve manual fixes between installation attempts. Every run starts fresh, making iterative bug fixing extremely difficult.

**Fix Applied:** Repacked nghdl.zip with all fixes already applied inside, so that even after extraction the fixed files are present.

**Status:** Fixed ✅

---

## Summary

- Total bugs found: 8
- Fixed properly: 5
- Workarounds applied: 2
- Documented with proposed fixes: 1

---

## Result

After applying all fixes, eSim 2.5 was successfully installed and launched on Ubuntu 25.04 inside VirtualBox.

---

## Files Modified

- `install-eSim.sh` — Added Ubuntu 25.04 version support
- `Ubuntu/install-eSim-scripts/install-eSim-24.04.sh` — Fixed KiCad PPA version
- `Ubuntu/install-nghdl.sh` — Added Ubuntu 25.04 version support
- `Ubuntu/install-nghdl-24.04.sh` — Removed unavailable libcanberra package, commented out GHDL and KiCad installations

---

## Contact

**Email:** tomararyan361@gmail.com  
**Submission:** FOSSEE Summer Fellowship 2026 — Task 4
