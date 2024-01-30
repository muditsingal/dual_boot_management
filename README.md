# Navigating the world of dual boot and beyond
Dual boot can be challenging for users new to linux. Dual boot is a powerful tool that allows developers, researchers, and hobbists to tinker with different OS configurations and harness the true potential of a system. Other alternatives to dual boot are available with a few limitations: 

1. Virtual machines (such as [VirtualBox](https://www.virtualbox.org/)) are flexible but hardware acceleration and working with I/O ports can be challenging.
2. [WSL 2 (Windows subsystem for linux)](https://learn.microsoft.com/en-us/windows/wsl/) is a great tool and comes with numerous features and linux capabilities that you can use without setting up a VM or dual boot machine. I haven't used it extensively, hence I am not sure how well hardare interfacing works on it.

## Overview of the most common steps to dual boot your system

**Here's a [video link](https://www.youtube.com/watch?v=-iSAyiicyQY) that I usually refer to when in doubt for ubuntu**

1. Make a partition in using _Create and format hard disk partitions_ (Disk management). I'd suggest a minimum of 30 GB of space for your linux partition, however you should allocate more if you can afford it. Just keep the partition as free unallocated space.
2. Download the iso image of the OS you want to use. For Ubuntu go to: [Ubuntu downloads](https://ubuntu.com/download/desktop)
3. Make a bootable USB by using software such as [Balena Etcher](https://etcher.balena.io/) or [Ventoy](https://www.ventoy.net/en/index.html). I have found Ventoy to be a life-changing tool, where you can use your bootable USB for storing multiple ISO images, alongside other files for daily usage. However, Ventoy may not work for some systems.
4. Next, shutdown your system and boot into the UEFI boot select menu. You should see the removable USB as a boot medium.
5. Boot into the live bootable USB.
6. Setup your linux system as you would into the partition created. Here's a useful link for [Ubuntu](https://www.youtube.com/watch?v=-iSAyiicyQY) (Thanks Ksk Royal for making this amazing video tutorial).



## How to give your PC an external soul (Dual boot using an external boot drive)

Sometimes, the issue with dual boot is that you do not have (or cannot add) sufficient storage in your PC (a common case for laptops). Now, you might be frustrated how you can allocate sufficient storage for another partition in which you will keep your linux distro. Well you are in luck (I hope)! You can use an external SSD to create a dual boot system, just like you'd use an internal SSD. 

Before we begin, I would like to give a few disclaimers:

1. You might face issues while booting with your original OS (eg. Windows), as the BIOS may not be able to detect all necessary partitions (grub bootloader).
2. Make sure you use a USB 3.0 device and port for optimal speeds. 
3. Have a backup ready of your most important files. As suggested for all new dual boot setups, its best to have a backup in case things go sideways.
4. You will need 2 USB ports - One for the external SSD and another for the bootable pendrive. You can use a USB hub if necessary.

Now to the good stuff! Follow these steps for setting up dual boot using an external SSD:
