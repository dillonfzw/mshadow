# set LD_LIBRARY_PATH
export CC  = gcc
export CXX = g++
export NVCC = clang++
include config.mk
include ../make/mshadow.mk
export CFLAGS = -Wall -O3 -std=c++11 -I../ $(MSHADOW_CFLAGS)
export LDFLAGS= -lm $(MSHADOW_LDFLAGS)
export NVCCFLAGS = -O3 -I../ -std=c++11 -DMSHADOW_FORCE_STREAM=0 -DMSHADOW_USE_MKL=0 $(MSHADOW_NVCCFLAGS)

# specify tensor path
BIN = basic defop
OBJ =
CUOBJ =
CUBIN = basic_stream
.PHONY: clean all

all: $(BIN) $(OBJ) $(CUBIN) $(CUOBJ)

basic: basic.cpp
defop: defop.cpp
basic_stream: basic_stream.cu

$(BIN) :
	$(CXX) $(CFLAGS) -o $@ $(filter %.cpp %.o %.c, $^)  $(LDFLAGS)

$(OBJ) :
	$(CXX) -c $(CFLAGS) -o $@ $(firstword $(filter %.cpp %.c, $^) )

$(CUOBJ) :
	$(NVCC) -c -o $@ $(NVCCFLAGS) $(filter %.cu, $^)

$(CUBIN) :
	$(NVCC) -o $@ $(NVCCFLAGS) $(LDFLAGS) $(filter %.cu %.cpp %.o, $^)

clean:
	$(RM) $(OBJ) $(BIN) $(CUBIN) $(CUOBJ) *~
