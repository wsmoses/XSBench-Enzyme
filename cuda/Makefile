#===============================================================================
# User Options
#===============================================================================

COMPILER    = nvidia
OPTIMIZE    = yes
DEBUG       = no
PROFILE     = no
SM_VERSION  = 60

#===============================================================================
# Program name & source code list
#===============================================================================

program = XSBench

source = \
Main.cu \
io.cu \
Simulation.cu \
GridInit.cu \
XSutils.cu \
Materials.cu

obj = $(source:.cu=.o)

#===============================================================================
# Sets Flags
#===============================================================================

# Standard Flags
CFLAGS := 

# Linker Flags
LDFLAGS = "-lcudart_static" "-ldl" "-lrt" -lpthread -lm

# Regular gcc Compiler
ifeq ($(COMPILER),nvidia)
  CC = /mnt/sabrent/wmoses/llvm13/buildallfast/bin/clang++
  # CFLAGS += -fno-experimental-new-pass-manager -O3 --cuda-path=/usr/local/cuda-11.0 -L/usr/local/cuda-11.0/lib64 --cuda-gpu-arch=sm_$(SM_VERSION)  -std=c++11 -Xclang -load -Xclang /home/wmoses/git/Enzyme/enzyme/build13D/Enzyme/ClangEnzyme-13.so 
  # CFLAGS += -fno-experimental-new-pass-manager -O3 --cuda-path=/usr/local/cuda-11.0 -L/usr/local/cuda-11.0/lib64 --cuda-gpu-arch=sm_$(SM_VERSION)  -std=c++11 -Xclang -load -Xclang /home/wmoses/git/Enzyme/enzyme/build13/Enzyme/ClangEnzyme-13.so 
  CFLAGS += -fno-experimental-new-pass-manager --cuda-path=/usr/local/cuda-11.0 -L/usr/local/cuda-11.0/lib64 --cuda-gpu-arch=sm_$(SM_VERSION)  -std=c++11 -Xclang -load -Xclang /home/wmoses/git/Enzyme/enzyme/build13Fast/Enzyme/ClangEnzyme-13.so 
endif

# Debug Flags
ifeq ($(DEBUG),yes)
  CFLAGS += -g -G
  LDFLAGS  += -g -G
endif

# Profiling Flags
ifeq ($(PROFILE),yes)
  CFLAGS += -pg
  LDFLAGS  += -pg
endif

# Optimization Flags
ifeq ($(OPTIMIZE),yes)
  CFLAGS += -O3
endif

#===============================================================================
# Targets to Build
#===============================================================================

$(program): $(obj) XSbench_header.cuh Makefile
	$(CC) $(CFLAGS) $(obj) -o $@ $(LDFLAGS)

%.o: %.cu XSbench_header.cuh Makefile
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -rf $(program) $(obj)

edit:
	vim -p $(source) XSbench_header.cuh

run:
	./$(program)
