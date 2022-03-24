# Run cutlass with gpgpu-sim

1. Download and install coda 9.1
	
	      wget https://developer.nvidia.com/compute/cuda/9.1/Prod/local_installers/cuda_9.1.85_387.26_linux
      
          sudo sh cuda_9.1.85_387.26_linux

	Set up the bashrc file, adding lines to the file
	
	       export PATH=/usr/local/cuda-9.1/bin:$PATH
	       export CUDA_INSTALL_PATH=/usr/local/cuda-9.1
	       export LD_LIBRARY_PATH=$CUDA_INSTALL_PATH/lib64:$LD_LIBRARY_PATH 

2. Download the gpgpusim 4.0
	
	       sudo apt-get install build-essential xutils-dev bison zlib1g-dev flex libglu1-mesa-dev

	       sudo apt-get install doxygen graphviz
	
	       sudo apt-get install python-pmw python-ply python-numpy libpng-dev python-matplotlib

	       sudo apt-get install libxi-dev libxmu-dev freeglut3-dev

	       git clone https://github.com/gpgpu-sim/gpgpu-sim_distribution.git
    
         Make and build GPGPUSIM:
	
	       make clean

	       source setup_environment

	       make


3. Download the gpgpusim version cutlass


	       git clone https://github.com/accel-sim/gpu-app-collection.git


4. Run and build the cutlass benchmark

	       cd gpu-app-collection/src/cuda/cutlass-bench/
	
	       git submodule update --init —recursive

	       mkdir build && cd build

	       cmake .. -DUSE_GPGPUSIM=1 -DCUTLASS_NVCC_ARCHS=70 && make cutlass_perf_test

	       cd tools/test/perf && ln -s ../../../../binary.sh . && ./binary.sh

	(Then copy the configs, i.e. 
	cp -a ~/gpgpu-sim_distribution/configs/tested-cfgs/SM7_TITANV/* . )

	       ./cutlass_perf_test --m=1760 --n=16 --k=1760 --kernels=wmma_gemm_nn --iterations=1 —providers=cutlass


	Reference and more information at:
	
	https://github.com/accel-sim/gpu-app-collection/tree/release/src/cuda/cutlass-bench
