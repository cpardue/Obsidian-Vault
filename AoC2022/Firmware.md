The Story

![Image for banner](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/b9d0777ff1cdcf114124a035dd4cecc4.png)  

Check out Saqib's video walkthrough for Day 20 [here](https://www.youtube.com/watch?v=1qc7C4h36ZQ)!  

  

We can now learn more about the mysterious device found in Santa's workshop. Elf Forensic McBlue has successfully been able to find the `device ID`. Now that we have the hardware `device ID`, help Elf McSkidy reverse the encrypted firmware and find interesting endpoints for IoT exploitation.  

  

**Learning Objectives**

-   What is firmware reverse engineering
-   Techniques for extracting code from the firmware
-   Extracting hidden keys from an encrypted firmware
-   Modifying and rebuilding a firmware

**What is Firmware Reverse Engineering**

Every embedded system, such as cameras, routers, smart watches etc., has pre-installed firmware, which has its own set of instructions running on the hardware's processor. It enables the **hardware to communicate with other software running on the device**. The firmware provides low-level control for the designer/developer to make changes at the root level. 

Reverse engineering is working your way back through the code to figure out how it was built and what it does. Firmware reverse engineering is extracting the original code from the firmware binary file and verifying that the code does not carry out any malicious or unintended functionality like undesired network communication calls. **Firmware reversing is usually done for security reasons** to ensure the safe usage of devices that may have critical vulnerabilities leading to possible exploitation or data leakage. Consider a smart watch whose firmware is programmed to send all incoming messages, emails etc., to a specific IP address without any indication to the user.

  

**Firmware Reversing Steps**

