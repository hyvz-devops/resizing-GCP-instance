# resizing-GCP-instance
Increasing the Size of Bastion to 100GB
davron1989 edited this page 5 minutes ago · 1 revision
To manually resize existing GCP disk size


Resizing a disk doesn't delete or modify disk data, but as a best practice, snapshot your disk before you make any changes.

Take a snapshot of an existing disk
In GCP console Navigate to > Compute Engine > Snapshots:

Create snapshot
Resize existing disk
To see existing disks
$ gcloud clompue disks list

To check disk size
$ df -h

$ lsblk

Resize the disk
$ gcloud compute disks resize DISK_NAME --size DISK_SIZE -- zone DISK_ZONE

After you resize the disk, you must resize the file system so that the operating system can access the additional space.

On Linux instances, connect to your instance and manually resize your partitions and file systems to use the disk space that you added. You do not need to restart your instance after you complete this manual process.

If the disk that you want to resize has a partition table, you must grow the partition before you resize the file system. Use growpart to resize your image partition.

To install growpart command and grow the partition on CentOS servers
$ sudo yum -y install cloud-utils-growpart

$ sudo growpart /dev/DEVICE_ID PARTITION_NUMBER

For example, sudo growpart /dev/sda 1, notice that there is a space between the device ID and the partition number.

Extend the file system on the disk or partition to use the added space
Check what filesystem disk uses
$ sudo df -Th

If you are using xfs, use the xfs_growfs command to extend the file system, and specify the mount point.

$ sudo xfs_growfs /

/ is the mount point.

Verify that the file system is resized
$ df -h /dev/DEVICE_ID
