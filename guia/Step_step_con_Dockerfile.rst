Step to step con Dockerfile
=================================

::

	[docker@docker weblogic-consis]$ docker build -t "weblogic" .
	Sending build context to Docker daemon  1.061GB
	Step 1/26 : FROM centos:7
	7: Pulling from library/centos
	469cfcc7a4b3: Pull complete 
	Digest: sha256:989b936d56b1ace20ddf855a301741e52abca38286382cba7f44443210e96d16
	Status: Downloaded newer image for centos:7
	 ---> e934aafc2206
	Step 2/26 : MAINTAINER Carlos Gomez G cgomeznt@gmail.com
	 ---> Running in d885f8c22368
	Removing intermediate container d885f8c22368
	 ---> a1950d9bb711
	Step 3/26 : ENV     container docker
	 ---> Running in be61d690712a
	Removing intermediate container be61d690712a
	 ---> 17e244c6d383
	Step 4/26 : RUN     yum -y update
	 ---> Running in d4a24709718e
	Loaded plugins: fastestmirror, ovl
	Determining fastest mirrors
	 * base: centos.mirror.iweb.ca
	 * extras: centos.mirror.iweb.ca
	 * updates: centos.mirror.iweb.ca
	No packages marked for update
	Removing intermediate container d4a24709718e
	 ---> 9499e470f373
	Step 5/26 : RUN     yum -y install sudo         tar         gzip         openssh-clients         vi         find
	 ---> Running in 0dd0098755b9
	Loaded plugins: fastestmirror, ovl
	Loading mirror speeds from cached hostfile
	 * base: centos.mirror.iweb.ca
	 * extras: centos.mirror.iweb.ca
	 * updates: centos.mirror.iweb.ca
	Package 2:tar-1.26-32.el7.x86_64 already installed and latest version
	Package gzip-1.5-9.el7.x86_64 already installed and latest version
	Package 2:vim-minimal-7.4.160-2.el7.x86_64 already installed and latest version
	No package find available.
	Resolving Dependencies
	--> Running transaction check
	---> Package openssh-clients.x86_64 0:7.4p1-13.el7_4 will be installed
	--> Processing Dependency: openssh = 7.4p1-13.el7_4 for package: openssh-clients-7.4p1-13.el7_4.x86_64
	--> Processing Dependency: fipscheck-lib(x86-64) >= 1.3.0 for package: openssh-clients-7.4p1-13.el7_4.x86_64
	--> Processing Dependency: libfipscheck.so.1()(64bit) for package: openssh-clients-7.4p1-13.el7_4.x86_64
	--> Processing Dependency: libedit.so.0()(64bit) for package: openssh-clients-7.4p1-13.el7_4.x86_64
	---> Package sudo.x86_64 0:1.8.19p2-11.el7_4 will be installed
	--> Running transaction check
	---> Package fipscheck-lib.x86_64 0:1.4.1-6.el7 will be installed
	--> Processing Dependency: /usr/bin/fipscheck for package: fipscheck-lib-1.4.1-6.el7.x86_64
	---> Package libedit.x86_64 0:3.0-12.20121213cvs.el7 will be installed
	---> Package openssh.x86_64 0:7.4p1-13.el7_4 will be installed
	--> Running transaction check
	---> Package fipscheck.x86_64 0:1.4.1-6.el7 will be installed
	--> Finished Dependency Resolution

	Dependencies Resolved

	================================================================================
	 Package             Arch       Version                       Repository   Size
	================================================================================
	Installing:
	 openssh-clients     x86_64     7.4p1-13.el7_4                updates     654 k
	 sudo                x86_64     1.8.19p2-11.el7_4             updates     1.1 M
	Installing for dependencies:
	 fipscheck           x86_64     1.4.1-6.el7                   base         21 k
	 fipscheck-lib       x86_64     1.4.1-6.el7                   base         11 k
	 libedit             x86_64     3.0-12.20121213cvs.el7        base         92 k
	 openssh             x86_64     7.4p1-13.el7_4                updates     509 k

	Transaction Summary
	================================================================================
	Install  2 Packages (+4 Dependent packages)

	Total download size: 2.3 M
	Installed size: 8.6 M
	Downloading packages:
	warning: /var/cache/yum/x86_64/7/base/packages/fipscheck-lib-1.4.1-6.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
	Public key for fipscheck-lib-1.4.1-6.el7.x86_64.rpm is not installed
	Public key for openssh-7.4p1-13.el7_4.x86_64.rpm is not installed
	--------------------------------------------------------------------------------
	Total                                              101 kB/s | 2.3 MB  00:23     
	Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
	Importing GPG key 0xF4A80EB5:
	 Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
	 Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
	 Package    : centos-release-7-4.1708.el7.centos.x86_64 (@CentOS)
	 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
	Running transaction check
	Running transaction test
	Transaction test succeeded
	Running transaction
	  Installing : fipscheck-1.4.1-6.el7.x86_64                                 1/6 
	  Installing : fipscheck-lib-1.4.1-6.el7.x86_64                             2/6 
	  Installing : openssh-7.4p1-13.el7_4.x86_64                                3/6 
	  Installing : libedit-3.0-12.20121213cvs.el7.x86_64                        4/6 
	  Installing : openssh-clients-7.4p1-13.el7_4.x86_64                        5/6 
	  Installing : sudo-1.8.19p2-11.el7_4.x86_64                                6/6 
	  Verifying  : fipscheck-lib-1.4.1-6.el7.x86_64                             1/6 
	  Verifying  : fipscheck-1.4.1-6.el7.x86_64                                 2/6 
	  Verifying  : libedit-3.0-12.20121213cvs.el7.x86_64                        3/6 
	  Verifying  : openssh-7.4p1-13.el7_4.x86_64                                4/6 
	  Verifying  : openssh-clients-7.4p1-13.el7_4.x86_64                        5/6 
	  Verifying  : sudo-1.8.19p2-11.el7_4.x86_64                                6/6 

	Installed:
	  openssh-clients.x86_64 0:7.4p1-13.el7_4    sudo.x86_64 0:1.8.19p2-11.el7_4   

	Dependency Installed:
	  fipscheck.x86_64 0:1.4.1-6.el7            fipscheck-lib.x86_64 0:1.4.1-6.el7  
	  libedit.x86_64 0:3.0-12.20121213cvs.el7   openssh.x86_64 0:7.4p1-13.el7_4     

	Complete!
	Removing intermediate container 0dd0098755b9
	 ---> ab334f829f83
	Step 6/26 : RUN     groupadd oinstall
	 ---> Running in e6ca28dba654
	Removing intermediate container e6ca28dba654
	 ---> c5790c171303
	Step 7/26 : RUN     useradd -g oinstall oracle
	 ---> Running in bc9608553a58
	Removing intermediate container bc9608553a58
	 ---> 474053dd06c2
	Step 8/26 : RUN	mkdir -p /u01/software
	 ---> Running in f6dd92b4ba7a
	Removing intermediate container f6dd92b4ba7a
	 ---> 04578007d2da
	Step 9/26 : RUN	mkdir -p /u01/app/oracle/middleware
	 ---> Running in 21dd10994224
	Removing intermediate container 21dd10994224
	 ---> b51a1b36de76
	Step 10/26 : RUN	mkdir -p /u01/app/oracle/config/domains
	 ---> Running in 917c14465229
	Removing intermediate container 917c14465229
	 ---> 3959a8bd0eea
	Step 11/26 : RUN	mkdir -p /u01/app/oracle/config/applications
	 ---> Running in 4eeeaea558f4
	Removing intermediate container 4eeeaea558f4
	 ---> 4c4c760c32b3
	Step 12/26 : RUN	chown -R oracle:oinstall /u01
	 ---> Running in 59727c733dfd
	Removing intermediate container 59727c733dfd
	 ---> 50dc3934535b
	Step 13/26 : RUN	chmod -R 775 /u01/
	 ---> Running in 7dff56ac31c7
	Removing intermediate container 7dff56ac31c7
	 ---> 066b3187b083
	Step 14/26 : ENV	export MW_HOME=/u01/app/oracle/middleware
	 ---> Running in 71861cc6cdeb
	Removing intermediate container 71861cc6cdeb
	 ---> 2c4cf8e224d8
	Step 15/26 : ENV	export WLS_HOME=$MW_HOME/wlserver
	 ---> Running in 3a5f3830a5ee
	Removing intermediate container 3a5f3830a5ee
	 ---> c632b9c08965
	Step 16/26 : ENV	export WL_HOME=$WLS_HOME
	 ---> Running in e2d0a475fc80
	Removing intermediate container e2d0a475fc80
	 ---> b38a1e59046f
	Step 17/26 : ENV	export JAVA_HOME=/u01/app/oracle/jdk1.8.0_77
	 ---> Running in fadb76ef1a59
	Removing intermediate container fadb76ef1a59
	 ---> 10140fa0c16d
	Step 18/26 : ENV	export PATH=$JAVA_HOME/bin:$PATH
	 ---> Running in 08df13067c90
	Removing intermediate container 08df13067c90
	 ---> 350db641af75
	Step 19/26 : COPY	jdk-7u79-linux-x64.rpm	/u01/software
	 ---> a5c32dd3a28c
	Step 20/26 : RUN	rpm -ivh /u01/software/jdk-7u79-linux-x64.rpm
	 ---> Running in 3a8e63ef8227
	Preparing...                          ########################################
	Updating / installing...
	jdk-2000:1.7.0_79-fcs                 ########################################
	Unpacking JAR files...
		rt.jar...
		jsse.jar...
		charsets.jar...
		tools.jar...
		localedata.jar...
		jfxrt.jar...
	Removing intermediate container 3a8e63ef8227
	 ---> f3e9471a00fb
	Step 21/26 : COPY	wls.rsp /u01/software
	 ---> 437ac6a8b6bb
	Step 22/26 : COPY	orainst.loc /u01/software
	 ---> 2dc66e3b3bb4
	Step 23/26 : COPY 	fmw_12.1.3.0.0_wls.jar /u01/software
	 ---> 8ebc02fe9ba0
	Step 24/26 : USER	oracle
	 ---> Running in ffc8f9de7a91
	Removing intermediate container ffc8f9de7a91
	 ---> 020108a398c6
	Step 25/26 : RUN 	$JAVA_HOME/bin/java -Xmx512m -jar /u01/software/fmw_12.1.3.0.0_wls.jar -silent -responseFile /u01/software/wls.rsp -invPtrLoc /u01/software/orainst.loc
	 ---> Running in 60472142486d
	Launcher log file is /tmp/OraInstall2018-04-30_12-56-49PM/launcher2018-04-30_12-56-49PM.log.
	Extracting files.......................................
	Starting Oracle Universal Installer

	Checking if CPU speed is above 300 MHz.   Actual 2993.200 MHz    Passed
	Checking swap space: must be greater than 512 MB.   Actual 1048572 MB    Passed
	Checking if this platform requires a 64-bit JVM.   Actual 64    Passed (64-bit not required)
	Checking temp space: must be greater than 300 MB.   Actual 7718 MB    Passed


	Preparing to launch the Oracle Universal Installer from /tmp/OraInstall2018-04-30_12-56-49PM
	Log: /tmp/OraInstall2018-04-30_12-56-49PM/install2018-04-30_12-56-49PM.log
	Copyright (c) 1996, 2014, Oracle and/or its affiliates. All rights reserved.
	Reading response file..
	Starting check : CertifiedVersions
	/bin/cat: /proc/sys/net/core/wmem_default: No such file or directory
	Starting check : CheckJDKVersion
	Expected result: 1.7.0_15
	Actual Result: 1.7.0_79
	Check complete. The overall result of this check is: Passed
	CheckJDKVersion Check: Success.
	Validations are enabled for this session.
	Verifying data......
	Copying Files...
	You can find the log of this install session at:
	 /tmp/OraInstall2018-04-30_12-56-49PM/install2018-04-30_12-56-49PM.log
	-----------20%----------40%----------60%----------80%--------100%

	The installation of Oracle Fusion Middleware 12c WebLogic Server and Coherence 12.1.3.0.0 completed successfully.
	Logs successfully copied to /u01/app/oraInventory/logs.
	Removing intermediate container 60472142486d
	 ---> 06719ceebaf3
	Step 26/26 : EXPOSE	7001
	 ---> Running in eb1e3d10dca9
	Removing intermediate container eb1e3d10dca9
	 ---> 8743af306724
	Successfully built 8743af306724
	Successfully tagged weblogic:latest

