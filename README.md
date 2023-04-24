# Squishy Core

![](./doc/imgs/squishy-logo.png)

Squishy Core is the native daemon for SQCN. It's available for three OS platforms - Windows, Linux, MacOS.

Use the following scripts to build:

- Linux: `build.sh` (native build)
- Windows: `build-win.sh` (cross-compilation for Win)
- MacOS: `build-mac-cross.sh` (cross-compilation for OSX)
- MacOS: `build-mac.sh` (native build)


## Tech Specification
- 23,000,000 max coin amount
- 100 coin block reward for era 1 (6 total eras 100, 25, 12.5, 6.25, 3.125, 1.5625)
- 7 min block time
- 25% staking rewards
- Mining Algorithm: Equihash 200,9

## Connect

Discord Server ([SquishyCoin](https://discord.com/invite/zxbBrzAqhZ))

## Getting started

```shell
# Start with Komodo - RECOMMENDED!!
./komodod -ac_name=SQCN -ac_supply=1468750 -ac_eras=6 -ac_blocktime=420 -ac_reward=10000000000,2500000000,1250000000,625000000,312500000,156250000 -ac_end=10000,100000,500000,1500000,2500000,5000000 -ac_staked=25 -ac_sapling=1 -ac_cbmaturity=6 -ac_cc=0 -addnode=146.190.69.236 -addnode=64.225.91.174
```

### Dependencies

```shell
#The following packages are needed:
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python python-zmq zlib1g-dev wget libcurl4-gnutls-dev bsdmainutils automake curl libsodium-dev
```

### Build Squishy

This software is based on Komodo and considered experimental and is continously undergoing heavy development.

#### Linux
```shell
git clone https://github.com/sqcndev/squishycoind.git
cd komodo
./zcutil/fetch-params.sh
./zcutil/build.sh -j$(expr $(nproc) - 1)
#This can take some time.
```


#### OSX
Ensure you have [brew](https://brew.sh) and Command Line Tools installed.
```shell
# Install brew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# Install Xcode, opens a pop-up window to install CLT without installing the entire Xcode package
xcode-select --install 
# Update brew and install dependencies
brew update
brew upgrade
brew tap discoteq/discoteq; brew install flock
brew install autoconf autogen automake
brew update && brew install gcc@8
brew install binutils
brew install protobuf
brew install coreutils
brew install wget
# Clone the Squishy repo
git clone https://github.com/sqcndev/squishycoind.git
# Change master branch to other branch you wish to compile
cd komodo
./zcutil/fetch-params.sh
./zcutil/build-mac.sh -j$(expr $(sysctl -n hw.ncpu) - 1)
# This can take some time.
```

#### Windows
Use a debian cross-compilation setup with mingw for windows and run:
```shell
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python python-zmq zlib1g-dev wget libcurl4-gnutls-dev bsdmainutils automake curl cmake mingw-w64 libsodium-dev libevent-dev
curl https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env
rustup target add x86_64-pc-windows-gnu

sudo update-alternatives --config x86_64-w64-mingw32-gcc
# (configure to use POSIX variant)
sudo update-alternatives --config x86_64-w64-mingw32-g++
# (configure to use POSIX variant)

git clone https://github.com/sqcndev/squishycoind.git
cd komodo
./zcutil/fetch-params.sh
./zcutil/build-win.sh -j$(expr $(nproc) - 1)
#This can take some time.
```
**squishy is experimental and a work-in-progress.** Use at your own risk.

To reset the Squishy blockchain change into the *~/.komodo/SQCN* data directory and delete the corresponding files by running `rm -rf blocks chainstate debug.log komodostate db.log`

#### Create SQCN.conf
```
mkdir ~/.komodo/SQCN
cd ~/.komodo/SQCN
touch SQCN.conf

#Add the following lines to the komodo.conf file:
rpcuser=yourrpcusername
rpcpassword=yoursecurerpcpassword
rpcbind=127.0.0.1
txindex=1
addnode=146.190.69.236
addnode=64.225.91.174

```

License
-------
For license information see the file [COPYING](COPYING).

**NOTE TO EXCHANGES:**
https://bitcointalk.org/index.php?topic=1605144.msg17732151#msg17732151
There is a small chance that an outbound transaction will give an error due to mismatched values in wallet calculations. There is a -exchange option that you can run komodod with, but make sure to have the entire transaction history under the same -exchange mode. Otherwise you will get wallet conflicts.

**To change modes:**

a) backup all privkeys (launch komodod with `-exportdir=<path>` and `dumpwallet`)  
b) start a totally new sync including `wallet.dat`, launch with same `exportdir`  
c) stop it before it gets too far and import all the privkeys from a) using `komodo-cli importwallet filename`  
d) resume sync till it gets to chaintip  

For example:
```shell
./komodod -ac_name=SQCN -exportdir=/tmp &
./komodo-cli -ac_name=SQCN dumpwallet example
./komodo-cli -ac_name=SQCN stop
mv ~/.komodo ~/.komodo.old && mkdir ~/.komodo && cp ~/.komodo.old/komodo.conf ~/.komodo.old/peers.dat ~/.komodo
./komodod -ac_name=SQCN -exchange -exportdir=/tmp &
./komodo-cli -ac_name=SQCN importwallet /tmp/example
```
---


Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
