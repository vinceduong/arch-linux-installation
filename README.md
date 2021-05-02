# <img src="https://user-images.githubusercontent.com/31627855/116778146-833ffd00-aa70-11eb-802e-4051a432f479.png" width="28"/> Arch Linux Install from scratch



## Minimal Install

### Check connection

```ping google.com```

### Partitionning

Choose your disk, to check available disks
> ```lsblk```

Enter in the disk partition menu ```fdisk /dev/[DISK NAME]```
> Partionning will erase your disk, see "5. Save your partition table" for more info.  
> Example: ```fdisk /dev/sda```

In order to have a stable installation, we need to create different partitions for safety reasons.
This will make sure that if one part of your installation breaks, it wont affects other partitions

#### 1. Create a first partition (boot, grub menu)

Type `n` (new partition) -> `p` (partition type) -> `Enter` (Partition number) -> `Enter` (first sector) -> `+200M` (last sector)

By pressing `p` you can check that your partition has been created, you should have something like this:

<img src="https://user-images.githubusercontent.com/31627855/116777025-3fe38f80-aa6c-11eb-89b4-cecbb322b3f8.png" alt="screenshot" width="500"/>


#### 2. Create second partition (swap)

Usually your swap size should be around 100 - 150 % of your RAM size, if you have 8G of RAM you can go
for `+12G`

Type `n` (new partition) -> `p` (partition type) -> `Enter` (Partition number) -> `Enter` (first sector) -> `+[X]G` (last sector)

Now your partition table should look like this (press `p`)

<img src="https://user-images.githubusercontent.com/31627855/116777204-943b3f00-aa6d-11eb-928c-86ac7fe51089.png" alt="screenshot" width="500"/>

#### 3. Create third partition (programs location)

To chose the size of your partition of the program location, it should be around 5G to 30G, dependending on how much you are gonna install stuff, example: `+10G`

Type `n` (new partition) -> `p` (partition type) -> `Enter` (Partition number) -> `Enter` (first sector) -> `+[X]G` (last sector)

Now your partition table should look like this (press `p`)

<img src="https://user-images.githubusercontent.com/31627855/116777880-fcd6eb80-aa6e-11eb-8507-3404f52ad77a.png" alt="screenshot" width="500"/>

#### 4. Create fourth partition (home location)

Since this is the partition where the rest of the file will belong, it should take all the remaining space.
You don't have to chose a size, `fdisk` will fill the partition with the remaining space.

Type `n` (new partition) -> `p` (partition type) -> `Enter` (Partition number) -> `Enter` (first sector) -> `Enter` (last sector)

Now your partition table should look like this (press `p`)

<img src="https://user-images.githubusercontent.com/31627855/116777987-c3eb4680-aa6f-11eb-8fd9-4c3b84bb2fdb.png" alt="screenshot" width="500"/>


#### 5. Save your partition table

⚠️ Warning ⚠️

<ins>Before doing this step make sure you have a backup of your disk data, this step will erase your disk</ins>

Now if you type again ```lsblk``` you should have a similar output

<img src="https://user-images.githubusercontent.com/31627855/116779269-a15c2c00-aa75-11eb-9d38-2ba029febc7c.png" alt="screenshot" width="500"/>

### Make filesystems

We need to create filesystems for the boot, programs and home location, so in our case `sda1`, `sda3`, `sda4`

You can type theses commands
```
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/sda3
mkfs.ext4 /dev/sda4
```

<img src="https://user-images.githubusercontent.com/31627855/116779175-f0ee2800-aa74-11eb-8fcd-74fb69ee7855.png" alt="screenshot" width="500"/>

For the swap partition run
```mkswap /dev/sda2```

<img src="https://user-images.githubusercontent.com/31627855/116779243-6a861600-aa75-11eb-9695-d3fece23792e.png" alt="screenshot" width="500"/>

To complete swap setup, run
```swapon /dev/sda2```

### Mount filesystems to linux

In order to make our filesystems usable for arch linux, we need to mount them into `MOUNTPOINTS`

First we want to mount the root directory into `/mnt`

```mount /dev/sda3 /mnt```

