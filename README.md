# Build and use protobuf with open enclave

## Clone the repo

```bash
git clone https://github.com/radhikaj/protobuf_prototype

git submodule --init
```

## Build protobuf including libraries for host and enclave

```bash
cd protobuf
mkdir build
export CXX=/usr/bin/clang++-7
export CC=/use/bin/clang
cmake -Dprotobuf_WITH_ZLIB=OFF -Dprotobuf_BUILD_TESTS=OFF  ../cmake
make
```

## Install protobuf

```bash
cmake -DCMAKE_INSTALL_PREFIX=/opt/protobuf ../cmake
sudo make install
```

## Start using the newly installed protobuf

```bash
export PKG_CONFIG_PATH=/opt/protobuf/lib/pkgconfig:${PKG_CONFIG_PATH}
export PATH=/opt/protobuf/bin:${PATH}
```

## Change to the directory of cloned protobuf_prototype and make the sample
```bash
make
```


