#clone the repo

git clone https://github.com/radhikaj/protobuf_prototype
git submodule --init

#build protobuf including libraries for host and enclave

cd protobuf
mkdir build
export CXX=/usr/bin/clang++-7
export CC=/use/bin/clang
cmake -Dprotobuf_WITH_ZLIB=OFF -Dprotobuf_BUILD_TESTS=OFF  ../cmake
make

#install prootbuf

cmake -DCMAKE_INSTALL_PREFIX=/opt/protobuf ../cmake
sudo make install

#start using the newly installed protobuf

export PKG_CONFIG_PATH=/opt/protobuf/lib/pkgconfig:${PKG_CONFIG_PATH}
export PATH=/opt/protobuf/bin:${PATH}

#change to the home directory of cloned protobuf_prototype and make the sample

make