Create the home directory for the home partition

```mkdir /mnt/home```

Create the boot directory

```mkdir /mnt/boot```

If you run ```ls /mnt/``` you should see this

<img src="https://user-images.githubusercontent.com/31627855/116782532-f3a74800-aa89-11eb-9e5a-48523016f20f.png" alt="screenshot" width="300"/>

Mount the boot partition to the boot directory

```mount /dev/sda1 /mnt/boot```

Mount the home partition to the home directory

```mount /dev/sda4 /mnt/home```

By running ```lsblk``` you should see the partitions mounted on the appropriate `MOUNTPOINTS`

<img src="https://user-images.githubusercontent.com/31627855/116782607-74feda80-aa8a-11eb-9ee4-a571cdc6ce19.png" alt="screenshot" width="500"/>

### Install Arch Linux dependencies

Install arch linux on `/mnt` with pacstrap, pacstrap is the package manager for the arch linux install process. We are going to install `base` `base-devel` and `vim` as an editor.

Run ```pacstrap /mnt base base-devel linux linux-firmware vim```. This will propably take a couple of minutes.

### Create an fstab file

Until now, we manually mounted our partitions, but we need to tell linux how to mount automatically the partitions on boot. An `fstab` file describes the necessary mounts.

Run ```genfstab -U /mnt >> /mnt/etc/fstab```

By running ```cat /mnt/etc/fstab``` you should see

<img src="https://user-images.githubusercontent.com/31627855/116783076-0d965a00-aa8d-11eb-8792-d5c1b4ec1de8.png" alt="screenshot" width="500"/>

### Jump into our arch linux installation

Run ```arch-chroot /mnt```, now you are in the arch installation, by typing ```ls``` you should see

<img src="https://user-images.githubusercontent.com/31627855/116783636-12104200-aa90-11eb-8bf2-07e3e89d107c.png" alt="screenshot" width="500"/>

### Install a network manager

By default, arch linux doesn't come with a network manager, so you wont be granted with network access.
To have internet access by booting into arch

Run ```pacman -S networkmanager```

To automatically start networmanager run ```systemctl enable NetworkManager```

### Install GRUB boot loader

GRUB will detect automatically your partitions and show a little menu to load into your system

Run ```pacman -S grub```

Install grub on your drive

Run ```grub-install --target=i386-pc /dev/sda```

<img src="https://user-images.githubusercontent.com/31627855/116783928-c9598880-aa91-11eb-8f1a-bb5594e89a1c.png" alt="screenshot" width="500"/>

Generate a configuration file

Run ```grub-mkconfig -o /boot/grub/grub.cfg```

### Create a password

Run ```passwd``` and set a password

### Generate a locale

Run ```vim /etc/locale.gen``` and uncomment your locale, in my case it is `EN_US`

<img src="https://user-images.githubusercontent.com/31627855/116784119-bf845500-aa92-11eb-862f-6bf5064f73f9.png" alt="screenshot" width="500"/>

Save the file and exit

Run ```locale-gen```

Set your language by creating a new file at `etc/locale.conf`

Run ```vim /etc/locale.conf```

Write the following into your file ```LANG=[YOUR_LANG].UTF-8```

<img src="https://user-images.githubusercontent.com/31627855/116806671-f0b26300-ab2e-11eb-91b7-e1045e5547d6.png" alt="screenshot" width="500"/>

Save the file and exit

### Configure system name

Run ```vim /etc/hostname```

And insert your system name

<img src="https://user-images.githubusercontent.com/31627855/116784562-34588e80-aa95-11eb-91c3-8b143e69a5b7.png" alt="screenshot" width="500"/>

Save the file and exit

### Reboot

Run `exit` to exit the arch installation

For safety reasons run ```umount -R /mnt```

<img src="https://user-images.githubusercontent.com/31627855/116784658-afba4000-aa95-11eb-9228-c5bc603ac6af.png" alt="screenshot" width="500"/>

You can now run ```reboot``` and remove your USB stick when your computer is off

Now you have a minimal installation of linux and you can start to play around

Your login will be by default `root` and your password is the one you set











