#

## Arch Linux

```
# pacman -S boost leveldb crypto++ miniupnpc opencl-headers
$ yaourt -S libjson-rpc-cpp
$ git clone https://github.com/bobsummerwill/cpp-ethereum
$ git checkout merge_repos
$ mkdir build && cd build
$ cmake -DBUNDLE=miner ..
$ make -j8 ethminer
```

## Ubuntu 16.04

NOTE: libjsonrpccpp-dev, not libjson-rpc-cpp-dev

```
# apt-get update
# apt-get -y install software-properties-common
# add-apt-repository -y ppa:ethereum/ethereum
# apt-get update
# apt-get install git cmake libcryptopp-dev libleveldb-dev libjsoncpp-dev libjsonrpccpp-dev libboost-all-dev libgmp-dev libreadline-dev libcurl4-gnutls-dev ocl-icd-libopencl1 opencl-headers mesa-common-dev libmicrohttpd-dev build-essential -y
$ git clone https://github.com/Genoil/cpp-ethereum/
$ cd cpp-ethereum/
$ mkdir build
$ cd build
$ cmake -DBUNDLE=miner ..
$ make -j8 ethminer
```
