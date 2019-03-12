# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.

# Detect C and C++ compiler options
# if not gcc and g++, default to clang-7
C_COMPILER=$(notdir $(CC))
ifeq ($(C_COMPILER), gcc)
        CXX_COMPILER=$(notdir $(CXX))
        USE_GCC = true
endif

ifeq ($(USE_GCC),)
        CC = clang-7
        CXX = clang++-7
        C_COMPILER=clang
        CXX_COMPILER=clang++
endif

CFLAGS=$(shell pkg-config oeenclave-$(C_COMPILER) --cflags)
CXXFLAGS=$(shell pkg-config oeenclave-$(CXX_COMPILER) --cflags)
LDFLAGS=$(shell pkg-config oeenclave-$(CXX_COMPILER) --libs)

all:
	$(MAKE) build
	$(MAKE) keys
	$(MAKE) sign

build:
	@ echo "Compilers used: $(CC), $(CXX)"
	oeedger8r ../helloworld.edl --trusted
	/opt/protobuf-nonenc/bin/protoc -I../ ../hello.proto --cpp_out=.
	$(CXX) -c $(CXXFLAGS) -g -std=c++11 enc.cpp -fPIC -I/opt/protobuf-nonenc/include -nostdinc++ -I /opt/protobuf-nonenc/include 
	$(CC) -c $(CFLAGS)  -g helloworld_t.c -o helloworld_t.o 
	$(CXX) -c $(CXXFLAGS) -g -std=c++11 hello.pb.cc -I/opt/protobuf-nonenc/include -fPIC -pthread -I/opt/protobuf-nonenc/include
	$(CXX) -o helloworldenc.so enc.o helloworld_t.o hello.pb.o  -L/opt/protobuf-enc/lib/ /opt/protobuf-enc/lib/libprotobuf.a  $(LDFLAGS)



sign:
	oesign sign helloworldenc.so helloworld.conf private.pem

clean:
	rm -f *.so *.o fileencryptor_t.* fileencryptor_args.h private.pem public.pem

keys:
	openssl genrsa -out private.pem -3 3072
	openssl rsa -in private.pem -pubout -out public.pem


