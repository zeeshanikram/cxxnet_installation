CXXNET Installation steps for Amazon EC2 GPU instances
===================
Launch a g2.xlarge instance with Ubuntu 14.04 LTS

```bash
First enable multiverse repositories
sudo vim /etc/apt/sources.list
Uncomment the lines ending with multiverse e.g.
deb http://us.archive.ubuntu.com/ubuntu/ oneiric multiverse
deb http://us.archive.ubuntu.com/ubuntu/ oneiric-updates multiverse

Perform apt-get update
sudo apt-get update

Install kernel headers

sudo apt-get install linux-headers-$(uname -r)
sudo apt-get install linux-headers-generic

Cuda installation
Downloaad nvidia cuda installation file (for Ubuntu it is in .deb format)
wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_6.5-14_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1404_6.5-14_amd64.deb
sudo apt-get update
sudo apt-get install cuda-toolkit-6-5 cuda-runtime-6-5 libcuda1-340
Cblas installation 
sudo apt-get install cblas
sudo apt-get install libblas3 libblas-dev
sudo apt-get install cuda-cublas-dev-6-5
sudo apt-get install libcublas5.5
Install dependencies
sudo apt-get install zlib1g-dev
sudo apt-get install libopencv-dev
sudo apt-get install cuda-core-6-5

Set Export Paths and create Symlinks
export CPLUS_INCLUDE_PATH=/usr/local/cuda-6.5/targets/x86_64-linux/include/
export PATH=$PATH:/usr/local/cuda-6.5/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH :/usr/local/cuda-6.5/targets/x86_64-linux/lib/
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib/
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64/

sudo ln -s /usr/local/cuda/lib64/libcublas.so /usr/lib/libcublas.so
sudo ln -s /usr/local/cuda/lib64/libcudart.so /usr/lib/libcudart.so
sudo ln -s /usr/local/cuda/lib64/libcurand.so /usr/lib/libcurand.so
sudo ln /dev/null /dev/raw1394

Install Intel’s Math Kernel Library
wget ftp://78.107.232.180/l_mkl_11.1.2.144.tgz
tar xvf l_mkl_11.1.2.144.tgz
cd l_mkl_11.1.2.144/
sudo ./install.sh

Update include paths
Find intel mkl include path, installation path of mkl library and add it to CPLUS_INCLUDE_PATH
export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH :~/intel/mkl/include/

CXXNET Installation 
Install git
apt-get install git
sudo apt-get install git
Download/clone cxxnet git repo
git clone https://github.com/antinucleon/cxxnet.git
cd cxxnet/
Build library
./build.sh blas=1
Build tools
cd tools
make

