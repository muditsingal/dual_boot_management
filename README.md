# Navigating the world of dual boot and beyond
Dual boot can be challenging for users new to Linux. Dual boot is a powerful tool that allows developers, researchers, and hobbyists to tinker with different OS configurations and harness the true potential of a system. Other alternatives to dual boot are available with a few limitations:

1. Virtual machines (such as [VirtualBox](https://www.virtualbox.org/)) are flexible but hardware acceleration and working with I/O ports can be challenging.
2. [WSL 2 (Windows subsystem for Linux)](https://learn.microsoft.com/en-us/windows/wsl/) is a great tool and comes with numerous features and Linux capabilities that you can use without setting up a VM or dual boot machine. I haven't used it extensively; hence I am not sure how well hardware interfacing works on it.

## Overview of the most common steps to dual boot your system (Ubuntu)

**Here's a [video link](https://www.youtube.com/watch?v=-iSAyiicyQY) that I usually refer to when in doubt for ubuntu**

1.	Make a partition using Create and format hard disk partitions (Disk management). I'd suggest a minimum of 30 GB of space for your Linux partition, however you should allocate more if you can afford it. Just keep the partition as free unallocated space.
2. Download the iso image of the OS you want to use. For Ubuntu go to: [Ubuntu downloads](https://ubuntu.com/download/desktop)
3. Make a bootable USB by using software such as [Balena Etcher](https://etcher.balena.io/) or [Ventoy](https://www.ventoy.net/en/index.html). I have found Ventoy to be a life-changing tool, where you can use your bootable USB for storing multiple ISO images, alongside other files for daily usage. However, Ventoy may not work for some systems.
4. Next, shutdown your system and boot into the UEFI boot select menu. You should see the removable USB as a boot medium.
5. Boot into the live bootable USB.
6. Setup your Linux system as you would into the partition created. Here's a useful link for [Ubuntu](https://www.youtube.com/watch?v=-iSAyiicyQY) (Thanks Ksk Royal for making this amazing video tutorial).

## How to undo dual boot or clean-up multi-boot systems!

You have worked with dual boot on your system and your project has been completed. You now want to remove the partition so that you can get more room on your primary disk (or any other reason). If you are like me, you deleted the Linux partition from windows disk management tool and added the space as a volume in your system. Well, though that works, the Grub Boot Loader is still configured in your system's UEFI boot loader. Once you reboot, you may notice that the other boot options are still visible in your boot menu! You shouldn't leave this as is, because if you have other boot options they may not work! Also, if you change your system configuration later on, you may not be able to properly boot into the desired OS.

I have looked into 2 ways to properly reconfigure the BIOS boot menu:
<ol>
    <li><b>The easy way:</b> Using the boot-repair-disk tool. You can follow this <a href="https://youtu.be/oLJczJBjhj0?si=9_K5uyrKA9Nn-ib4">video tutorial</a> or the steps given below: </li>
        <ol type="i">
            <li>Download the ISO image using the link: <a href="https://sourceforge.net/projects/boot-repair-cd/">boot-repair-disk</a></li>
            <li>Create a bootable disk with this ISO image using Balena, Ventoy, or any other tool you are comfortable with.</li>
            <li>Now, boot into this option and should be visible as UEFI removable partition (or similar option). Once in, you should be welcomed by a primitive Linux home. The Boot Repair should automatically start.</li>
            <li>If not, launch it using the icon that looks like a spanner</li>
            <li>If the tool asks to update it, select yes and let it update. After the update is complete, the tool will continue scanning your system for potential boot related issues.</li>
            <li>In a few minutes the tool will finish its diagnosis, and you you can then choose to simply select <b>Recommended repair</b> which should normally work. You can create a <b>BootInfo Summary</b> which will list all the existing bootloaders.</li>
            <li>Once the "Recommended Repair" is complete, you should only see the boot options that actually work!
            </li>
        </ol>
    <li><b>The not so easy way:</b> Manually deleting boot options from Windows. I know it sounds scary, but don't worry, the step-by-step guide will get you through the process effortlessly. I followed these 2 links to successfully fix my UEFI boot menu: <a href="https://unix.stackexchange.com/questions/552728/removed-both-Linux-installations-but-bios-still-shows-them-in-boot-options">Unix Stack Exchange</a> and <a href="https://dev.to/spectrumcetb/how-to-remove-ubuntu-completely-from-a-dual-boot-pc-uefi-3f12#:~:text=You%20will%20still%20find%20ubuntu,step%20is%20to%20remove%20it">Dev guide by Silla Priyadarshni</a>. Make sure to follow from the first to the last step in order:
    <ol type="i">
            <li>Open a command prompt (cmd.exe) as an Administrator.</li>
            <li>List the boot options from firmware boot manager using <code>bcdedit /enum firmware</code>. Each entry will have a description, look for the one that matches your previously installed boot OS.</li>
            <li>To delete an entry, use bcdedit /delete <identifier>, replacing <identifier> with the identifier GUID value of the corresponding entry. The command should look like:
            <code>bcdedit /delete {12345678-9abc-def0-1234-56789abcdef0}</code>.</li>
            <li>Start the diskpart tool by the command - <code>diskpart</code>.</li>
            <li>Identify UEFI partitions by:  <code>list disk</code>.</li>
            <li>Mount the concerned disk (if your boot is in other disk it might be disk 1 or 2, but for single disk systems it'll be disk 0): <code>select disk 0</code>. </li>
            <li>Check all the partitions of the selected disk using: <code>list partition</code> and identify the <b>uefi system partition</b>.</li>
            <li>Mount the concerned partition: <code>select partition 3</code>. In my case, the uefi partition system had partition number 3, it might vary in your case.</li>
            <li>Assign a label to the partition using: <code>assign letter=t</code> (It can be any letter). This will mount your partition EFI partition (approx. 100 mb) in file explorer.</li>
            <li>Exit the diskpart tool using: <code>exit</code>.</li>
            <li>Now access the file system of the mounted partition using the letter which you have assigned in the CMD line. I am entering <code>t:</code>.</li>
            <li>List the contents of the partition using: <code>dir</code>.</li>
            <li>Now we have to change folder so type <code>cd efi</code> and type <code>dir</code> to see the contents of efi folder.</li>
            <li>Now you will see the list where a folder named of Linux that you have installed once (like Ubuntu, fedora, etc.). That's the folder which we want to delete. Type <code>rd /s</code>. I entered <code>rd ubuntu /s</code>. Type Y to conform deletion.</li>
            <li>Type <code>dir</code> to ensure the folder is deleted</li>
            <li>Finally, restart your PC to apply changes!</li>
            </ol>
    </li>
</ol>
Congratulations! You can now recover the original state of your PC from any number of OS configuration changes!
<br>

## How to give your PC an external soul (Dual boot using an external boot drive)

Sometimes, you do not have (or cannot add) sufficient storage in your PC (a common case for laptops). Now, you might be frustrated how you can allocate sufficient storage for another partition in which you will keep your Linux distro. Well you are in luck! You can use an external SSD to create a dual boot system, just like you'd use an internal SSD. 

Before we begin, I would like to give a few disclaimers:

1. You might face issues while booting with your original OS (eg. Windows), as the BIOS may not be able to detect all necessary partitions (grub bootloader).
2. Make sure you use a USB 3.0+ device and port for optimal speeds.
3. Have a backup ready of your most important files. As suggested for all new dual boot setups, its best to have a backup in case things go sideways.
4. You will need 2 USB ports - One for the external SSD and another for the bootable pendrive. You can use a USB hub if necessary.

Now to the good stuff! Follow these steps for setting up dual boot using an external SSD:

1. Buy an external SSD with ample storage capacity something like: https://a.co/d/dfzWilH should do.
2. Mount both the external SSD and the bootable USB into your PC and restart your PC into the BIOS boot menu.
3. Now follow the same steps that you would to setup a normal dual boot system. Carefully select the external SSD as the partition for installing your Linux Distro.
4. After your Linux installation is done, restart into the BIOS boot menu while ensuring that the external SSD is mounted to your system.
5. Select the Linux distro that matches the name of your boot media.

Congratulations! You can now work in your favorite Linux distro without running out of space. 
This installation is very flexible and reliable. I have been using this setup as my daily dev environment for about a year now and haven't faced any issues.


P.S. Thank you for reading the article. I am by no means an expert of dual booting systems, this is just what I have learned through my experiences. Feel free to share your thoughts on this article and if there're any errors in any of the above sections. 