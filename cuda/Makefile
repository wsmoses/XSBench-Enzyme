#===============================================================================
# User Options
#===============================================================================

COMPILER    = nvidia

OPTIMIZE    ?= yes
FORWARD     ?= no
INLINE      ?= yes
NEWCACHE    ?= no
AA          ?= yes
PHISTRUCT   ?= yes
TEMPLATIZE  ?= no

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
  # CFLAGS += -Rpass=enzyme -mllvm -enzyme-print -fno-experimental-new-pass-manager --cuda-path=/usr/local/cuda-11.2 -L/usr/local/cuda-11.2/lib64 --cuda-gpu-arch=sm_$(SM_VERSION)  -std=c++11 -Xclang -load -Xclang /home/wmoses/git/Enzyme/enzyme/build13Fast/Enzyme/ClangEnzyme-13.so 
  CFLAGS += -Rpass=enzyme -mllvm -enzyme-print -fno-experimental-new-pass-manager --cuda-path=/usr/local/cuda-11.2 -L/usr/local/cuda-11.2/lib64 --cuda-gpu-arch=sm_$(SM_VERSION)  -std=c++11 -Xclang -load -Xclang /home/wmoses/git/Enzyme/enzyme/build13Fast/Enzyme/ClangEnzyme-13.so 
  # CFLAGS += -mllvm -enzyme-inactive-dynamic=0 -Rpass=enzyme -mllvm -enzyme-print -fno-experimental-new-pass-manager --cuda-path=/usr/local/cuda-11.2 -L/usr/local/cuda-11.2/lib64 --cuda-gpu-arch=sm_$(SM_VERSION)  -std=c++11 -Xclang -load -Xclang /home/wmoses/git/Enzyme/enzyme/build13Fast/Enzyme/ClangEnzyme-13.so 

  # CC = /mnt/sabrent/wmoses/llvm13/buildall/bin/clang++
  # CFLAGS += -Rpass=enzyme -mllvm -enzyme-print -fno-experimental-new-pass-manager --cuda-path=/usr/local/cuda-11.2 -L/usr/local/cuda-11.2/lib64 --cuda-gpu-arch=sm_$(SM_VERSION)  -std=c++11 -Xclang -load -Xclang /home/wmoses/git/Enzyme/enzyme/build13D/Enzyme/ClangEnzyme-13.so -mllvm -enzyme-new-cache=0
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

# Optimization Flags
ifeq ($(FORWARD),yes)
  CFLAGS += -DFORWARD_PASS=1
endif

# Optimization Flags
ifeq ($(INLINE),yes)
  CFLAGS += -DALWAYS_INLINE=1
endif

# Optimization Flags
ifeq ($(NEWCACHE),yes)
  CFLAGS += -mllvm -enzyme-new-cache=1
else
  CFLAGS += -mllvm -enzyme-new-cache=0
endif

ifeq ($(AA),yes)
  CFLAGS += -mllvm -enzyme-aggressive-aa=1
else
  CFLAGS += -mllvm -enzyme-aggressive-aa=0
endif


ifeq ($(PHISTRUCT),yes)
  CFLAGS += -mllvm -enzyme-phi-restructure=1
else
  CFLAGS += -mllvm -enzyme-phi-restructure=0
endif


ifeq ($(TEMPLATIZE),yes)
  CFLAGS += -DTEMPLATIZE=1
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
