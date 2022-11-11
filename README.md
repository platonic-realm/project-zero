# Project-Zero

<!---
<p align="center">
    <img src="logo_outlined.svg" width="400" alt="Project Zero logo">
</p>
-->

Project-Zero provides step-by-step instructions for building a minimal but functional Linux-based operating system from scratch.

"Let's build a minimal Linux-based operating system." does this sentence excites you? If yes, you might already have tried projects like LFS (Linux from scratch) or might be busy compiling your custom Linux kernel for your embedded device. These projects were joyful, but I still lack the ultimate insight and I want to know more about the Linux kernel and its subsystems. Additionally, I am interested in having a minimal but functional computing environment, using only bash, busy box, tmux, and an editor. Project-Zero is my answer to myself.

We start by disabling every available option in the Linux kernel (disabling all the subsystems) and then based on our requirements, will gradually enable them. Also, I provide more explanations or resources for each of the subsystems in use. For example, in the beginning, we start with only the kernel and bash, and we don't need any file-system or network support. The project consists of several chapters. At any chapter, we add some functionality that corresponds to a collection of user space software and kernel space tweaks.

-------
<h3><a href="Chapter 0/Chapter-0.md">Chapter 0: Development Environment</a></h3>


To standardize our development environment, we create an Alpine-based [OCI](https://github.com/opencontainers/image-spec/blob/main/spec.md) image. Chapter 0 describes the procedure. Alternatively, you can pull the image from the docker hub. Also, we need qemu for virtualization. We compile our software using the docker container and then use qemu to run it.

-------
<h3><a href="Chapter-1.md">Chapter 1: Linux + Bash</a></h3>


We compile the kernel with minimal enabled features. Also, we compile Bash statically and use it as the init process. The goal is to give you an insight into how kernel initiates userspace processes.

-------
<h3><a href="Chapter-2.md">Chapter 2: Filesystem support</a></h3>

We need to add more functionality to our boring Bash interpreter. To do so, we will use BusyBox along with tmux and nano. Therefore, we need a sort of storage to store our userspace application. So our kernel should be able to communicate with the corresponding storage hardware and also, interpret its content as files. In this chapter, we will configure the kernel to add these functionalities.

-------
<h3><a href="Chapter-3.md">Chapter 3: BusyBox</a></h3>

Now that our kernel has access to a storage device and can read the ext2 filesystem. We can install BusyBox on an ext2 image file and use it for the init process. BusyBox provides much more functionality that we will use in the future. In this chapter, we configure and install BusyBox on our ext2 image. And configure our kernel to use it for initiating the userspace processes.

-------
<h3><a href="Chapter-4.md">Chapter 4: Userspace configurations</a></h3>

We can use BusyBox for user management, networking, etc. To do so, we need to add some configuration files to the filesystem. This chapter is about these configuration files. The network configuration files are also added in this chapter but will be used in the next one.

-------
<h3><a href="Chapter-5.md">Chapter 5: Networking support</a></h3>

-------
<h3><a href="Chapter-6.md">Chapter 6: Tmux & Nano</a></h3>

-------
<h3><a href="Chapter-7.md">Chapter 7: Continuous Development</a></h3>

-------
<h3><a href="Spin-off.md">Spin off: ARM support</a></h3>


-------
<h3>Who is it for?</h2>

You don't need to be a veteran. But you need to be familiar with Bash, docker, build tools (GCC, make, etc.), and how dynamic linking works. The rest you will learn along the project. The development in chapter 7 will be in Bash, and no C programming skills are needed. But, it is beneficial to have a high-level understanding of how compilation and linking work in C compilers.


-------
<h3>Disclaimer</h2>

The operating system built in this project is meant for educational purposes only. There is no guarantee for its security, reliability, and efficiency in real-world applications.

Also, I am by no means an expert in the field. If you encounter any mistake or if there is a better way of handling something (for example: to build an even smaller system with the same functionality), please feel free to open an issue or send a pull request.