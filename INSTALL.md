# Installing Raspberry Pi images from terminal on Mac

## Download the correct version of rasbian lite

```
    wget
http://debian.rutgers.edu/raspbian_images/raspberrypi/images/raspbian/2019-09-26-raspbian-buster/2019-09-26-raspbian-buster-lite.zip
```

## Insert the SD card and identify the matching disk

*NOTE* THIS DISK MAY DIFFER FROM THE NOTES BELOW

```
$ diskutil list
/dev/disk0 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         500.3 GB   disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:                 Apple_APFS Container disk1         500.0 GB   disk0s2

/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +500.0 GB   disk1
                                 Physical Store disk0s2
   1:                APFS Volume Macintosh HD            159.2 GB   disk1s1
   2:                APFS Volume Preboot                 48.1 MB    disk1s2
   3:                APFS Volume Recovery                510.4 MB   disk1s3
   4:                APFS Volume VM                      2.1 GB     disk1s4

/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *63.9 GB    disk2
   1:             Windows_FAT_32 boot                    268.4 MB   disk2s1
   2:                      Linux                         63.6 GB    disk2s2
```

## Unmount the disk

```
diskutil unmountDisk /dev/disk2
```

## Unzip and write the image to the disk

```
unzip 2019-09-26-raspbian-buster-lite.zip
sudo dd bs=1m if=2019-09-26-raspbian-buster-lite.img of=/dev/disk2 conv=sync
```

## Remount the disk

```
diskutil mountDisk /dev/disk2
```

## Enable ssh access by touching a file on the mounted disk

```
touch /Volumes/boot/ssh
```

## Configure WIFI access if required

```
cat << EOF > /Volumes/boot/wpa_supplicant.conf
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=<Insert country code here>

network={
 ssid="<Name of your WiFi>"
 psk="<Password for your WiFi>"
}
EOF
```

## Eject the disk and insert it into Raspberry pi

```
diskutil eject /dev/disk2
```


