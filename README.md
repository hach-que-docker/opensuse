# Preparing a Docker image with OpenSUSE

## Create a hard disk image of OpenSUSE in SuSE Studio

Here's one that's already been prepared: https://susestudio.com/a/Sn0thq/docker-base-image

You'll want to download "USB drive / Hard Disk Image" which we'll use later.

## Extract the image

```
> tar -xvf MyOpenSUSEImage.oem.tar.gz
```

## Inspect the hard disk image

We need to inspect the hard disk image to find out where the partition is, so we can mount it.

Use parted for this:

```
> parted MyOpenSUSEImage.raw
GNU Parted 3.1
Using .....
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) unit
Unit?  [compact]? B
(parted) print
Model:  (file)
Disk ......: 305135616B
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:

Number  Start     End         Size        Type     File system  Flags
 1      1048576B  305135615B  304087040B  primary  ext3         boot, type=83

(parted) q
```

## Mount the image

```
> mkdir target
> mount -o loop,ro,offset=1048576 MyOpenSUSEImage.raw target
```

## GZip the image

```
> cd target
> tar -cpf ../opensuse.tar *
> cd ..
```

## Import into Docker

```
> docker import opensuse.tar my-repo/opensuse
```

## Test your new image

```
> docker run -t -i my-repo/opensuse /bin/bash
```

## Push your image

```
> docker push my-repo/opensuse
```