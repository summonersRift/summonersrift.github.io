---
layout: post
title:  "SST macro installation"
date:   2017-03-03
---

<p class="intro"><span class="dropcap">C</span>uarabitur blandit tempus porttitor. Nullam quis risus eget urna mollis ornare vel eu leo. Vestibulum id ligula porta felis euismod semper. Donec sed odio dui. Aenean lacinia bibendum nulla sed consectetur.</p>




#### What is sst-macro?
A HPC simulator for interconnection and application performance prediction.

#### Goal of this post:
 Install sst-macro with sst-core .

#### References and helpful links:
*  http://sst-simulator.org/SSTPages/SSTBuildAndInstall6dot1dot0SeriesDetailedBuildInstructions/
*  https://github.com/sstsimulator/sst-macro/blob/v6.1.0%5Fbeta/docs/manual/manual.md
    and sst-elements modeling support for detailed simulation.




#### My machine Spec: 
* UBUNTU 14.04, x86%5F64

#### Dependencies
1. gcc, g++ 4.9, 5.+
2. openmpi 1.8.8 (Make from source)
    * Note: mpich from apt-get didnt work for me
3. boost 1.59
4. sst-core 6.0.1
5. sst-elements 


<blockquote>SST/macro installation instructions:- </blockquote>


#### Installing gcc 4.9
From : http://askubuntu.com/questions/466651/how-do-i-use-the-latest-gcc-on-ubuntu


sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-4.9 g++-4.9
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.9


<h3>~/.bashrc OR whatever your environment file is</h3>


{% highlight bash %}


export MPICC=mpicc
export MPICXX=mpicxx

export MPIHOME=/usr/local/packages/OpenMPI-1.8.8
export BOOST_HOME=/usr/local/packages/boost-1.59
export SST_CORE_HOME=/local/sst/sstcore
export SST_ELEMENTS_HOME=/local/sst/sstelements
export SST_MACRO_HOME=/local/sst/sstmacro

export PATH=$MPIHOME/bin:$SST_CORE_HOME/bin:$SST_ELEMENTS_HOME/bin:$SST_MACRO_HOME/bin:$PATH
export LD_LIBRARY_PATH=$BOOST_HOME/lib:$SST_MACRO_HOME/lib:$MPIHOME/lib:$LD_LIBRARY_PATH


{% endhighlight %}


Now you source <i>.bashrc</i>:


<h3> Installing Dependency: (1) openmpi</h3>
sudo su -
cd /local/scratch/src
scp obaida@prime-server.cs.fiu.edu:~/hpc/sst/*.tar.gz .
scp obaida@prime-server.cs.fiu.edu:~/hpc/sst/*.tar.bz2 .
tar -zxf openmpi-1.8.8.tar.gz
cd openmpi-1.8.8
./configure --prefix=$MPIHOME
make all install

<h3>Installing Dependency: (2) boost</h3>
cd /local/scratch/src/boost_1_59_0
./bootstrap.sh --prefix=$BOOST_HOME
./b2 install
#...updated 12357 targets...; NO ERROR? WOW


<h3>Installing Dependency: (3) sst-core & (4)sst-elements</h3>
cd /local/scratch/src/sstcore-6.1.0
./configure --prefix=$SST_CORE_HOME --with-boost=$BOOST_HOME
make all
make install

@installing sst-elements
cd /local/scratch/src/sst-elements-library-6.1.0
./configure --prefix=$SST_ELEMENTS_HOME --with-sst-core=$SST_CORE_HOME
make all
make install

<h3> Test sst installation</h3>
sst --version
#Should see the sst version 6.1.0
sst /local/scratch/src/sst-elements-library-6.1.0/src/sst/elements/simpleElementExample/tests/test%5FsimpleRNGComponent%5Fmersenne.py




<h3>sst-macro installation</h3>

cd /local/scratch/src
git clone https://github.com/sstsimulator/sst-macro.git
cd sst-macro
./bootstrap.sh
mkdir build 
cd build
../configure --prefix=$SST_MACRO_HOME --with-sst-core=$SST_CORE_HOME --disable-regex CC=$MPICC CXX=$MPICXX
make
make install

Ubuntu Bug [as mentioned in the documentation] 
 LDFLAGS="-Wl,-no-as-needed" 
Doesnt work[BUG]:
--with-sst-core=$SST_CORE_HOME




