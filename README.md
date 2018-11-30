# OpenMVS: open Multi-View Stereo reconstruction library

## Introduction

[OpenMVS (Multi-View Stereo)](http://cdcseacave.github.io/openMVS) is a library for computer-vision scientists and especially targeted to the Multi-View Stereo reconstruction community. 
While there are mature and complete open-source projects targeting Structure-from-Motion pipelines (like [OpenMVG](https://github.com/openMVG/openMVG)) which recover camera poses and a sparse 3D point-cloud from an input set of images, there are none addressing the last part of the photogrammetry chain-flow. *OpenMVS* aims at filling that gap by providing a complete set of algorithms to recover the full surface of the scene to be reconstructed. The input is a set of camera poses plus the sparse point-cloud and the output is a textured mesh. The main topics covered by this project are:

- **dense point-cloud reconstruction** for obtaining a complete and accurate as possible point-cloud
- **mesh reconstruction** for estimating a mesh surface that explains the best the input point-cloud
- **mesh refinement** for recovering all fine details
- **mesh texturing** for computing a sharp and accurate texture to color the mesh

See the complete [documentation](https://github.com/cdcseacave/openMVS/wiki) on wiki.

## This armhf specific Patch was requested by cdcseacave

This is a unstable back port designed for testing armhf with Raspbian Stretch Debian on the Raspberry pi3B+.

Demo data used to benchmark: https://github.com/openMVG/ImageDataset_SceauxCastle.git
Took about 80 minutes to process into 3D with the build flags below...

Note: Optional 'Refine the dense mesh' does not currently work on Pi3B+, as it can't handle the memory demand without thrashing.

The entire demo pipeline will be included in our club OS rc1 on Dec 28, 2018:
https://sourceforge.net/projects/micrometer-cnc-on-raspberry-pi

## Build

git clone https://github.com/cdcseacave/VCG.git /opt/photogrammetry/vcglib 
git clone https://github.com/cdcseacave/openMVS.git 
cd openMVS
mkdir build 
cd build

cmake .. \
-DCMAKE_BUILD_TYPE=Release \
-DVCG_ROOT=".../vcglib" \
-DOpenMVS_USE_CUDA=OFF \
-DBoost_USE_STATIC_LIBS=ON \
-DBUILD_SHARED_LIBS=OFF \
-DOpenMVS_USE_SSE=OFF  \
-DOpenMVS_USE_FAST_CBRT=OFF \
-DOpenMVS_USE_FAST_FLOAT2INT=OFF \
-DCERES_DIR_HINTS=.../ceres  -DCERES_INCLUDE_DIR=.../ceres \
-DCGAL_DIR=.../lib/cmake/CGAL  -DCGAL_INCLUDE_DIR=.../include/CGAL \
-DOpenMVG_DIR=.../share/openMVG

make 
sudo make install

