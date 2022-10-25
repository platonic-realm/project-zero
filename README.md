# Project-Zero

<!---
<p align="center">
    <img src="logo_outlined.svg" width="400" alt="Project Zero logo">
</p>
-->

Project-Zero provides step-by-step instructions for building a minimal but functional Linux-based operating system from scratch.

"Let's build a minimal Linux-based operating system", does this sentence excites you? If yes, you might already have tried projects like LFS (Linux from scratch), or might already be busy compiling your custom Linux kernel for your embedded device. These projects were joyful and taught me a lot. But I still lack the ultimate insight. I want to know more about the kernel and its subsystems. Additionally, I want to use that knowledge in action. Also, to some extent, I am interested in having a minimal but functional computing environment, only using bash, busy box, and tmux. Project-Zero is my answer to myself.

We start by disabling every available option in the Linux kernel (disabling all the subsystems) and then based on our requirements, will gradually enable them. Also, I provide more explanations or resources for each of the subsystems in use. For example, in the beginning, we start with only the kernel and bash, and we don't need any filesystem or network support. The project consists of several stages. At any stage, we add some functionality that corresponds to a collection of userspace software and kernel space tweaks.

#### Chapter 0: Development Enviroment

To standardize our development environment, we create an Alpine-based docker(OCI) image. Stage 0 describes the procedure. Alternatively, you can pull the image from the docker hub. Also, we need qemu for virtualization. We compile our software using the docker container and then use qemu to run it.

#### Chapter 1: Linux + Bash

We compile the kernel with minimal enabled features. Also, we compile Bash statically and use it as the init process. The goal is to give you an insight into how kernel initiates userspace processes.

#### Chapter 2: Filesystem support

We need to add more functionality to our boring Bash interpreter. To do so, we will use BusyBox along with Tmux and nano. Therefore, we need a sort of storage to store our userspace application. So our kernel should be able to communicate with the corresponding storage hardware and also, interpret its content as files. In this chapter, we will configure the kernel to add these functionalities.

#### Chapter 3: BusyBox

Now that our kernel has access to a storage device and can read the ext2 filesystem. We can install BusyBox on an ext2 image file and use it for the init process. BusyBox provides much more functionality that we will use in the future. In this chapter, we configure and install BusyBox on our ext2 image. And configure our kernel to use it for initiating the userspace processes.

#### Chapter 4: Userspace configurations

We can use BusyBox for user management, networking, etc. To do so, we need to add some configuration files to the filesystem. This chapter is about these configuration files. The network configuration files are also added in this chapter but will be used in the next one.

#### Chapter 5: Networking supoprt

#### Chapter 6: Tmux & Nano

#### Chapter 7: Continuous Development

#### Spin off: ARM support

## Who is it for?

You don't need to be a veteran. But you need to be familiar with Bash, docker, build tools (GCC, make, etc.), and how dynamic linking works. The rest you will learn along the project. The development in stage 7 will be in Bash, and no C programming skills are needed. But, it is beneficial to have a high-level understanding of how compilation and linking work in C compilers.

## Disclaimer

The operating system built in this project is meant for educational purposes only. There is no guarantee for its security, reliability, and efficiency in real-world applications.

Also, I am by no means an expert in the field. If you encounter any mistake or if there is a better way of handling something (for example: to build an even smaller system with the same functionality), please feel free to open an issue or send a pull request.
