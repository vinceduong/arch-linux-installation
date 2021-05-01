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

<img src="https://user-images.githubusercontent.com/31627855/116778736-0f075880-aa74-11eb-8392-108a3970652d.png" alt="screenshot" width="500"/>





