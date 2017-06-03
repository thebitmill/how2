#

## Generate keys

```
# Generate the private and public keys
> openssl ecparam -name secp256k1 -genkey -noout | 
  openssl ec -text -noout > Key

# Extract the public key and remove the EC prefix 0x04
> cat Key | grep pub -A 5 | tail -n +2 |
            tr -d '\n[:space:]:' | sed 's/^04//' > pub

# Extract the private key and remove the leading zero byte
> cat Key | grep priv -A 3 | tail -n +2 | 
            tr -d '\n[:space:]:' | sed 's/^00//' > priv

# Generate the hash and take the address part
> cat pub | keccak-256sum -x -l | tr -d ' -' | tail -c 41 > address

# (Optional) import the private key to geth
> geth account import priv
```

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
