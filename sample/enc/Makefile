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

CFLAGS=$(shell pkg-config oeenclave-$(C_COMPILER) --cflags) $(shell pkg-config protobuf --cflags)
CXXFLAGS=$(shell pkg-config oeenclave-$(CXX_COMPILER) --cflags) $(shell pkg-config protobuf --cflags)
LDFLAGS= $(shell pkg-config protobuf_oe_enclave --libs) $(shell pkg-config oeenclave-$(CXX_COMPILER) --libs)

all:
	$(MAKE) build
	$(MAKE) keys
	$(MAKE) sign

build:
	@ echo "Compilers used: $(CC), $(CXX)"
	oeedger8r ../helloworld.edl --trusted
	protoc -I../ ../hello.proto --cpp_out=.
	$(CXX) -c $(CXXFLAGS) -g -std=c++11 enc.cpp 
	$(CC) -c $(CFLAGS)  -g helloworld_t.c -o helloworld_t.o 
	$(CXX) -c $(CXXFLAGS) -g -std=c++11 hello.pb.cc 
	$(CXX) -o helloworldenc.so enc.o helloworld_t.o hello.pb.o $(LDFLAGS)



sign:
	oesign sign -e helloworldenc.so -c helloworld.conf -k private.pem

clean:
	rm -f *.so *.o *.so.signed helloworld_t.* helloworld_args.h hello.pb.cc hello.pb.h private.pem public.pem

keys:
	openssl genrsa -out private.pem -3 3072
	openssl rsa -in private.pem -pubout -out public.pem


