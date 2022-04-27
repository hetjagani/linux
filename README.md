# CMPE 283 Assignment 2,3 Linux Kernel

## Assignment 2

Following are the commits for changes regarding Assignment 2
* https://github.com/hetjagani/linux/commit/cb602315451a575386fa6490088ba66d7a548959
* https://github.com/hetjagani/linux/commit/03e8c3ae172fc255821aadc8a893147cff471f0e

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
![Screenshot from 2022-04-21 10-08-16](https://user-images.githubusercontent.com/27214644/164514529-952d79f0-a08b-462c-90a1-76a4c996e969.png)

8) Issued CPUID command with 0x4FFFFFFF as parameter and compared the values with printed values by our new KVM module. (Screenshots below)
![Screenshot from 2022-04-21 10-09-30](https://user-images.githubusercontent.com/27214644/164514556-2b66ef1f-3741-41bd-81ae-cbcb90a22ea1.png)

9) Issued CPUID command with 0x4FFFFFFE as parameter and compared the values with printed values by our new KVM module. (Screenshots below)
![Screenshot from 2022-04-21 10-10-02](https://user-images.githubusercontent.com/27214644/164514598-80cc2a0c-e975-44cf-97e3-2653952782d1.png)


## Assignment 3

Following are the commits for changes regarding Assignment 3
* https://github.com/hetjagani/linux/commit/d38b5ec1a3d146cfd955ad34b954c0f87e95e2da

> Ubuntu OS installed on Intel machine is used while doing this assignment

In this assignment changes are done in KVM to modify behaviour of `cpuid` instruction when `%eax=0x4FFFFFFD` and `%eax=0x4FFFFFFC`

For CPUID leaf node `%eax=0x4FFFFFFD`:
* Return the number of exits for the exit number provided (on input) in %ecx
    - This value should be returned in %eax   
    - 
For CPUID leaf node `%eax=0x4FFFFFFC`:   
* Return the time spent processing the exit number provided (on input) in %ecx
    - Return the high 32 bits of the total time spent for that exit in %ebx
    - Return the low 32 bits of the total time spent for that exit in %ecx   


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
7) Created CentOS 7 server OS VM on KVM and ran the VM (Screenshot below)
![ss1](https://user-images.githubusercontent.com/27214644/165578949-f64403ad-8e69-4e18-9ebc-c45c8a225b55.png)


8) Created script which takes leaf node param (i.e. value for `eax`) and calls `cpuid` for all exit types 1 to 75.
```bash
#!/bin/bash

for i in {0..75}
do
    echo "EXIT $i"
    cpuid -l $1 -s $i
done
```

9) Ran script with `0x4ffffffd` as parameter and compared the values with printed values by our new KVM module. (Screenshots below)
![ss2](https://user-images.githubusercontent.com/27214644/165579069-9c6ce41d-bc39-4c6b-b6a4-1523c0019c1f.png)


10) Ran script with `0x4ffffffc` as parameter and compared the values with printed values by our new KVM module. (Screenshots below)
![ss3](https://user-images.githubusercontent.com/27214644/165579109-5396ae26-f3e9-4241-bb43-1931d27fcfec.png)

#### Q3: Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are theremore exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
* Frequency of some exits such as HLT (# 12), CPUID (# 10) etc remains quiet stable because those exits are called fairly regularly and so number of exits increase at a stable rate. This can be observed in screenshot below.
![ss4](https://user-images.githubusercontent.com/27214644/165579156-c01f4080-e888-47cf-852c-3e1650354af5.png)


#### Q4: Of the exit types defined in the SDM, which are the most frequent? Least?
Following are some of the most frequently called exits:
* Exit #28 - CR_ACCESS
* Exit #48 - EPT_VIOLATION

Following are some of the least frequently called exits:
* Exit #55 - XSETBV
* Exit #47 - LDTR_TR
* Exit #29 - DR_ACCESS
