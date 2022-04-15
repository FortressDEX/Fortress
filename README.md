# Fortress
Fortress Core
=============

Setup
---------------------
Fortress Core is the original Fortress client and it builds the backbone of the network. It downloads and, by default, stores the entire history of Fortress transactions, which requires approximately 22 megabytes (MB) of disk space. Depending on the speed of your computer and network connection, the synchronization process can take anywhere from a few hours to a day or more.

To download Fortress Core (COMING SOON), visit [download.fortresscorp.org](https://explorer.fortresscorp.org/).

Running
---------------------
The following are some helpful notes on how to run Fortress Core on your native platform.

### Unix

Unpack the files into a directory and run:

- `bin/fortress-qt` (GUI) or
- `bin/fortressd` (headless)

### Windows

Unpack the files into a directory, and then run fortress-qt.exe.

### macOS

Drag Fortress Core to your applications folder, and then run Fortress Core.

### Need Help?

* See the documentation at the [Fortress Wiki](https://fortress.info/)
for help and more information.
* Ask for help on [#fortress](http://webchat.freenode.net?channels=fortress) on Freenode. If you don't have an IRC client use [webchat here](http://webchat.freenode.net?channels=fortress).
* Ask for help on the [FortressTalk](https://fortresstalk.io/) forums, in the [Technical Support section](https://fortresstalk.io/c/technical-support).

Building
---------------------
The following are developer notes on how to build Fortress Core on your native platform. They are not complete guides, but include notes on the necessary libraries, compile flags, etc.

- [Dependencies](dependencies.md)
- [macOS Build Notes](build-osx.md)
- [Unix Build Notes](build-unix.md)
- [Windows Build Notes](build-windows.md)
- [FreeBSD Build Notes](build-freebsd.md)
- [OpenBSD Build Notes](build-openbsd.md)
- [NetBSD Build Notes](build-netbsd.md)
- [Gitian Building Guide (External Link)](https://github.com/bitcoin-core/docs/blob/master/gitian-building.md)

Development
---------------------
The Fortress repo's [root README](/README.md) contains relevant information on the development process and automated testing.

- [Developer Notes](developer-notes.md)
- [Productivity Notes](productivity.md)
- [Release Notes](release-notes.md)
- [Release Process](release-process.md)
- [Translation Process](translation_process.md)
- [Translation Strings Policy](translation_strings_policy.md)
- [JSON-RPC Interface](JSON-RPC-interface.md)
- [Unauthenticated REST Interface](REST-interface.md)
- [Shared Libraries](shared-libraries.md)
- [BIPS](bips.md)
- [Dnsseed Policy](dnsseed-policy.md)
- [Benchmarking](benchmarking.md)

### Resources
* Discuss on the [FortressTalk](https://fortresstalk.io/) forums.
* Discuss general Fortress development on #fortress-dev on Freenode. If you don't have an IRC client use [webchat here](http://webchat.freenode.net/?channels=fortress-dev.

### Miscellaneous
- [Assets Attribution](assets-attribution.md)
- [bitcoin.conf Configuration File](bitcoin-conf.md)
- [Files](files.md)
- [Fuzz-testing](fuzzing.md)
- [Reduce Traffic](reduce-traffic.md)
- [Tor Support](tor.md)
- [Init Scripts (systemd/upstart/openrc)](init.md)
- [ZMQ](zmq.md)
- [PSBT support](psbt.md)

License
---------------------
Distributed under the [MIT software license](/COPYING).
This product includes software developed by the OpenSSL Project for use in the [OpenSSL Toolkit](https://www.openssl.org/). This product includes
cryptographic software written by Eric Young ([eay@cryptsoft.com](mailto:eay@cryptsoft.com)), and UPnP software written by Thomas Bernard.

# Tutorial - Compile Windows wallet on Ubuntu Server 18.04
### Compile a wallet for Microsoft Windows on Ubuntu Server 18.04 with the following tutorial.

Update your Ubuntu server with the following command:

    sudo apt-get update && sudo apt-get upgrade -y

Install the required dependencies with the following command:

    sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 curl libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev libboost-all-dev libboost-program-options-dev libminiupnpc-dev libzmq3-dev libgmp3-dev libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler libqrencode-dev unzip doxygen cmake nsis wine-stable wine64 bc -y

Install the repository ppa:bitcoin/bitcoin with the following command:

    sudo add-apt-repository ppa:bitcoin/bitcoin

Confirm the installation of the repository by pressing on the enter key. enter

Install Berkeley DB with the following command:

    sudo apt-get update && sudo apt-get install libdb4.8-dev libdb4.8++-dev -y

Create your source code directory with the following commands:

    cd ~/
    mkdir source_code
    cd source_code

Download the source code of your coin with the following command:

    wget "https://dl.walletbuilders.com/download?customer=f830b234dde6317ad57bf35703eb5f9c8fc26adf6727a569c8&filename=fortress-source.tar.gz" -O fortress-source.tar.gz

Type the following command to extract the tar file:

    tar -xzvf fortress-source.tar.gz

Type the following command to download the update for QT:

    wget https://raw.githubusercontent.com/walletbuilders/qt_fix/main/qt_fix_scrypt_pow_018.diff

Type the following command to update QT:

    patch -p1 < qt_fix_scrypt_pow_018.diff

64-bit

Install the required dependencies with the following command:

    sudo apt-get install g++-mingw-w64-x86-64 -y

Set the default x86_64-w64-mingw32-g++ compiler option to posix with the following command:

    sudo update-alternatives --set x86_64-w64-mingw32-g++ /usr/bin/x86_64-w64-mingw32-g++-posix

Build x86_64-w64-mingw32 with the following commands:

    PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g')
    cd depends
    make HOST=x86_64-w64-mingw32
    cd ..

Type the following commands to compile your 64 bit wallet for Microsoft Windows.

    ./autogen.sh
    CONFIG_SITE=$PWD/depends/x86_64-w64-mingw32/share/config.site ./configure --prefix=/
    make

32-bit

Type the following command to clean your source code:

    make clean

Install the required dependencies with the following command:

    sudo apt-get install g++-mingw-w64-i686 mingw-w64-i686-dev -y

Set the default i686-w64-mingw32-gcc and i686-w64-mingw32-g++ compiler option to posix with the following commands.

    sudo update-alternatives --set i686-w64-mingw32-gcc /usr/bin/i686-w64-mingw32-gcc-posix
    sudo update-alternatives --set i686-w64-mingw32-g++ /usr/bin/i686-w64-mingw32-g++-posix

Build i686-w64-mingw32 with the following commands:

    PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g')
    cd depends
    make HOST=i686-w64-mingw32
    cd ..

Type the following commands to compile your 32 bit wallet for Microsoft Windows.

    ./autogen.sh
    CONFIG_SITE=$PWD/depends/i686-w64-mingw32/share/config.site ./configure --prefix=/
    make

The compiled wallet for Microsoft Windows is located in the directory src/qt, the tools are located in the directory src

