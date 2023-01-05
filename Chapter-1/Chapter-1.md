[Back](../README.md)

# Chapter 1: Linux + "sh" shell

I assume we have our OCI image ready and can use docker or podman to run a container to start building the system.

In this chapter we disable all Linux kernel features and then enable only the bare minimum needed to run a **sh** shell after boot. We heavily rely on [BusyBox](https://busybox.net/) in this project this project. So, to save time, instead of statically building another shell and use it as **init** process, we use builtin **sh** shell inside BusyBox.

[BusyBox](https://busybox.net/) is a software suite that provides several Unix utilities in a single executable file. It is often used in embedded devices because it is small, efficient, and provides a simple interface for users to access the utilities. Some of the utilities that BusyBox provides include ls, cat, echo, grep, and sed, among others. BusyBox can be configured to include or exclude certain utilities, depending on the needs of the device it is being used on.

After running our development container, we build the minimal kernel and BusyBox executable. Then, we will create an **initrd** image using **cpio**. At the end, using qemu, we will mix everything together to run the compiled Linux kernel with the BusyBox binary as the **userspace** software in an virtualized environment.

## Introduction

In a typical Linux system, the kernel is responsible for managing the hardware resources of the system and providing an interface for user programs to access these resources. The kernel is typically loaded into memory at boot time and remains resident in memory for the duration of the system's uptime.

User programs, on the other hand, are normal programs that run on the system and perform various tasks such as editing text files, browsing the web, or playing games. These programs are executed in user space, which is a portion of the system's memory that is dedicated to running user programs.

The separation of user space and kernel space is implemented through the use of virtual memory, which allows the operating system to map different portions of the system's physical memory to different logical addresses. The kernel is typically mapped to a high range of logical addresses, while user space is mapped to a lower range of addresses.

This separation is important because it helps to protect the kernel from interference or corruption by user programs. If a user program were able to write to the kernel's memory space, it could potentially cause the kernel to crash or behave unexpectedly, leading to system instability or even a complete system failure.

To prevent this from occurring, the kernel is carefully designed to prevent user programs from directly accessing its memory space. Instead, user programs must use system calls and other mechanisms to request access to kernel resources or services. The kernel can then decide whether to grant the request, based on factors such as the privileges of the requesting program and the current state of the system.

The separation of user space and kernel space also allows the kernel to control and prioritize access to system resources, ensuring that important processes and tasks are given priority over less important ones. This helps to ensure that the system remains responsive and stable, even when running multiple programs concurrently.

In summary, the concept of user space and kernel space in the Linux operating system is a key feature that helps to ensure the stability, security, and efficient operation of the system by protecting the kernel and its resources from interference or corruption by user programs.

## Step 1: Running the container

First, create a directory named **"host"**. We map this directory into the container filesystem to act as shared storage between the host and the container.

```
mkdir host && cd host
```

If you followed the previous instructions in [Chapter 0](../Chapter-0/Chapter-0.md), you have a OCI image called **"project-zero"** with **"x86-x86"** tag. Below command will run the container.

```
docker run --user $(id -u):$(id -g) --platform=i386 --rm -it -v $(pwd):/host project-zero:x86-x86
```
Lets break down the meaning of each options:
* **--user**: Instructs docker to use specific user and group ids instead of root. This makes life easier later when we want to access the files created in the container from the host. The command **"id"** is used to extract current user/group ids, so it should be availabe on the host.
* **--platform:** Most probably, you are using a CPU with [AMD64](https://en.wikipedia.org/wiki/X86-64) [ISA](https://en.wikipedia.org/wiki/Instruction_set_architecture), bust we are building a [i386](https://en.wikipedia.org/wiki/I386) operating system in an i386 container. AMD64 processesor are backward compatible and can support i386 code provided that the OS and the rest of software stack supports it. Here we are telling docker that we know what we are doing, so it stops sending us warnings about ISA mismatch. 
* **--rm:** Instructs docker to remove the container after exit
* **--it:** Instructs docker that we need an interactive shell connected to a terminal
* **-v:** Instrcuts docker to map a path on the host specified before ":" into a path(inside the container) that comes after it. So here, we are mapping the **"host"** directory that we created into a the path **"/host/"** in the container.

After running the command, you should have something similar to the below image:

![container started](img/container_started.png)

## Step 2: Downloading the kernel and BusyBox source codes
 
Linux kernel's and BusyBox's configurations remain consistent most of the time. But, new features are introduced in new releases and sometimes lead to a minor rearrangement of configurations. Therefore, I suggest using the same version mentioned next in your first build. We will [Linux 5.15.86](https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.15.86.tar.xz) and [BusyBox 1.35.0](https://www.busybox.net/downloads/busybox-1.35.0.tar.bz2).

First, we create a directory **"res"** to download the source code tar files.
```
mkdir res && cd res
```

Then download the Linux source tar file with command:
```
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.15.86.tar.xz
```

And do the same for BusyBox:
```
wget https://www.busybox.net/downloads/busybox-1.35.0.tar.bz2
```

Now we use ["tar"](https://www.gnu.org/software/tar/) command to extract the content of the tar file and have access to the acutal source codes.

We want to extract the source code in the parent directory, so:
```
cd ..
```

Then for Linux kernel:
```
tar xvf res/linux-5.15.86.tar.xz
```

And for BusyBox:
```
tar xvf res/busybox-1.35.0.tar.bz2
```

Regarding the tar command:
* The **"x"** option stands for "extract", and tells tar to extract the contents of an archive. 
* The **"v"** option stands for "verbose", and tells tar to print the names of the files it is extracting to the terminal as it is extracting them.
* The **"f"** option stands for "file", and specifies the name of the archive file that tar should operate on.

At the end, the host directory content should be like this:

![host directory content](img/host_directory_source_codes.png)

## Step 3: Configuring and bulding the Linux kernel

We will build the kernel in a separate directory named **"linux-build"**, so we first need to create it.
```
mkdir linux-build
```

Then we go into our kernel's source tree.
```
cd linux-5.15.86/
```

Linux kernel has tons of configuration, some of them depend on each other and sometimes their availabilty also depends on other options. To make life easier for ourself, we create a config file with all features disabled.

```
make O=../linux-build/ allnoconfig
```
* **O=** Specify the output directory for the built kernel and associated files.

* [**allnoconfig**](https://www.kernel.org/doc/makehelp.txt) sets all configuration options to their default value of "no", resulting in a kernel with no support for any features or hardware devices.

At this point, I give you two options. The easy option would be to copy a ready to use **.config** file into your build directory and compile the kernel. The fun option would be to configure the kernel yourself based on detailed step-by-step instructions and explanations. These two options is mutually excusive, choose one.

### **Option 1:** To copy the configurations

Go to the build directory:
```
cd ../linux-build/
```

Download the already configured configuration file:
```
wget -c https://raw.githubusercontent.com/platonic-realm/project-zero/main/Chapter-1/kernel/.config
```

Alpine linux(which our container is based upon it) also uses BusyBox and the version of wget included with BusyBox does not support the -N option for timestamping. However, you can use the -c option to allow wget to resume a download that was previously interrupted, which will effectively overwrite the local file.

### **Option 2:** To configure the kernel your self

## Step 3: Configuring and bulding BusyBox

## Step 4: Create a initrd image

## Step 5: Run our minimal system using qemu