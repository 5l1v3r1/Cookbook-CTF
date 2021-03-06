# CTF-Cookbook

## Description
This cookbook is used setup and prepare the instance environment for CTF events and Wargame challenges. For more insight into why I changed to a Vagrant-based infrastructure, check out my blog post: http://infamoussyn.com/2015/04/04/my-brand-new-ctf-environment/

## Requirements
* Ubuntu 14.04 x86-64
* git
* ruby 1.9.3p484
* Vagrant 1.4.3
* Virtualbox
* apt cookbook 2.7.0 or greater - https://github.com/opscode-cookbooks/apt

## Attributes
```
## General cookbook attributes
default['general']['owner']								= 'vagrant'
default['general']['group']								= 'vagrant'
default['general']['mode']								= 0711
default['general']['dir']								= '/home/vagrant'

## Attributes for libraries recipes
default['libraries']['multiarch']						= true
default['libraries']['arch']							= 'sudo dpkg --add-architecture i386'
default['libraries']['legacy_method']					= 'sudo echo foreign-architecture i386 > /etc/dpkg/dpkg.cfg.d/mutliarch'
default['libraries']['i386']							= ['libc6:i386','libncurses5:i386','libstdc++6:i386','libglib2.0-0:i386','libfreetype6:i386','libsm6:i386','libXrender1:i386','libfontconfig1:i386','libxext6:i386']
default['java']['enabled']								= true
#default['java']['package']								= 'default-jre'
default['java']['package']								= 'openjdk-7-jre'
default['applications']['packages']						= ['gcc-multilib','gdb','unzip','git']

## Attributes for applications
default['files']['checksec']							= 'checksec.sh'
default['files']['shellcode_examiner']					= 'shellcode_examiner.c'
default['ida']['enabled']								= true
default['ida']['package']								= 'idademo68_linux.tgz'
default['radare2']['enabled']							= true
default['radare2']['package']							= 'radare2'
default['radare2']['depends']							= ['libmagic-dev','libmagic1','libradare2-0.9.6','libradare2-common','libradare2-dev']
default['hopper']['enabled']							= true
default['hopper']['package']							= 'hopperv3-3.9.5.deb'
default['hopper']['depends']							= ['libavahi-common3','libavahi-client3','libxslt1.1','libpthread-workqueue0','libkqueue0','libblocksruntime0','libqt5core5a','libqt5gui5','libqt5network5','libqt5printsupport5','libqt5widgets5','libqt5xml5']
default['hopper']['dir']								= '/opt'
default['pwntools']['enabled']							= true
default['pwntools']['package']							= 'pwntools'
default['pwntools']['depends']							= ['python-dev','python-pip','binutils-aarch64-linux-gnu']
default['qemu']['enabled']								= true
default['qemu']['version']								= ['qemu-system-arm']
default['qemu']['kernel']['arm']						= 'kernel_ARMv6_3.12.28'
default['burp']['enabled']								= true
default['burp']['package']								= 'burpsuite_free_v1.6.30.jar'
```
NOTE: *Some attributes may require changing based on user cases.*

## Usage
This cookbook has been designed to work in conjunction with Vagrant's instance but can work with Amazon's OpsWork.

Initialises the current directory to be a Vagrant environment.
```
$ vagrant init
```

Download the cookbook from GitHub into the current vagrant directory.
```
$ git clone https://github.com/InfamousSYN/CTF-Cookbook.git
```

Download the apt cookbook from git into the current vagrant directory.
```
$ git clone https://github.com/opscode-cookbooks/apt.git
```

Replace the Vagrantfile created by the "vagrant init" command with the Vagrantfile from the CTF-Cookbook. *This command is to be performed from within vagrant initalised directory.*
```
$ cp ./CTF-Cookbook/Vagrantfile ./Vagrantfile
```
NOTE: *Depending on your hardware and configuration, settings specifed within the Vagrantfile may not be optimal for you case.*

Launch Vagrant instance. *This command is to be performed from within vagrant initalised directory.*
```
$ vagrant up
```

NOTE: *Certain files referenced in the CTF-Cookbook atrributes have not been pushed intentionally to GitHub. This will require modification of the Cookbook attributes to suit your scenario and/or download the missing files which are then placed into the default/files directory before starting the instance.*

Shutdown Vagrant instance gracefully. *This command is to be performed from within vagrant initalised directory.*
```
$ vagrant halt
```

### Recipes

#### default
The default recipe is used as a launching platform for other recipes used by the Cookbook. Also installs packages which are required for minimal functionality.

#### ctf-tools
Tools that are intended to be used specifically in CTFs are deployed from this recipe. Currently the tools to be deployed are:
* `checksec.sh` - A tool which is used to check for mitigations such as RELRO, NX, Stack Canaries, ASLR and PIE.
* `Hopper` - A 64-bit Linux based Disassembler tool.
* `IDA` - A multi-processor disassembler and debugger tool.
* `Pwntools` - A CTF framework and exploit development Python library.
* `qemu-system-arm` - Adds a Qemu environment to host.
* `Burp Suite` - The Burp web proxy is now supported.

##### References
* http://www.trapkit.de/tools/checksec.sh
* http://www.hopperapp.com/download.html#linux | http://www.hopperapp.com/install_linux.html
* https://www.hex-rays.com/products/ida/
* https://pwntools.readthedocs.org/en/latest/index.html

#### x86_libraries
This recipe will install the required x86 libraries.

## Authors and License

|  Authors   | Contact              |
|------------|:--------------------:|
|InfamousSYN |ainfamoussyn@gmail.com|

Please read the LICENSE file for more information.
