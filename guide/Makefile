# set LD_LIBRARY_PATH
export CC  = gcc
export CXX = g++
# nv
export NVCC = nvcc
# corex
#export NVCC = clang++
include config.mk
include ../make/mshadow.mk
export CFLAGS = -Wall -O3 -std=c++11 -I../ $(MSHADOW_CFLAGS)
export LDFLAGS= -lm $(MSHADOW_LDFLAGS)
# nv
export NVCCFLAGS = -O3 --use_fast_math -ccbin $(CXX) -DMSHADOW_FORCE_STREAM=0 $(MSHADOW_NVCCFLAGS)
# corex
#export NVCCFLAGS = -O3 -I../ -std=c++11 -DMSHADOW_FORCE_STREAM=0 -DMSHADOW_USE_MKL=0 $(MSHADOW_NVCCFLAGS)

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
	# nv
	$(NVCC) -c -o $@ $(NVCCFLAGS) -Xcompiler "$(CFLAGS)" $(filter %.cu, $^)
	# corex
	#$(NVCC) -c -o $@ $(NVCCFLAGS) $(filter %.cu, $^)

$(CUBIN) :
	# nv
	$(NVCC) -o $@ $(NVCCFLAGS) -Xcompiler "$(CFLAGS)" -Xlinker "$(LDFLAGS)" $(filter %.cu %.cpp %.o, $^)
	# corex
	#$(NVCC) -o $@ $(NVCCFLAGS) $(LDFLAGS) $(filter %.cu %.cpp %.o, $^)

clean:
	$(RM) $(OBJ) $(BIN) $(CUBIN) $(CUOBJ) *~
