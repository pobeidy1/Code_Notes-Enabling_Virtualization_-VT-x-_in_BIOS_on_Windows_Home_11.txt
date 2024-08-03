# Protocol for Enabling Virtualization (VT-x) in BIOS on Windows 11 Home

    ### Step 1: Enter BIOS Setup
        
    1. **Restart Your Computer**:
        - Save any open documents and close all applications.
        - Restart your computer.
    2. **Access BIOS**:
        - During the startup, press the BIOS key (commonly `Del`, `F2`, `F10`, or `Esc`). The key varies by manufacturer and is typically displayed during the startup process.
    
    ### 2: Enable Virtualization Technology (VT-x)
    
    1. **Navigate to Advanced Settings**:
        - Use the arrow keys to navigate to the `Advanced` tab in the BIOS setup utility.
    2. **Locate Virtualization Settings**:
        - Find the option labeled `Intel Virtualization Technology`, `Intel VT-x`, `Virtualization Technology`, or similar.
    3. **Enable Virtualization**:
        - Select the virtualization option and change it to `Enabled`.
    
    ### 3: Save and Exit BIOS
    
    1. **Save Changes**:
        - Navigate to the `Exit` tab.
        - Select `Save Changes and Exit` or a similar option.
    2. **Confirm Changes**:
        - Confirm that you want to save the changes and exit BIOS. Your computer will restart.
        
### Step 2: Verify Virtualization in Windows
    1. **Open Command Prompt**:
        - Press `Win + R` to open the Run dialog box.
        - Type `cmd` and press `Enter` to open the Command Prompt.
    2. **Check System Information**:
        - In the Command Prompt, type `systeminfo.exe` and press `Enter`.
        - Look for the line that mentions `Hyper-V Requirements`. If virtualization is enabled, it should indicate that "VM Monitor Mode Extensions," "Virtualization Enabled In Firmware," and other related features are available.
        
### Step 3: Enable Hyper-V on Windows 11
    1. **Create a Batch File**:
        - Open Notepad or any text editor.
        - Copy and paste the following script into the text editor:
        
        ```
        batchCopy code
        @echo off
        pushd "%~dp0"
        
        :: Generate a list of Hyper-V related packages
        dir /b %SystemRoot%\\servicing\\Packages\\*Hyper-V*.mum >hv.txt
        
        :: Add each Hyper-V package
        for /f %%i in ('findstr /i . hv.txt 2^>nul') do (
            dism /online /norestart /add-package:"%SystemRoot%\\servicing\\Packages\\%%i"
        )
        
        :: Delete the temporary file
        del hv.txt
        
        :: Enable the Hyper-V feature
        dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
        
        pause
        
        ```
        
    2. **Save the Batch File**:
        - Save the file with a`.bat` extension, for example, `enable_hyperv.bat`.
    3. **Run the batch file as an administrator**:
        - Right-click the saved batch file.
        - Select "Run as administrator".
- Step 4: Verify Hyper-V Installation
    1. **Check Hyper-V Installation**:
        - Open Command Prompt as administrator.
        - Type `systeminfo.exe` and press `Enter`.
        - Ensure that the `Hyper-V Requirements` section indicates that Hyper-V is enabled.

### Additional Information

- **Hyper-V Usage**:
    - Hyper-V is Microsoft's hypervisor software that allows you to create and run virtual machines (VMs) on your PC.
    - It is useful for testing unstable applications or using features from different operating systems without affecting the host PC.
- **Full-Screen VM**:
