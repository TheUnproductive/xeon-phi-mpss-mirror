Intel(R) Manycore Platform Software Stack MPSS 3.8 README (Intel(R) MPSS README)

Intel, Xeon, and Intel Xeon Phi are trademarks of Intel Corporation in the U.S.
and/or other countries

Firmware and hardware requirements for Intel(R) MPSS 3.8 Software Release:

NOTE: With each release firmware updates are required.
 _______________________________________________________________
|                                       |                       |
|      Flash Revision                   |    2.1.02.0391        |
|---------------------------------------|-----------------------|
|      SMC Firmware Version             |    1.17.6900          |
|---------------------------------------|-----------------------|
|      SMC Boot Loader Version          |    1.8.4326           |
|---------------------------------------|-----------------------|
|      B-step or later Intel(R)         |                       |
|      Xeon Phi(TM) coprocessor         |                       |
|_______________________________________|_______________________|
If Intel(R) Xeon Phi(TM) coprocessor firmware is older than 375-1 or hardware 
earlier than B0, please contact your Intel(R) representative for assistance. 

This readme file is for software version Intel(R) MPSS 3.8 release: 
 _______________________________________________________________                
|                                       |                       |
|     Coprocessor OS Version            | 2.6.38.8+mpss3.8      |
|---------------------------------------|-----------------------|
|     Driver Revision                   | 3.8                   |
|_______________________________________|_______________________|


Part Number:  Intel(R) Xeon Phi(TM) MPSS 3.8 README
Export Compliance: ECCN = 5D992a; ECCN = EAR99

Notes:
  o This document pertains to systems containing at least one Intel(R) Xeon 
    Phi(TM) coprocessor.
  o Detailed configuration information and procedures appear in the 
    Intel(R) MPSS User's Guide (MPSS_Users_Guide.pdf).
  o In this document, lines preceded by: 
    - "[host]$" denote a command entered on the host with user privileges.
    - "[host]#" denote a command entered on the host with root privileges.
    
DISCLAIMER:

This document makes reference to products developed by Intel. This section 
highlights restrictions on how these products may be used, and what 
information may be disclosed to others. Contact your Intel field representative
for more information.

Intel is making no claims of usability, efficacy or warranty. The license.txt 
contained herein completely defines the license and use of this software except
in the cases of the GPL components. This document contains information on 
products in the design phase of development. The information here is subject 
to change without notice. Do not finalize a design with this information.

The code contained in these modules may be specific to the Intel(R) Xeon Phi(TM)
coprocessor and is not backward compatible with other Intel(R) products.
Additionally, Intel makes no commitments for support of the code or instruction
set in future products.

*Other names and brands may be claimed as the property of others.


===============================================================================
 Table of Contents
===============================================================================
  1.   Overview
  1.1  Software Revision Numbers  
  2.   Installing Intel(R) MPSS
  2.1  Requirements
  2.2  Steps to Install Intel(R) MPSS
  2.3  Steps to Uninstall Intel(R) MPSS  
  2.4  Intel(R) Xeon Phi(TM) coprocessor Flash & SMC Update
  2.5  SSH Access and Configuration for the Intel(R) Xeon Phi(TM) coprocessor
  2.6  Enable Large Base Address Registers Support (BAR) in the platform BIOS

===============================================================================
1.  Overview
===============================================================================

The Intel(R) MPSS README is for the Intel(R) Manycore Platform Software Stack 
(Intel(R) MPSS) build revision 3.8.

1.1  Software Revision Numbers

The revision numbers of the Intel(R) MPSS 3.8 Linux* Package software 
components are listed below:

Intel(R) MPSS 3.8 Linux* host driver and run time.


===============================================================================
2.  Installing Intel(R) MPSS
===============================================================================

If the RPM overlays have:

  1)     version conflicts with other overlays

  2)     version conflicts with existing packages on the coprocessors

  3)     missing dependencies

all overlays could silently fail to install. 

If applied overlays fail to install, upload all targeted RPMs to the coprocessor
and attempt to manually install them to for conflicts or missing dependencies.

After updating an Intel(R) MPSS install, use the "micctrl --overlay" command to 
verify there are no duplicate or out of date overlays from previous Intel(R) 
MPSS installs.

2.1 Requirements

  1) Super-user privileges are required to install the Intel(R) MPSS 3.8 release

  2) Valid SSH keys, password authentication or host based authentication are 
     required for users (including "root" user) that need SSH access to each 
     Intel(R) Xeon Phi(TM) coprocessor. To set SSH access, see Section 2.5, 
     "SSH Access and Configuration for the Intel(R) Xeon Phi(TM) coprocessor".

  3) Supported host hardware with at least one Intel(R) Xeon Phi(TM) coprocessor
     installed.

  4) Supported Linux* host operating system release kernel versions.
 _______________________________________________________________
|                                       |                       |
| Supported Host OS Versions            | Kernel Version        |
|---------------------------------------|-----------------------|
| Red Hat* Enterprise Linux* 64-bit 6.7 | 2.6.32-573            |
|---------------------------------------|-----------------------|
| Red Hat* Enterprise Linux* 64-bit 6.8 | 2.6.32-642            |
|---------------------------------------|-----------------------|
| Red Hat* Enterprise Linux* 64-bit 6.9 | 2.6.32-696            |
|---------------------------------------|-----------------------|
| Red Hat* Enterprise Linux* 64-bit 7.1 | 3.10.0-229            |
|---------------------------------------|-----------------------|
| Red Hat* Enterprise Linux* 64-bit 7.2 | 3.10.0-327            |
|---------------------------------------|-----------------------|
| Red Hat* Enterprise Linux* 64-bit 7.3 | 3.10.0-514            |
|---------------------------------------|-----------------------|
| SUSE* Linux* Enterprise Server 11 SP4 | 3.0.101-63-default    |
| 64 bit                                |                       |
|---------------------------------------|-----------------------|
| SUSE* Linux* Enterprise Server 12     | 3.12.28-4-default     |
| 64-bit                                |                       |
|---------------------------------------|-----------------------|
| SUSE* Linux* Enterprise Server 12 SP1 | 3.12.49-11-default    |
| 64-bit                                |                       |
|---------------------------------------|-----------------------|
| SUSE* Linux* Enterprise Server 12 SP2 | 4.4.21-69-default     |
| 64-bit                                |                       |
|_______________________________________|_______________________|

CAUTION: Updating the Host OS kernel beyond the officially supported initial
         release versions specified in the table above may prevent the 
         pre-built Intel(R) MPSS 3.8 driver modules from loading. Most Linux* 
         distributions provide minor kernel updates and patches. To ensure 
         compatibility, disable kernel updates on your host OS.                 
                                                                              
         If the host system must run with an updated kernel, use the following  
         steps to re-build the Intel(R) MPSS modules for the updated kernel 
         version prior to installing the software stack.  
         
         If using InfiniBand* as an interconnect, refer to section 3.7 of the 
         Intel(R) MPSS User's Guide. 
                                              
                                                                              
  To recompile the Intel(R) MPSS host kernel modules: 
  
    1) Ensure the prerequisites are installed:                           
       a. For Red Hat* Enterprise Linux*                                 
          [host]#  yum install kernel-headers kernel-devel                
       b. For SUSE* Linux* Enterprise Server 11.x                             
          [host]# zypper install kernel-default-devel rpm
       c. For SUSE* Linux* Enterprise Server 12.x
          [host]# zypper install kernel-default-devel rpm-build               
                                                                              
    2) Regenerate the Intel(R) MPSS driver module package:               
          [host]$  cd <path to extracted MPSS-3.x.-Linux.tar>/src/        
          [host]$  rpmbuild --rebuild mpss-modules-*.rpm                  
                                                                              
    3) The newly built mpss-modules and mpss-modules-dev RPMs will be    
       located at:                                                       
       a. Red Hat* Enterprise Linux*
          $HOME/rpmbuild/RPMS/x86_64/
       b. SUSE* Linux* Enterprise Server
          /usr/src/packages/RPMS/x86_64/                                 
                                                                              
    4) Copy the rebuilt mpss-modules and mpss-modules-dev RPMs from the 
       respective directory specified in step 3 into the /modules        
       directory of the extracted MPSS tar file. Continue to section 2.2 
       to install Intel(R) MPSS.                                                  
      
2.2  Steps to Install Intel(R) MPSS

Note:
  o The software stack build process generates an empty /home/root directory in 
  the Intel(R) Xeon Phi(TM) coprocessor file system. Please do not use this 
  directory. Micctrl will create an entry for root's home at /root and place 
  root's ssh keys in /root/.ssh.

Note:
  o In SUSE*, "/sbin" and "/usr/sbin" are by default not included in the user's 
    execution search path. Attempts to run micctrl and various other commands 
    discussed in this document will fail with the "command not found" error from
    the shell. 

    To avoid this in the current shell session run the following command prior 
    to execution:
       
        export PATH=$PATH:/usr/sbin:/sbin

    To make this change persistent, add the PATH to the user's shell 
    configuration file.
    
Steps:
  1) Remove any previous installations of Intel(R) MPSS. Refer to Section 2.3
     for instructions.
        To check for previous installed version of Intel(R) MPSS package:
            [host]# rpm -qa | grep -e intel-mic -e mpss

    Note:   Both yum and zypper support software upgrades and downgrades. 
            However, it is necessary that Intel(R) MPSS upgrades and downgrades 
            be carried out by completely uninstalling existing software and
            performing by a clean installation of the latest version. Section 
            2.3 describes how to uninstall Intel(R) MPSS.

  2) RHEL* and SLES* 11.x:
     Issues were encountered when configuring the virtual network interfaces 
     for the Intel(R) Xeon Phi(TM) coprocessors when NetworkManager is used. 
     It is strongly recommended to use the network daemon instead.
      
     To switch to the network daemon:

             [host}# chkconfig NetworkManager off
             [host]# chkconfig network on
             [host]# service NetworkManager stop
             [host]# service network start
 
     SLES* 12 requires the wicked nanny service to be enabled for the Intel(R) 
     Xeon Phi(TM) coprocessor virtual Network interfaces to start correctly.
     
     To enable Nanny, edit the /etc/wicked/common.xml, and change
     <use-nanny>false</use-nanny> to "true".

  4) Disarm Module security policies:
    a) The SUSE* Linux* Enterprise Server release requires setting the kernel
       to allow non-SUSE driver modules to be loaded. 
       
           Edit the following file:
                SUSE* 11.x:
                        /etc/modprobe.d/unsupported-modules.conf 
                        
                SUSE* 12.x:
                        /etc/modprobe.d/10-unsupported-modules.conf
 
                Set the value of "allow_unsupported_modules" to 1

  5) Download the tar file for this release, extract it and install the Intel(R)
     MPSS package.
            [host]$ tar xvf mpss-3.8-linux.tar

     This will create the mpss-3.8 directory, which contains a file structure \
     with MPSS related packages.          

            [host]$ cd mpss-3.8
            [host]$ cp ./modules/*`uname -r`*.rpm .
       
        o Red Hat* Enterprise Linux* 
            [host]# yum install *.rpm

        o SUSE* Linux* Enterprise Server
            [host]# zypper install *.rpm
     
     Note: MPSS packages are not GPG signed. If local package GPG checking
           is enabled in the yum.conf or zypp.conf file, use the command below 
           to install the packages:
        
        o Red Hat* Enterprise Linux*    
            [host]# yum install --nogpgcheck *.rpm
            
        o SUSE* Linux* Enterprise Server
            [host]# zypper --no-gpg-checks install  *.rpm 

  6) Load the mic.ko driver, and initialize MPSS Default settings.
            [host]# modprobe mic
            [host]# micctrl --initdefaults

  6a) Configure Intel(R) MPSS via .conf Files (optional).
        o The Intel(R) MPSS User's Guide explains in detail how to modify the
          configuration files.
           
  7) Update Flash & SMC (see Section 2.4). Flash and SMC updates to versions 
     specified in this release are required.

  8) Start Intel(R) MPSS by using the Linux* service command:
         RHEL* 6.x and SLES* 11.x
            [host]# service mpss start
         RHEL* 7.x and SLES* 12.x
            [host]# systemctl start mpss

         RHEL* 6.x and SLES* 11.x
         o To configure the Intel(R) MPSS service to start when the host OS 
           boots:
            [host]# chkconfig mpss on

         o To disable the Intel(R) MPSS service from starting when the host OS 
           boots:
            [host]# chkconfig mpss off

         RHEL* 7.x, and SLES* 12.x
         o To enable or disable service start on the host OS boot: 
            [host]# systemctl enable mpss
            [host]# systemctl disable mpss

2.3  Steps to Uninstall Intel(R) MPSS

  To uninstall 3.x-based builds:

     1) Unload the mpss driver:
        RHEL* 6.x and SLES* 11.x
            [host]# service mpss unload
        RHEL* 7.x and SLES* 12.x
            [host]# systemctl stop mpss
        [host]# modprobe -r mic

     2) Uninstall:
        a. To uninstall MPSS-3.x:
            [host]$ cd mpss-3.x
            [host]# ./uninstall.sh

        b. To uninstall MPSS-2.x:
        o Red Hat* Enterprise Linux*
            [host]# yum remove intel-mic\*

        o SUSE* Linux* Enterprise Server
            [host]# zypper remove intel-mic\*

  2.4  Intel(R) Xeon Phi(TM) coprocessor Flash & SMC Update

********************************************************************************
*  WARNING:  Flash and SMC updates to versions specified in this release are   *
*            required.                                                         *
*                                                                              *
* Using this release with the incorrect Flash and SMC versions is not supported*
* and may lead to erratic behavior.                                            *
********************************************************************************

Note: 
    o These steps will not work if the flash files (ending in .rom.smc) are
      moved to a location other than the default install path.

    o PREREQUISITES:
      - Uninstall prior version of Intel(R) MPSS and install the latest version.
      - Current running version of Flash must be >= 375. If its version differs,
        contact your Intel support representative.

Steps:
  1) Check the status of the coprocessor(s):
            [host]# micctrl -s

     If the status for all of the coprocessors shows 'ready', proceed to step 2;
     otherwise change status of each coprocessor to 'ready':
            [host]# micctrl -rw

  2) Run from the command prompt:
            [host]# /usr/bin/micflash -update -device all -smcbootloader

   NOTE:  If using C0 stepping, or 5110P B1 SKUs with a TA of G65758-253 or
   higher, or if the SMC boot loader version is 1.8, use this command 
   in step 2:
            [host]# /usr/bin/micflash -update -device all

  3) Reboot the host system for all flash and SMC changes to take effect.

2.5  SSH Access and Configuration for the Intel(R) Xeon Phi(TM) coprocessor

Communication with the coprocessor Linux* operating system on the Intel(R) Xeon 
Phi(TM) coprocessor is provided by a standard network interface. The interface 
uses a virtual network driver over the PCIe bus. Standard networking tools 
such as SSH are supported. 

The Intel(R) Xeon Phi(TM) coprocessor Linux* OS supports network access using 
SSH keys, password authentication, or host based authentication, provided that 
valid credentials exist on the coprocessor. (For more information on host based 
authentication see the MPSS User Guide section 6.2.2). The initial configuration
phase of the Intel(R) MPSS creates users for each coprocessor based on the 
current user IDs in the host /etc/passwd file. For each user in /etc/passwd 
(including root), if SSH key files are found in the user's ".ssh" directory, 
those keys are also propagated to the Intel(R) Xeon Phi(TM) coprocessor's file 
system.

The default user credentialing behavior on the coprocessor can be customized 
with the "micctrl --userupdate" option. Administrators who prefer to only 
provide a limited set of user credentials (root, sshd, micuser, nobody and 
nfsnobody) can run the following commands after the initial configuration to 
prevent all users from being propagated: 
         RHEL* 6.x and SLES* 11.x
            [host]# service mpss stop
            [host]# micctrl --userupdate=none 
            [host]# service mpss start
         RHEL* 7.x and SLES* 12.x
            [host]# systemctl stop mpss
            [host]# micctrl --userupdate=none 
            [host]# systemctl start mpss

It is recommended to install the latest version of the OpenSSH software on the 
coprocessor. The mpss-3.x-k1om.tar archive contains, among others, 
openssh*.k1om.rpm and open-ssh-without-openssl*.k1om.rpm packages that can be 
installed in the coprocessor's file system. Refer to section 7 of the user's 
guide for instructions on how to install additional software on the coprocessor.
            
In the case where a user did not have a valid SSH key present during the initial
coprocessor configuration process, they can be added as necessary on a per-user 
basis. To generate the SSH key, each user must execute: 
            [host]$ ssh-keygen

Then, the "micctrl --sshkeys" option is used to allow Intel(R) MPSS to pick up 
the new keys for a specific user. For example, the following updates the 
coprocessor configuration for mic0 to include root's SSH keys: 
          RHEL* 6.x and SLES* 11.x
            [host]# service mpss stop
            [host]# micctrl --sshkeys=root mic0 
            [host]# service mpss start
          RHEL* 7.x and SLES* 12.x
            [host]# systemctl stop mpss
            [host]# micctrl --sshkeys=root mic0 
            [host]# systemctl start mpss 

See the Intel(R) MPSS User's Guide (MPSS_Users_Guide.pdf) for detailed 
information regarding user authentication and customization options including 
support for LDAP and NIS. 

If after executing 'ssh mic0' the following message appears, remove the 
mic0 RSA from the SSH 'known_hosts' file typically found at ~/.ssh/ folder. 
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@ 
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @ 
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@ 
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY! 
    Someone could be eavesdropping on you right now (man-in-the-middle attack)! 

Note: If running an ssh command in the background, it is preferable to use the 
      "-f" option instead of appending "&" to the command: 
           [host]$ ssh -f hostname "sleep 20; echo complete" 

           
2.6 Enable Large Base Address Registers (BAR) Support in the Platform BIOS

In order for Intel(R) Xeon Phi(TM) coprocessors to function properly in a
platform, BIOS and OS support for large (8GB+) Memory Mapped I/O Base Address
Registers (MMIO BAR's) above the 4GB address limit must be enabled.

By default, most platform BIOS implementations have this set to disabled, 
therefore it must be enabled manually in the platform BIOS setup.

Contact your platform and/or BIOS vendor to determine whether changing this 
setting applies to the platform being used.


