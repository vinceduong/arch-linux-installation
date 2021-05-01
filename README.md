# Install Arch Linux from scratch

## Minimal Install

### 1. Check connection

```ping google.com```

### 2. Partionning

Choose your disk, to check available disks:   
```lsblk```

Partition your disk:   

```fdisk /dev/[DISK NAME]```

#### Create a first partition (boot, grub menu)

Type `n` (new partition) -> `p` (partition type) -> `Enter` (Partition number) -> `Enter` (first sector) -> `+200M` (last sector)

By pressing `p` you can check that your partition has been created, you should have something like this:

<img src="https://user-images.githubusercontent.com/31627855/116777025-3fe38f80-aa6c-11eb-89b4-cecbb322b3f8.png" alt="screenshot" width="500"/>


#### Create second partition (swap)

Usually your swap size should be around 100 - 150 % of your RAM size, if you have 8G of RAM you can go
for `+12G`

Type `n` (new partition) -> `p` (partition type) -> `Enter` (Partition number) -> `Enter` (first sector) -> `+[X]G` (last sector)

Now your partition table should look like this (press `p`)

<img src="https://user-images.githubusercontent.com/31627855/116777204-943b3f00-aa6d-11eb-928c-86ac7fe51089.png" alt="screenshot" width="500"/>

#### Create third partition (programs location)

To chose the size of your partition of the program location, it should be around 5G to 30G, dependending on how much you are gonna install stuff, example: `+10G`

Type `n` (new partition) -> `p` (partition type) -> `Enter` (Partition number) -> `Enter` (first sector) -> `+[X]G` (last sector)

Now your partition table should look like this (press `p`)

<img src="https://user-images.githubusercontent.com/31627855/116777880-fcd6eb80-aa6e-11eb-8507-3404f52ad77a.png" alt="screenshot" width="500"/>

#### Create fourth partition (home location)

Since this is the partition where the rest of the file will belong, it should take all the remaining space.
You don't have to chose a size, `fdisk` will fill the partition with the remaining space.

Type `n` (new partition) -> `p` (partition type) -> `Enter` (Partition number) -> `Enter` (first sector) -> `Enter` (last sector)

Now your partition table should look like this (press `p`)

<img src="https://user-images.githubusercontent.com/31627855/116777987-c3eb4680-aa6f-11eb-8fd9-4c3b84bb2fdb.png" alt="screenshot" width="500"/>




