## Set up gRPC
1. Sync gRPC from: `https://github.com/carryorange/3p`
	- It references gRPC as a submodule. Update the submodule if necessary
2. Build and install gRPC and Protocol Buffers
  ```bash
    $ # Set the install directory $MY_INSTALL_DIR, files will be installed under $MY_INSTALL_DIR/bin
	$ # Navigate to grpc source root folder
	$ mkdir -p cmake/build
	$ pushd cmake/build
	$ cmake -DgRPC_INSTALL=ON \
      -DgRPC_BUILD_TESTS=OFF \
      -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR \
      ../..
	$ make -j
	$ make install
	$ popd
    $ # Add $MY_INSTALL_DIR/bin to PATH
  ```
    Tips: Install directory can be set to path to `3p/install` repo
3. Install Abseil C++ library
  ```bash
    $ # From grpc source root folder
    $ mkdir -p third_party/abseil-cpp/cmake/build
    $ pushd third_party/abseil-cpp/cmake/build
    $ cmake -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR \
          -DCMAKE_POSITION_INDEPENDENT_CODE=TRUE \
          ../..
    $ make -j
    $ make install
    $ popd
  ```