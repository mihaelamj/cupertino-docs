---
source: https://www.swift.org/install/linux/tarball
crawled: 2025-11-15T21:55:22Z
---

# Linux Installation via Tarball

# Linux Installation via Tarball

 
 
 

 Installation via Tarball 
  
 

**1. Install required dependencies:**

 Amazon Linux 2
 


```bash
$ yum install 
      binutils 
      gcc 
      git 
      glibc-static 
      zip 
      gzip 
      libbsd 
      libcurl 
      libedit 
      libicu 
      libsqlite 
      libstdc++-static 
      libuuid 
      libxml2 
      tar 
      tzdata
```



 CentOS 7
 


```bash
$ yum install 
      binutils 
      gcc 
      git 
      glibc-static 
      libbsd-devel 
      libedit 
      libedit-devel 
      libicu-devel 
      libstdc++-static 
      pkg-config 
      python2 
      sqlite

      # __block conflicts with clang's __block qualifier
      sed -i -e 's/*__block/*__libc_block/g' /usr/include/unistd.h
```



 Debian 12
 


```bash
$ apt install 
      binutils-gold 
      gcc 
      git 
      libcurl4-openssl-dev 
      libedit-dev 
      libicu-dev 
      libncurses-dev 
      libpython3-dev 
      libsqlite3-dev 
      libxml2-dev 
      pkg-config 
      tzdata 
      uuid-dev
```



 Fedora 39
 


```bash
$ yum install 
      binutils 
      gcc 
      git 
      libcurl-devel 
      libedit-devel 
      libicu-devel 
      libuuid-devel 
      libxml2-devel 
      python3-devel 
      sqlite-devel 
      zip 
      unzip
```



 Red Hat UBI 9
 


```bash
yum install         
  git               
  gcc-c++           
  libcurl-devel     
  libedit-devel     
  libuuid-devel     
  libxml2-devel     
  ncurses-devel     
  python3-devel     
  rsync             
  sqlite-devel      
  unzip             
  zip
```



 Ubuntu 24.04
 


```bash
$ apt-get install 
          binutils 
          git 
          gnupg2 
          libc6-dev 
          libcurl4-openssl-dev 
          libedit2 
          libgcc-13-dev 
          libncurses-dev 
          libpython3-dev 
          libsqlite3-0 
          libstdc++-13-dev 
          libxml2-dev 
          libz3-dev 
          pkg-config 
          tzdata 
          zip 
          unzip 
          zlib1g-dev
```



 Ubuntu 23.10
 


```bash
$ apt-get install 
          binutils 
          git 
          gnupg2 
          libc6-dev 
          libcurl4-openssl-dev 
          libedit2 
          libgcc-11-dev 
          libpython3-dev 
          libsqlite3-0 
          libstdc++-11-dev 
          libxml2-dev 
          libz3-dev 
          pkg-config 
          python3-lldb-13 
          tzdata 
          zip 
          unzip 
          zlib1g-dev
```



 Ubuntu 22.04
 


```bash
$ apt-get install 
          binutils 
          git 
          gnupg2 
          libc6-dev 
          libcurl4-openssl-dev 
          libedit2 
          libgcc-11-dev 
          libpython3-dev 
          libsqlite3-0 
          libstdc++-11-dev 
          libxml2-dev 
          libz3-dev 
          pkg-config 
          python3-lldb-13 
          tzdata 
          zip 
          unzip 
          zlib1g-dev
```



 Ubuntu 20.04
 


```bash
$ apt-get install 
          binutils 
          git 
          gnupg2 
          libc6-dev 
          libcurl4 
          libedit2 
          libgcc-9-dev 
          libpython2.7 
          libsqlite3-0 
          libstdc++-9-dev 
          libxml2 
          libz3-dev 
          pkg-config 
          tzdata 
          uuid-dev 
          zlib1g-dev
```



 Ubuntu 18.04
 


```bash
$ apt-get install 
          binutils 
          git 
          libc6-dev 
          libcurl4 
          libedit2 
          libgcc-5-dev 
          libpython2.7 
          libsqlite3-0 
          libstdc++-5-dev 
          libxml2 
          pkg-config 
          tzdata 
          zlib1g-dev
```



**2. Download the latest binary release** ([6.2.1](/download/#releases)).

The `swift-<VERSION>-<PLATFORM>.tar.gz` file is the toolchain itself.
The `.sig` file is the digital signature.

**3. Import and verify the PGP signature:**

Skip this step if you have imported the keys in the past. This does not apply to verify.

 Import PGP keys details
 

```
$ gpg --keyserver hkp://keyserver.ubuntu.com 
          --recv-keys 
          'A62A E125 BBBF BB96 A6E0  42EC 925C C1CC ED3D 1561'
          'E813 C892 820A 6FA1 3755  B268 F167 DF1A CF9C E069'
    
```



 or:

 
 

```
$ wget -q -O - https://swift.org/keys/all-keys.asc | 
        gpg --import -
      
```


 

 Verify the PGP signature

 
 If `gpg` fails to verify and reports “BAD signature”,
 do not use the downloaded toolchain.
 Instead, please email [swift-infrastructure@forums.swift.org](mailto:swift-infrastructure@forums.swift.org)
 with as much detail as possible,
 so that we can investigate the problem.
 
 The `.tar.gz` archives for Linux are signed using GnuPG
 with one of the keys of the Swift open source project.
 Everyone is strongly encouraged to verify the signatures
 before using the software.
 First, refresh the keys to download new key revocation certificates,
 if any are available:

 
 
 

```
$ gpg --keyserver hkp://keyserver.ubuntu.com --refresh-keys Swift
```


 
 
 Then, use the signature file to verify that the archive is intact:

 
 
 

```
$ gpg --verify swift-<VERSION>-<PLATFORM>.tar.gz.sig
  ...
  gpg: Good signature from "Swift Automatic Signing Key #4 <swift-infrastructure@forums.swift.org>"
        
```


 
 
 If `gpg` fails to verify because you don’t have the public key (`gpg: Can't
 check signature: No public key`), please follow the instructions in [Active Signing Keys](#active-signing-keys) below to import the keys into your keyring.
 
 You might see a warning:

 
 
 

```
gpg: WARNING: This key is not certified with a trusted signature!
  gpg: There is no indication that the signature belongs to the owner.
        
```


 
 
 This warning means that there is no path in the Web of Trust between this
 key and you. The warning is harmless as long as you have followed the steps
 above to retrieve the key from a trusted source.

 [Active Signing Keys](/keys/active)

 [Expired Signing Keys](/keys/expired)

**4. Extract the archive with the following command:**



```
$ tar xzf swift-<VERSION>-<PLATFORM>.tar.gz

```



This creates a `usr/` directory in the location of the archive.

**5. Add the Swift toolchain to your path as follows:**



```
$ export PATH=/path/to/usr/bin:"${PATH}"

```



You can now execute the `swift repl` command to run the REPL or `swift build` to build Swift packages.