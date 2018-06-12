# ![logo](./anoncoin_logo_doxygen.png) Building Anoncoin for OSX

[Home](../README.md) |
**[Installation](../README.md#quick-start)** |
[Developers](./doc/DEVELOPER.md) |
[Project](https://github.com/Anoncoin/anoncoin/projects/1)

[Linux](./INSTALLATION_LINUX.md) |
**OSX** |
[Windows](./INSTALLATION_WINDOWS.md)

Preparation
-----------

**Command Line Tools**

Ensure the OSX command line tools are installed:

```bash
xcode-select --install
```

When the popup appears, click `Install`.

**Homebrew**

Install [Homebrew](https://brew.sh) by running the following command:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
**I2P**

Install the Invisible Internet Project (I2P):

```bash
brew install i2p
```

**Other Dependencies**

Use Homebrew to install the dependencies on your machine:

```bash
brew update && brew upgrade
brew install automake berkeley-db4 boost git libevent librsvg libtool miniupnpc openssl pkg-config protobuf python3 qt
```

Installation From Source
------------------------

**Source Code**

Clone the Anoncoin source code and cd into `anoncoin`

```bash
git clone https://github.com/Anoncoin/anoncoin.git
cd anoncoin
```

Configure and build the headless Anoncoin binaries as well as the GUI:

**Set Flags**

Anoncoin needs to be compiled with a newer version of C++ than what is shipped with OSX:

```bash
export CXXFLAGS=-std=c++11
```

**Build**

Compile the Anoncoin code as follows:

```bash
./autogen.sh
./configure
make
```

*NOTE: You can pass in the following flags into `configure` if needed:*
* `--without-gui`: disable the GUI
* `--with-qt-incdir=/usr/local/opt/qt/include`: in case the QT `include` directory is not found
* `--with-qt-libdir=/usr/local/opt/qt/lib`: in case the QT `library` directory is not found

**Tests**

It is recommended to build and run the unit tests:

```bash
make check
```

**App Bundle**

You can also create a .dmg that contains the .app bundle:

```bash
make deploy
```

**Configuration**

First, launch the I2P router:

```bash
i2prouter start
```
Next, generate your I2P destination key from within the Anoncoin daemon:

```bash
./src/anoncoind i2p.options.enabled=1 -onlynet=i2p -generatei2pdestination
```
