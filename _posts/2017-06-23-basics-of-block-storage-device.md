---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: 'Basics of block storage devices' 
date:   2017-06-23 06:18:00
tags: Block Storage HDD
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/logo/logo1.png'
---
<img src="/assets/images/2017/2017_06_hard_disk.png"  alt="Hard disk" class="leftimg" /> Today's mass storage devices include magnetic hard disk, solid state drive, and often flash drive. Though technologically quite different, all these storage classes have one thing in common: they are accessed in a bigger chunk of data unit, not by a byte. This unit of access is called block (sometimes also called page). The internal technology, organizational structure etc could be whatever but the storage device's adapter IDE/SATA/PATA/SCSI exposes the storage in a uniform manner to the operating system.  
 
<!--more-->

##Magnetic Hard Disks
A magnetic hard disk, often simply hard disk drive, consists of  a set of metal circular platter put together in a spindle. A 2.5-inch drive now can have a maximum of 5 platters (max capacity 5TB), while a 3.5-inch drive can have 5 to 8 platers (max 12TB).  Read/Write heads are placed just a thin air away of the surfaces on each platter. Heads are numbered. Each surface of a platter is hypothetically considered to have circular tracks, each track split into sectors. Tracks are numbered too, begining at 0 from outside, increasing towards center. Physical size of sectors are bigger around the outer side of the platter, and gets smaller towards the centre. However, to keep things simple each sector is capable of storing equal amount of data, most commonly 512 bytes. (A newer standard called Advanced Format is calling for 4096 bytes). 

Same numbered tracks on all the platters are imagined to form a cylinder. Therefore, to identify a particular track, one needs a cylinder number and head number. Once in the desired track, a sector is the smallest addressable unit identified by another number.  So to identify a sector uniquely in an entire disk, one needs the cylinder, head and sector (CHS) number.

Older PCs would access storage (both floppy and hard) using these CHS addressing. The PC BIOS interrupt 13hex allowed/served that. However, with the wider adoption of hard disks, and increasing storage capacity, CHS was found to be unncessarily complex and burdensome for the operating system (since it is the OS that manages the storage). Then came LBA.

###Logical Block Addressing (LBA)
In LBA philosopy the entire stoarage is consindered as a linear array of blocks. Each blck is identified by a single block number. Once an address (block number) is given, the hard disk adapter maps that to physical C/H/S. This means LBA is independent of the internal organization of disk, and simply referes them as blocks (which for hard disks are sectors). 

LBA address can be 32-bit, 48-bit or even 64-bit, and the addressing is simply linear: starts at zero and grows upward. Which address is mapped to which sector is up to the drive adapter. However, most drives follow a simple forumla. More can be found in <a href="https://en.wikipedia.org/wiki/Logical_block_addressing"> Wikipedia</a>.
 

###Disk Partition
A physical disk drive can be logically divided into more than one unit and accessed as an independent storage, even by different operating system. There are mainly two schemes on PCs to do that: the legacy MBR (master boot record), and newer GPT (GUID Partition Table). Apple has its own scheme.

####Master Boot Record (MBR)
Ignoring the wording "boot record", lets pretend "MBR" is a scheme to have multiple partitions in a single physical disk drive. The information about the partions (and more) are stored in the first (LBA=0) Block (also a sector for hard disk) of a drive. An "MBR" partitioning scheme can have a maximum of 4 partitions (all 4 primary or 3 primary and 1 extended which in turn can have more `logical partitions` within it).  The first primary partion, called `system partition` stores opeating system boot files. A layout of a disk with "MBR" scheme is shown below: <img src="/assets/images/2017/2017_06_23_ms_mbr.gif" alt="MBR Scheme" title="MBR scheme" />
The first 440 bytes in the MBR that reads Master Boot Code in the picture is actually executable code. When a PC powers up, the BIOS, as part of the booting proceess, reads those codes and execute them.
 
Structure of different versions of "MBR" can be found in <a href="https://en.wikipedia.org/wiki/Master_boot_record"> Wikipedia MBR</a> enry.

####Boot Sector
More than one primary partions can have a bootable operating system. The first sector on a partion is boot sector (if it has boot record). Similar to master boot code, boot sector can have executable code that the BIOS or a boot loader loads and executes. The boot sector is only valid for a partion, that is marked as `bootable` with a special signature.


####GUID Partition Table (GPT)
Among the few limitations of "MBR" scheme, the partition table entries indicate the begining/ending of a partition as 32-bit LBA. This imposes a limitation of maximum 2TB disk capacity. GPT removes that restrictions, uses 64 bits for LBA, and supports upto 128 partitions. Though originally developed as part of EFI, GPT scheme however leaves the legacy LBA=0 `Protective MBR` untouched. An example of a GPT scheme is shown below: <img src="/assets/images/2017/2017_06_23_gpt.png" />


####Credits
- The hard disk picture is taken from <a href="http://ccm.net/contents/385-hard-drive"> CCM </a>
- The "MBR" picture has been borrowed from  <a href="https://gerardnico.com/wiki/data_storage/partition"> here.</a>
- MBR structure is taken from  <a href="https://upload.wikimedia.org/wikipedia/commons/0/07/GUID_Partition_Table_Scheme.svg">wikipedia</a>
