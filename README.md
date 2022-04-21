# CMPE 283 Assignment 2,3 Linux Kernel

## Assignment 2

Following are the commits ids for changes regarding Assignment 2
* cb6023154
* 03e8c3ae1

> Fedora OS installed on Intel machine is used while doing this assignment

In this assignment changes are done in KVM to modify behaviour of `cpuid` instruction when `%eax=0x4FFFFFFF` and `%eax=0x4FFFFFFE`

For CPUID leaf node `%eax=0x4FFFFFFF`:
* Return the total number of exits (all types) in %eax
For CPUID leaf node `%eax=0x4FFFFFFE`:
* Return the high 32 bits of the total time spent processing all exits in %ebx
* Return the low 32 bits of the total time spent processing all exits in %ecx
    - %ebx and %ecx return values are measured in processor cycles, across all VCPUs

#### Q1: Team member details
* Done by myself

#### Q2: The steps used to complete the assignment   

1) Cloned the kernel code from forked repository in my personal github account (i.e. https://github.com/hetjagani/linux)   
2) Build the kernel using following steps:  
    - `make oldconfig`  
    - `make modules`  
    - `make install`  
    - `sudo make modules_install`  
    - `sudo make install`  
3) Reboot the OS and boot with newly built kernel to modify its code and test new functionalities.
4) Made code changes in local clone of linux repository, then build all modules and installed modules
    - `make modules && sudo make modules_install`
5) To load newly built kvm module removed old modules and loaded newly installed modules
    - `sudo rmmod kvm_intel`  
    - `sudo rmmod kvm`  
    - `sudo modprobe kvm`  
    - `sudo modprobe kvm_intel`  
6) To test the functionality KVM Virtual Machine Manager was installed to run VM on KVM using qemu.
7) Created Ubuntu 20.04 server OS VM on KVM and ran the VM (Screenshot below)

8) Issued CPUID command with 0x4FFFFFFF as parameter and compared the values with printed values by our new KVM module. (Screenshots below)


9) Issued CPUID command with 0x4FFFFFFE as parameter and compared the values with printed values by our new KVM module. (Screenshots below)