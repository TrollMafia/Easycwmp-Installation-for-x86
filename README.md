# Easycwmp-Installation
This repository has all the documentation on how to install easycwmp on OpenWRT. It is being used on a Virtual Machine

### Step 1: Install Ubuntu or Debian OS in VM

Install a fresh Ubuntu or Debian operating system in a virtual machine (you can refer this site https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox).

### Step 2: Update and Install Dependencies

Run the following commands in the terminal to update the package list and install necessary dependencies:

```bash
sudo apt update
sudo apt install build-essential clang flex bison g++ gawk \
gcc-multilib g++-multilib gettext git libncurses-dev libssl-dev \
python3-distutils rsync unzip zlib1g-dev file wget
```

### Step 3: Clone OpenWrt Repository

Now after the installation of required dependencies you have to clone the OpenWrt repository using the following commands:

```bash
git clone https://git.openwrt.org/openwrt/openwrt.git
cd openwrt
```

### Step 4: Download easycwmp File

Download the easycwmp file. You can obtain it from the official source i.e. *https://easycwmp.org/get/* and select the openwrt package.

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/771ca423-f66d-4fa6-b57f-aa5723cc175f)

 
### Step 5: Extract and Copy easycwmp

Extract the easycwmp file and copy it to the `/openwrt/package` path. Since we have to build the easycwmp as a package we have to place it in the package folder

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/eb884920-1c9d-489f-8f62-a4808028feae)


 ### Step 6: Update Feeds and Install Packages

Run the following commands to update feeds and install packages:

```bash
./scripts/feeds update -a
./scripts/feeds install -a 
```

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/d1d980f9-3079-463b-8cb6-f1c3aca538b6)

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/77c44170-90cf-47fc-9346-812f1953986d)

 
 ### Step 7: Handle libpam Error

If there is an error related to libpam, run the following command:

```bash
./scripts/feeds install libpam liblzma
```

 ![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/518bb696-b01a-462f-9863-345cf99ac7dc)


### Step 8: Handle libmicroxml Error

If there is an error related to libmicroxml, run the following commands:

```bash
cd package
wget https://easycwmp.org/download/libmicroxml.tar.gz
tar -xzvf libmicroxml.tar.gz
```
![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/3dfebb05-4781-434d-a0d7-29defdf0a79d)

 
### Step 9: Run `make menuconfig`

In the `openwrt` directory, run the following command:

```bash
make menuconfig
```

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/4e1a3005-ae06-4fe7-9924-5e0be55d7d52)

 ### Step 10: Select easycwmp in menuconfig

Navigate to the Utilities section in `menuconfig`, select easycwmp by pressing 'Y'.

 ![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/c0c8d198-e475-4ce4-aaae-4eac7f280fc1)

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/ede8d29a-a59b-4b2f-90fb-9c9f5680f788)

 ![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/63e6926f-5e66-462f-a55c-410761961310)

Now just press on exit two times and click on 'yes' when prompted and the configuration will be saved.

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/ec89fb07-8d6a-4dcc-a0c1-affedde515d3)

 
### Step 11: Compile easycwmp

Run the following command to compile easycwmp:

```bash
make package/easycwmp/compile V=sc
```

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/a4d44feb-cbb8-45a6-9232-c0f51fa2af73)

V=sc is used for higher verbosity level to check what's going on with the building process.

**If you are getting these errors try the below steps.**
 
### Step 12:

This error is showing due to we have not build the required dependencies i.e. **tools** and **toolchain**.
These are required to make a package , we can do this by running these two commands :
```bash
make tools/install
make toolchain/install
```
 ![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/dfdeabd0-6553-42be-ab50-f35a1487bc8f)

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/4aa8abf8-0ea1-4468-9220-a2fbc84bfe7a)

 
This will build our toolchain

### Step 13:

Now we just need to build one package that we downloaded i.e. **libmicroxml** by using this command :

```bash
make package/libmicroxml/compile
```

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/93387026-b3c9-4147-9fb3-2a8ca881e061)

 ### Step 14:

If there is some kind of Hash error it can solved by running these two commands :

```bash
make package/easycwmp/download PKG_HASH=skip V=s
make package/easycwmp/check FIXUP=1 V=s
```

### Step 15:

Now just build the easycwmp package:
```bash
make package/easycwmp/compile
```

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/3f9c58cb-8cee-452e-b9e6-590e31d4e4ac)

### Step 16:

There are some required packages you need to install in order to install Easycwmp. 
I have included the main packages in the repository, if there are some packages missing like `curl` just install it with `opkg`.

 
You will get your `.ipk` file in the `bin/packages/your_selected_target/base/easycwmp.ipk`

![image](https://github.com/TrollMafia/Easycwmp-Installation/assets/116051989/72f1c562-9436-4a53-8a9f-39c41e5d2f5b)


***PS : I am using x86 architecture since I am using Virtual Machine for OpenWRT. If you are building it for your router then head to Targets in menuconfig and select your device. 
I have also included the pre-build easycwmp package for x86 and also its image , you can use it to run easycwmp.***