-   The firmware is first obtained from the vendor's website or extracted from the device to perform the analysis.
-   The obtained/extracted firmware, usually a binary file, is first analysed to figure out its type (bare metal or OS based). 
-   It is verified that the firmware is either encrypted or packed. The encrypted firmware is more challenging to analyse as it usually needs a tricky workaround, such as reversing the previous non-encrypted releases of the firmware or performing hardware attacks like [Side Channel Attacks (SCA)](https://en.wikipedia.org/wiki/Side-channel_attack) to fetch the encryption keys. 
-   Once the encrypted firmware is decrypted, different techniques and tools are used to perform reverse engineering based on type.

**Types of Firmware Analysis**  

Firmware analysis is carried out through two techniques, Static & Dynamic.

**Static Analysis**

Static analysis involves an essential examination of the binary file contents, performing its reverse engineering, and reading the assembly instructions to understand the functionality. This is done through multiple commonly used command line utilities and binary analysis tools such as:

-   **[BinWalk](https://github.com/ReFirmLabs/binwalk):** A firmware extraction tool that extracts code snippets inside any binary by searching for signatures against many standard binary file formats like `zip, tar, exe, ELF,` etc. Binwalk has a database of binary header signatures against which the signature match is performed. The common objective of using this tool is to extract a file system like `Squashfs, yaffs2, Cramfs, ext*fs, jffs2,` etc., which is embedded in the firmware binary. The file system has all the application code that will be running on the device.
-   **[Firmware ModKit (FMK)](https://www.kali.org/tools/firmware-mod-kit/)**: FMK is widely used for firmware reverse engineering. It extracts the firmware using `binwalk` and outputs a directory with the firmware file system. Once the code is extracted, a developer can modify desired files and repack the binary file with a single command.   
    
-   **[FirmWalker](https://github.com/craigz28/firmwalker)**: Searches through the extracted firmware file system for unique strings and directories like `etc/shadow`, `etc/passwd`, `etc/ssl`, special keywords like `admin, root, password`, etc., vulnerable binaries like `ssh, telnet, netcat` etc.

**Dynamic Analysis**

Firmware dynamic analysis involves running the firmware code on actual hardware and observing its behaviour through emulation and hardware/ software based debugging. One of the significant advantages of dynamic analysis is to analyse unintended network communication for identifying data pilferage. The following tools are also commonly used for dynamic analysis:

-   **[Qemu](https://www.qemu.org/)**: Qemu is a free and open-source emulator and enables working on cross-platform environments. The tool provides various ways to emulate binary firmware for different architectures like Advanced RISC Machines (ARM), Microprocessors without Interlocked Pipelined Stages (MIPS), etc., on the host system. Qemu can help in full-system emulation or a single binary emulation of ELF (Executable and Linkable Format) files for the Linux system and many different platforms.
-   **[Gnu DeBugger (GDB)](https://www.sourceware.org/gdb/)**[:](https://www.sourceware.org/gdb/) GDB is a dynamic debugging tool for emulating a binary and inspecting its memory and registers. GDB also supports remote debugging, commonly used during firmware reversing when the target binary runs on a separate host and reversing is carried out from a different host.

**Shall We Reverse the Firmware? Let's Do It!**  

Launch the virtual machine by clicking `Start Machine` at the top right of this task. The machine will load in a split-screen view. If it does not, click the `Show Split View` button to view the machine. Wait for 1-2 minutes for the machine to load completely. In case you refresh your web browser page, it will appear as if the target machine has restarted/rebooted (as it shows the default terminal output that is shown after booting up). In reality, the machine state remains the same, and your progress is not lost. When reversing the firmware, use the password `Santa1010` if prompted for a **sudo** password. 

  

After identifying the device id, McSkidy extracted the encrypted firmware from the device. To reverse engineer it, she needs an unencrypted version of the firmware first - luckily, she found it online. Open the terminal and run the `dir` command. You will see the following directories:

Terminal

```shell-session
ubuntu@machine$ dir
bin  firmware-mod-kit  bin-unsigned
```

The `bin` folder contains the firmware binary, while the `firmware-mod-kit` folder contains the script for extracting and modifying the firmware.

In this exercise, we will primarily be using two tools:

-   **Binwalk**: For verifying encryption and can also be used to decrypt the firmware (Usage: `binwalk -E -N`)
-   **Firmware Mod Kit (FMK)**: Library for firmware extraction and modification (Usage: `extract-firmware.sh`)

Now coming over to the task, we will perform reversing step by step.

**Step 1: Verifying Encryption**  
In this step, McSkidy will verify whether the binary `firmwarev2.2-encrypted.gpg` is encrypted through [file entropy analysis](https://en.kali.tools/?p=1634). First, change the directory to the `bin` folder by entering the command `cd bin`. She will then use the `binwalk` tool to verify the encryption using the command `binwalk -E -N firmwarev2.2-encrypted.gpg.`

Terminal

```shell-session
ubuntu@machine:-/bin$ binwalk -E -N firmwarev2.2-encrypted.gpg 

DECIMAL       HEXADECIMAL     ENTROPY
--------------------------------------------------------------------------------
0             0x0             Rising entropy edge (0.988935)

```

In the above output, the **rising entropy edge** means that the file is probably encrypted and has increased randomness.   

**Step 2: Finding Unencrypted Older Version**  
Since the latest version is encrypted, McSkidy found an older version of the same firmware. The version is located in the `bin-unsigned` folder. _Why was she looking for an older version?_ Because she wants to find encryption keys that she may use to decrypt the original firmware and reverse engineer it. McSkidy has decided to use the famous `FMK` tool for this purpose. To extract the firmware, change the directory by entering the command `cd ..` and then `cd bin-unsigned`. She extracted the firmware by issuing the following command.

Terminal

```shell-session
ubuntu@machine:-/bin-unsigned$ extract-firmware.sh firmwarev1.0-unsigned 
Firmware Mod Kit (extract) 0.99, (c)2011-2013 Craig Heffner, Jeremy Collake

Scanning firmware...

Scan Time:     2022-11-21 17:52:44
Target File:   /home/ubuntu/bin/firmwarev1.0-unsigned
MD5 Checksum:  b141dc2678be3a20d4214b93354fedc0
Signatures:    344

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             TP-Link firmware header, firmware version: 0.-15360.3, image version: "", product ID: 0x0, product version: 138412034, kernel load address: 0x0, kernel entry point: 0x80002000, kernel offset: 4063744, kernel length: 512, rootfs offset: 849104, rootfs length: 1048576, bootloader offset: 2883584, bootloader length: 0
13344         0x3420          U-Boot version string, "U-Boot 1.1.4 (Apr  6 2016 - 11:12:23)"
13392         0x3450          CRC32 polynomial table, big endian
14704         0x3970          uImage header, header size: 64 bytes, header CRC: 0x5A946B00, created: 2016-04-06 03:12:24, image size: 35920 bytes, Data Address: 0x80010000, Entry Point: 0x80010000, data CRC: 0x510235FE, OS: Linux, CPU: MIPS, image type: Firmware Image, compression type: lzma, image name: "u-boot image"
14768         0x39B0          LZMA compressed data, properties: 0x5D, dictionary size: 33554432 bytes, uncompressed size: 93944 bytes
131584        0x20200         TP-Link firmware header, firmware version: 0.0.3, image version: "", product ID: 0x0, product version: 138412034, kernel load address: 0x0, kernel entry point: 0x80002000, kernel offset: 3932160, kernel length: 512, rootfs offset: 849104, rootfs length: 1048576, bootloader offset: 2883584, bootloader length: 0
132096        0x20400         LZMA compressed data, properties: 0x5D, dictionary size: 33554432 bytes, uncompressed size: 2494744 bytes
1180160       0x120200        Squashfs filesystem, little endian, version 4.0, compression:lzma, size: 2812026 bytes, 600 inodes, blocksize: 131072 bytes, created: 2022-11-17 11:14:32

Extracting 1180160 bytes of tp-link header image at offset 0
Extracting squashfs file system at offset 1180160
3994112
3994112
0
Extracting squashfs files...
Firmware extraction successful!
Firmware parts can be found in '/home/ubuntu/bin-unsigned/fmk/*'
```

  
Step 3: Finding Encryption Keys  
The original firmware is [gpg](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) protected, which means that we need to find a public, and private key and a paraphrase to decrypt the originally signed firmware. We know that the unencrypted firmware is extracted successfully and stored in the `fmk` folder. The easiest way to find keys is by using the `grep` command. The `-i` flag in the grep command ignores case sensitivity while the `-r` operator recursively searches in the current directory and subdirectories.  

Terminal

```shell-session
ubuntu@machine:~bin-unsigned$ grep -ir key
Binary file firmwarev2.2-encrypted.gpg matches
Binary file firmwarev1.0-unsigned matches
Binary file fmk/image_parts/rootfs.img matches
Binary file fmk/rootfs/usr/sbin/dropbearmulti matches
Binary file fmk/rootfs/usr/sbin/dhcp6ctl matches
Binary file fmk/rootfs/usr/sbin/dhcp6c matches
Binary file fmk/rootfs/usr/sbin/xl2tpd matches
Binary file fmk/rootfs/usr/sbin/pppd matches
Binary file fmk/rootfs/usr/sbin/dhcp6s matches
Binary file fmk/rootfs/usr/bin/httpd matches
fmk/rootfs/gpg/public.key:-----BEGIN PGP PUBLIC KEY BLOCK-----
fmk/rootfs/gpg/public.key:-----END PGP PUBLIC KEY BLOCK-----
fmk/rootfs/gpg/private.key:-----BEGIN PGP PRIVATE KEY BLOCK-----
fmk/rootfs/gpg/private.key:-----END PGP PRIVATE KEY BLOCK-----


```

Bingo! We have the **public and private keys**, but what about the **paraphrase** usually used with the private key to decrypt a gpg encrypted file?  

Let's find the paraphrase through the same `grep` command.

Terminal

```shell-session
ubuntu@machine:~bin-unsigned$ grep -ir paraphrase
fmk/rootfs/gpg/secret.txt:PARAPHRASE: [OUTPUT INTENTIONALLY HIDDEN]


```

McSkidy has finally located the public and private keys and the paraphrase as well.  

Step 4: Decrypting the Encrypted Firmware  
Now that we have the keys, let's import them using the following command:

Terminal

```shell-session
ubuntu@machine:~bin-unsigned$ gpg --import fmk/rootfs/gpg/private.key 
gpg: key 56013838A8C14EC1: "McSkidy " not changed
gpg: key 56013838A8C14EC1: secret key imported
gpg: Total number processed: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:  secret keys unchanged: 1


```

While importing the private key, you will be asked to enter the paraphrase. Enter the one you found in **Step 3**.

Importing the public key.

Terminal

```shell-session
ubuntu@machine:~bin-unsigned$ gpg --import fmk/rootfs/gpg/public.key 
gpg: key 56013838A8C14EC1: "McSkidy " not changed
gpg: Total number processed: 1
gpg:              unchanged: 1


```

We can list the secret keys.

Terminal

```shell-session
ubuntu@machine:~bin-unsigned$ gpg --list-secret-keys
/home/ubuntu/.gnupg/pubring.kbx
-------------------------------
sec   rsa3072 2022-11-17 [SC] [expires: 2024-11-16]
      514B4994E9B3E47A4F89507A56013838A8C14EC1
uid           [ unknown] McSkidy 
ssb   rsa3072 2022-11-17 [E] [expires: 2024-11-16]



```

Once the keys are imported, McSkidy decrypts the firmware using the `gpg` command. Again change the directory by entering the command `cd ..` and then `cd bin`.

Terminal

```shell-session
ubuntu@machine:~bin$ gpg firmwarev2.2-encrypted.gpg
gpg: WARNING: no command supplied.  Trying to guess what you mean ...
gpg: encrypted with 3072-bit RSA key, ID 1A2D5BB2F7076FA8, created 2022-11-17
      "McSkidy "

```

After decryption, once you issue the `ls` command, the decrypted file result will look like the following:  

Terminal

```shell-session
ubuntu@machine:~bin$ ls -lah

-rw-rw-r-- 1 test test 3.9M Dec  7 19:10 firmwarev2.2-encrypted
-rw-rw-r-- 1 test test 3.6M Dec  1 05:45 firmwarev2.2-encrypted.gpg
```

**Step 5: Reversing the** **Original** **Encrypted Firmware**  
This is the simplest step, and we can use `binwalk` or `FMK` to extract code from the recently unencrypted firmware. In this example, we will be using `FMK` to extract the code.  

Terminal

```shell-session
ubuntu@machine:~bin$ extract-firmware.sh firmwarev2.2-encrypted
Firmware Mod Kit (extract) 0.99, (c)2011-2013 Craig Heffner, Jeremy Collake

Scanning firmware...

Scan Time:     2022-11-21 18:05:54
Target File:   /home/ubuntu/bin/firmwarev2.2-encrypted
MD5 Checksum:  af379de1dac784b3eab78339fb203fbc
Signatures:    344

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             TP-Link firmware header, firmware version: 0.-15360.3, image version: "", product ID: 0x0, product version: 138412034, kernel load address: 0x0, kernel entry point: 0x80002000, kernel offset: 4063744, kernel length: 512, rootfs offset: 849104, rootfs length: 1048576, bootloader offset: 2883584, bootloader length: 0
13344         0x3420          U-Boot version string, "U-Boot 1.1.4 (Apr  6 2016 - 11:12:23)"
13392         0x3450          CRC32 polynomial table, big endian
14704         0x3970          uImage header, header size: 64 bytes, header CRC: 0x5A946B00, created: 2016-04-06 03:12:24, image size: 35920 bytes, Data Address: 0x80010000, Entry Point: 0x80010000, data CRC: 0x510235FE, OS: Linux, CPU: MIPS, image type: Firmware Image, compression type: lzma, image name: "u-boot image"
14768         0x39B0          LZMA compressed data, properties: 0x5D, dictionary size: 33554432 bytes, uncompressed size: 93944 bytes
131584        0x20200         TP-Link firmware header, firmware version: 0.0.3, image version: "", product ID: 0x0, product version: 138412034, kernel load address: 0x0, kernel entry point: 0x80002000, kernel offset: 3932160, kernel length: 512, rootfs offset: 849104, rootfs length: 1048576, bootloader offset: 2883584, bootloader length: 0
132096        0x20400         LZMA compressed data, properties: 0x5D, dictionary size: 33554432 bytes, uncompressed size: 2494744 bytes
1180160       0x120200        Squashfs filesystem, little endian, version 4.0, compression:lzma, size: 2776905 bytes, 596 inodes, blocksize: 131072 bytes, created: 2016-04-06 03:20:50

Extracting 1180160 bytes of tp-link header image at offset 0
Extracting squashfs file system at offset 1180160
3957248
4063744
106496
Extracting 106496 byte footer from offset 3957248
Extracting squashfs files...
Firmware extraction successful!
Firmware parts can be found in '/home/ubuntu/bin/fmk/*'

```

McSkidy has finally been able to reverse the complete firmware and extract essential files she will use for IoT exploitation (next room). She has used the keys from an older version (1.0)  to decrypt the latest version (2.2) of the same firmware.