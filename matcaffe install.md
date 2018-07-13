# 1. Edit Makefile.config
<Set MATLAB path to build matcaffe>
This is required only if you will compile the matlab interface.
MATLAB directory should contain the mex binary in /bin.
MATLAB_DIR := /usr/local
MATLAB_DIR := /usr/local/MATLAB/R2015b/bin/

# 2.Build caffe and matcaffe
<build caffe>
cd caffe
make all
make all test
make runtest
make all matcaffe

# 2-1 build issue
<HDF5 issues: src/caffe/net.cpp:8:18: fatal error: hdf5.h: No such file or directory>
compilation terminated.
Makefile:581: recipe for target '.build_release/src/caffe/net.o' failed
make: *** [.build_release/src/caffe/net.o] Error 1


<Solve>
install libhdf5-dev
open Makefile.config, locate line containing LIBRARY_DIRS and append /usr/lib/x86_64-linux-gnu/hdf5/serial
locate INCLUDE_DIRS and append /usr/include/hdf5/serial/ (per this SO answer)
rerun make all

# 2-2 '/home/xw/caffeBuild/caffe-master/matlab/+caffe/private/caffe_.mexa64':

/home/xw/caffeBuild/caffe-master/matlab/+caffe/private/caffe_.mexa64: undefined
symbol:
_ZN2cv8imencodeERKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERKNS_11_InputArrayERSt6vectorIhSaIhEERKSB_IiSaIiEE

<solve>
root@test222:/matlab/r2016a/bin/glnxa64# mv libopencv_imgproc.so.2.4 libopencv_imgproc.so.2.4.bak
root@test222:/matlab/r2016a/bin/glnxa64# mv libopencv_highgui.so.2.4 libopencv_highgui.so.2.4.bak
root@test222:/matlab/r2016a/bin/glnxa64# mv libopencv_core.so.2.4 libopencv_core.so.2.4.bak


root@test222:/matlab/r2016a/bin/glnxa64# ln /usr/lib/x86_64-linux-gnu/libopencv_core.so.2.4.9 libopencv_core.so.2.4
root@test222:/matlab/r2016a/bin/glnxa64# ln /usr/lib/x86_64-linux-gnu/libopencv_highgui.so.2.4.9 libopencv_highgui.so.2.4
root@test222:/matlab/r2016a/bin/glnxa64# ln /usr/lib/x86_64-linux-gnu/libopencv_imgproc.so.2.4.9 libopencv_imgproc.so.2.4

# reference 1
https://github.com/BVLC/caffe/issues/3934

# 3. How to use matcaffe
root@49648ad1361b:/opt/caffe# LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.21 /usr/local/MATLAB/R2015b/bin/matlab -desktop

# reference 2
https://tech.d-itlab.co.jp/programming/790/
