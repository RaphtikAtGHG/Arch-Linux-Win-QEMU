# Arch Linux install guide on Windows
> [!IMPORTANT]
> This will only work if your on a 64-bit release

## 1. Install prequesits

### 1.1 QEMU

You can download the prebuild installer on [this mirror site](https://qemu.weilnetz.de/w64)[^1] they are at the bottom of the explorer 
[^1]: This is not an official QEMU site, but it works.

Then just run the installer. e.g. ```qemu-w64-setup-20240720.exe```
> [!NOTE]
> UAC will probably notify you
1. Click next
2. Agree to the License
3. Set install location
4. Click finish
**Add to PATH**
1. Open Command prompt as Administrator
2. Insert `setx PATH "%PATH%;{QEMU_INSTALL_PATH}"` replacing {QEMU_INSTALL_PATH} to the path QEMU installed (default C:\Program Files\qemu)
3. Press enter
4. Restart CMD / Powershell
5. Verify by running `qemu-system-x86_64 --version`

### 1.2 Arch ISO
1. Go to [Arch Linux Install Page](https://archlinux.org/download/)[^2] and scroll down for your mirror
[^2]: Under https://archlinux.de/download there is a direct download button.

2. Press on the mirror and download the ISO
3. Move the iso to a dedicated folder for the VM

# 1.3 UEFI BIOS
1. Run `curl -Lo OVMF.fd https://retrage.github.io/edk2-nightly/bin/RELEASEX64_OVMF.fd` in the folder
2. Now you should have a file called OVMF.fd

## 2. Prepairing

### 2.1 Making the disk image
1. To make the disk-image in a terminal in the dedicated folder.
2. Execute the command: `qemu-img create -f qcow2 archlinux_hdd.img 180G` to create a 180GB big expanding image

### Make a batch file
**This step is recommended**
**DO NOT USE THIS ON THE FIRST BOOT IT WILL NOT WORK**
1. Create a new file called run_vm.bat
2. In the file enter this batch code:
   ```batch
    qemu-system-x86_64 -m {MEM_AMOUNT} -hda archlinux_dynamic.img -bios OVMF.fd
   ```
   replace `mem_amount` with the actual amount of memory to use in MB
3. Save

## 3. Installing
In the windows terminal execute: `qemu-system-x86_64 -m {MEM_AMOUNT} -cdrom {ARCH_ISO_NAME}.iso -hda archlinux_hdd.img -boot d -bios OVMF.fd -accel whpx -smp 4 -cpu qemu64`.
Now a QEMU Window should open up. Now enter everything into QEMU unless I say otherwise.
After the first reboot adter the install mkae sure to use `run_vm.ps1`

### You need to now setup everything yourself i hope you can do that

## Add qemu guest agent
1. Install the `qemu-guest-agent` package
2. Enable it using systemctl: `sudo systemctl enable qemu-guest-agent`
3. In `run-vm.ps1` at the qemu command add `-device virtio-balloon`