::

	[docker@docker weblogic-consis]$ docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
	weblogic            latest              8743af306724        55 seconds ago      2.52GB
	centos              7                   e934aafc2206        3 weeks ago         199MB

::

	[docker@docker weblogic-consis]$ docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

::

	[docker@docker weblogic-consis]$ docker run -dti --name "WebLogic" -p 7001:7001 "weblogic"
	30f6c9e3a0f81d2b341b03fb69a7434d31183da88a033b35b97dd6ee69a16a53

::

	[docker@docker weblogic-consis]$ docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
	30f6c9e3a0f8        weblogic            "/bin/bash"         8 seconds ago       Up 2 seconds        0.0.0.0:7001->7001/tcp   WebLogic

::

	[docker@docker weblogic-consis]$ docker stop WebLogic
	WebLogic

::

	[docker@docker weblogic-consis]$ docker rm WebLogic
	WebLogic


==========================================================================================================================::

	[docker@docker weblogic-consis]$ docker run -dti --name "WebLogic" -p 7001:7001 "weblogic"
	7de1e88f9ff63a6b3c7f0e8880c126199a14e047980d5a5384348d5cffa717b4

::

	[docker@docker weblogic-consis]$ docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
	7de1e88f9ff6        weblogic            "/bin/bash"         9 seconds ago       Up 5 seconds        0.0.0.0:7001->7001/tcp   WebLogic

::

	[docker@docker weblogic-consis]$ docker exec -i -t WebLogic /bin/bash
	[oracle@7de1e88f9ff6 /]$ 


