# ![logo](./anoncoin_logo_doxygen.png) Building Anoncoin for Linux

[Home](../README.md) |
**Installation** |
[Developers](./doc/DEVELOPER.md) |
[Project](https://github.com/Anoncoin/anoncoin/projects/1) |
Roadmap |
Wallet |
Mining

[OSX](.BUILD_OSX.md)
[Windows](./BUILD_WINDOWS.md) |
[Linux](.BUILD_LINUX.md) |
[Unix](.BUILD_UNIX.md)

## Preparation

## Dependencies

Use `apt-get` to install the dependencies on your machine:

```bash
sudo apt-get update
sudo apt-get install software-properties-common bsdmainutils
sudo apt-get install build-essential libtool autotools-dev autoconf pkg-config libssl-dev git wget
```

For Ubuntu 12.04 and later or Debian 7 and later libboost-all-dev has to be installed:

```bash
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install libboost-all-dev
```

You also need some C++ header files which are being hosted by Bitcoin:

```bash
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install libdb4.8-dev libdb4.8++-dev
```

## Obtain Source Code

Clone the Anoncoin source code and cd into `anoncoin`

```bash
git clone https://github.com/Anoncoin/anoncoin.git
cd anoncoin
```

## Install Berkeley DB

It is recommended to use Berkeley DB 4.8. To build it yourself:

```bash
./contrib/install_db4.sh .
```

*NOTE: You only need Berkeley DB if the wallet is enabled (see the section `--disable-wallet-mode` below)*


## Build Anoncoin Core

Configure and build the headless Anoncoin binaries as well as the GUI:

#### Set Flags

In order for Anoncoin to find the correct BDB lib, some flags must be set before compiling:

```bash
export BDB_PREFIX="$(pwd)/anoncoin/db4"
```

Note the update to configure if you have Berkeley DB installed:

```bash
./autogen.sh
./configure BDB_LIBS="-L${BDB_PREFIX}/lib -libdb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include"
make
```

*NOTE: You can disable the GUI build by passing `--without-gui` to configure.*


It is recommended to build and run the unit tests:

```bash
make check
```

## App bundle

You can also create a .dmg that contains the .app bundle (optional):

```bash
make deploy
```

## Running Anoncoin Core

#### RPC Config

Before running, it's recommended you create an RPC configuration file

```bash
echo -e "rpcuser=anoncoinrpc\nrpcpassword=$(xxd -l 16 -p /dev/urandom)" > "/root/.anoncoin/anoncoin.conf"
chmod 600 "/root/.anoncoin/anoncoin.conf"
```

#### Bootstrap

The first time you run anoncoind, it will start downloading the blockchain. This process could take several hours.  To speed up the process, you can download a bootstrap file.

From your browser, download the `bootstrap.dat` file from [mega.nz](https://mega.nz/#!IqACmRhL!2Ti8rUlsnWoD4d5q3boMHQwaEbbqmxZqYq6FmWevVxI) or use [Bittorrent](./BOOTSTRAP.md).

Move the Bootstrap file to Data Directory:

```
mv ~/Downloads/bootstrap.dat "${HOME}/Library/Application Support/Anoncoin"
```

*Note: this is safe - the `bootstrap.dat` file will be verified on the blockchain*


Anoncoin Core is now available at `./src/anoncoind`


You can monitor the download process by looking at the debug.log file:

    tail -f $HOME/Library/Application\ Support/Anoncoin/debug.log

Other commands:
-------

    ./src/anoncoind -daemon # Starts the anoncoin daemon.
    ./src/anoncoin-cli --help # Outputs a list of command-line options.
    ./src/anoncoin-cli help # Outputs a list of RPC commands when the daemon is running.
