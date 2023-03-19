安装源代码
========================

本节介绍如何从git@github直接获取的源代码进行安装。

预备条件
--------------
1. 需要GCC编译器（gcc、g++和gfortran）。
  大多数现代GCC版本（从3.4.6开始）应该可以工作。
  我们建议使用4.1及更高版本。
  支持Intel编译器（版本10和11）。仅Intel数学库
  就能提供显著的性能提升，原因是更有效的pow(x,y)函数。

2. 需要Flex（>=2.5.4a）和Bison（>=2.0）。

3. Petsc
        Petsc 3.1到3.6应该可以工作。建议使用petsc-3.5版本。
        最新的petsc-3.6可以工作，但尚未完全测试。

   编译PETSC需要设置两个系统变量：
   'PETSC_DIR'指向PETSC的目录。
   'PETSC_ARCH'给出PETSC配置的标签。

 对于MPI版本的petsc，MPICH2和OpenMPI都可以很好地工作。
 petsc的推荐配置参数是
./configure --download-mpich=1
--with-debugging=0 --with-shared=0 --with-x=0 --with-pic=1
--download-f-blas-lapack=1
--download-superlu=1 --download-superlu_dist=1
--download-blacs=1 --download-scalapack=1
--download-parmetis=1 --download-mumps=1
--COPTFLAGS="-O2" --CXXOPTFLAGS="-O2" --FOPTFLAGS="-O2"

对于串行版本的petsc
petsc的推荐配置参数是
./configure --with-mpi=0
--with-debugging=0 --with-shared=0 --with-x=0 --with-pic=1
--download-f-blas-lapack=1
--download-metis=1
--download-superlu=1
--COPTFLAGS="-O2" --CXXOPTFLAGS="-O2" --FOPTFLAGS="-O2"
  
  
注意：需要--download-parmetis=1或--download-metis=1。
Genius将使用（par）metis作为其网格划分器。

4. cgnslib-2.5。
默认情况下，头文件和库文件分别安装在
/usr/local/include和/usr/local/lib。

注意：cgns 3.0未经测试，可能无法使用。

5. VTK-5.4.x。
默认情况下，头文件和库文件分别安装在
/usr/local/include/vtk-5.4和/usr/local/lib/vtk-5.4。

注意：VTK 5.6和5.8也可以使用，其他版本未经测试。

6. 配置和构建Genius
./waf --prefix=$PWD --with-petsc-dir=$PETSC_DIR
--with-petsc-arch=$PETSC_ARCH configure build install

   本选项：
   --prefix                      Genius二进制文件的安装位置
   --with-petsc-dir=<dir>        Petsc安装的目录（PETSC_DIR）
   --with-petsc-arch=<arch>      PETSC_ARCH

如果CGNS和VTK安装在非标准目录中，需要以下选项：
   --with-cgns-dir=<dir>         CGNS安装的目录（将自动搜索/usr和
                                 /usr/local）。
   --with-vtk-dir=<dir>          VTK安装的目录（将自动搜索/usr和
                                 /usr/local）。
   --with-vtk-ver=<str>          VTK版本（vtk-5.4或vtk-5.6等）。

进一步的选项：
   --version=<str>               构建的版本字符串
   --with-git=<path/to/git>      git二进制文件
   --cc-opt=<compiler opt>       CC优化选项（如-O2 -unroll）。
                                 如果没有指定，我们将尝试
                                 检测编译器支持什么。
   --debug                       启用调试，禁用优化。

要获取完整的选项列表，请运行
./waf --help

7. 开始使用Genius
export GENIUS_DIR=<path/to/genius/>
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH/:$GENIUS_DIR/lib
cd examples/PN_Diode/2D
$GENIUS_DIR/bin/genius.LINUX -i pn2d.inp

8. 解决无法打开共享对象文件的问题
Genius依赖于一些第三方库，其中一些可能是共享对象文件（libxxx.so）。如果genius可执行代码报告，例如：
error while loading shared libraries: libpetsc.so.3.5: cannot open shared object file: No such file or directory
请设置环境变量LD_LIBRARY_PATH指向包含共享库文件libpetsc.so.3.5的目录，即petsc的库文件所在目录。
此外，genius可能会报告无法找到VTK的共享库，请将LD_LIBRARY_PATH指向包含VTK库文件的目录。

这是我的环境变量LD_LIBRARY_PATH
LD_LIBRARY_PATH=/usr/local/petsc/arch-linux2-c-opt/lib:/usr/local/slepc/arch-linux2-c-opt/lib:/usr/local/vtk-5.8/lib/vtk-5.8:/usr/local/mpich-3.1.4/lib:/home/gdiso/develop/genius/lib


  
