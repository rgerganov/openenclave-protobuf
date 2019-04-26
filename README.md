# Build and use protobuf with OpenEnclave

## Build or install OpenEnclave
See [openenclave repo](https://github.com/Microsoft/openenclave) 

## Clone the repo

```bash
git clone https://github.com/openenclave/openenclave-protobuf
git submodule --init
```
### Apply the patch that enables building libprotobuf_oe_enclave.lib which uses OE headers and compiler flags
```bash
cd protobuf
git am ../0001-Add-ability-to-build-libprotobuf_oe_enclave.patch
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

## Build the sample
See sample's [README](sample/README.md).


## Contributing

This project welcomes contributions and suggestions. Most contributions require you to
agree to a Contributor License Agreement (CLA) declaring that you have the right to,
and actually do, grant us the rights to use your contribution. For details, visit
https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need
to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the
instructions provided by the bot. You will only need to do this once across all
repositories using our CLA in the Open Enclave GitHub organization.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/)
or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
