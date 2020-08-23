## `diskpart`

[`diskpart`](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/diskpart) is a Windows CLI program used for disk partitioning.

### Why?
Even though windows's disk management tool enables disk partitioning and formatting options, it sometimes doesn't allow all actions on some partitions. For me, create new simple volume was grayed out, and I couldn't remove partitions from my pendrive. `diskpart` solved this easily. And there was no need to install any third party tools.

### How?

```dos
c:\> diskpart

DISKPART> list disk

  Disk ###  Status         Size     Free     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  Disk 0    Online          256 GB   130 GB         
  Disk 1    Online           14 MB    14 GB         
       
DISKPART> select disk 1

Disk 1 is now the selected disk.

DISKPART> clean

DiskPart succeeded in cleaning the disk

DISKPART> create partition primary

DiskPart succeeded in creating the specified partition
```

### Reference

 * [`clean`](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/clean)
 * [`create partition primary`](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/create-partition-primary)
