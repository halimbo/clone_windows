### You need:

A working linux install with partclone

Windows Installer ISO

### Clone and compress (unmount beforehand, go in root shell)

```#partclone.ntfs -c -s /dev/oldwindows -o - | gzip > windows.pcl.gz```

### Restore (target partition has to be NTFS)

```#gzip -d -c windows.pcl.gz | partclone.ntfs -r -o /dev/windows```

If the restored partition is smaller than the target partition, open up gparted and right click + "Check".

This will call ntfsresize. You could do this manually or with other programs.

### Apply necessary fixes to mount and boot the partition

```#partclone.ntfsfixboot -p /dev/windows```

```#partclone.ntfsfixboot -w /dev/windows```

```#ntfsfix /dev/windows```

### Boot up windows installer iso and hit Shift+F10 

Change numbers accordingly.

Drive l:\ will be the windows partition (might be mounted already)

```#list disk```

```#select disk 1```

```#list partition```

```#select partition 3```

```#assign letter l```

Drive k:\ will be the EFI partition (needs esp and boot flag)

```#diskpart```

```#select disk 1```

```#select partition 1```

```#assign letter k```

```#exit```

### Repair/Install bootloader on drive k: pointing to windows on drive l:

```#bcdboot l:\windows /s k: /f UEFI ```

You may need to specify your locale with "/l en_US" (change accordingly).

This process creates a bootable UEFI Entry for your motherboard and it might overwrite the boot order.
