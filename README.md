# Linux 编译Tensorflow C++ 动态库（aarch64）

## 环境

- gcc 7.4
- tensorflow 1.15.0
- bazel 0.26.1
- python 3.7
- ldd (Debian GLIBC 2.28.21-1+deepin-1) 2.28



## 编译安装bazel 0.26.1

安装jdk：`sudo apt-get install openjdk-8-jdk`

```
java --version
javac --version
```

源码地址：下载bazel-0.26.1-dist.zip

https://github.com/bazelbuild/bazel/releases?page=16

```
mkdir bazel && unzip bazel-0.26.1-dist.zip -d bazel
env EXTRA_BAZEL_ARGS=“–host_javabase=@local_jdk//:jdk” bash ./compile.sh
```

参考：https://www.johngo689.com/58862/

compile.sh执行完后，将output添加到环境变量，让系统能找到二进制文件bazel



## 编译tensorflow源码

cd tensorflow-1.15.0

执行`./configure`

```
xxx@xxx-PC:/data/home/xxx/proj/tensorflow/tensorflow-1.15.0$ ./configure 
WARNING: --batch mode is deprecated. Please instead explicitly shut down your Bazel server using the command "bazel shutdown".
You have bazel 0.26.1- (@non-git) installed.
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3


Found possible Python library paths:
  /usr/local/python3/lib/python3.7/site-packages
Please input the desired Python library path to use.  Default is [/usr/local/python3/lib/python3.7/site-packages]

Do you wish to build TensorFlow with XLA JIT support? [Y/n]: n
No XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: n
No OpenCL SYCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with ROCm support? [y/N]: n
No ROCm support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: n
No CUDA support will be enabled for TensorFlow.

Do you wish to download a fresh release of clang? (Experimental) [y/N]: n
Clang will not be downloaded.

Do you wish to build TensorFlow with MPI support? [y/N]: n
No MPI support will be enabled for TensorFlow.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native -Wno-sign-compare]: 


Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: n
Not configuring the WORKSPACE for Android builds.

Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See .bazelrc for more details.
        --config=mkl            # Build with MKL support.
        --config=monolithic     # Config for mostly static monolithic build.
        --config=gdr            # Build with GDR support.
        --config=verbs          # Build with libverbs support.
        --config=ngraph         # Build with Intel nGraph support.
        --config=dynamic_kernels        # (Experimental) Build kernels into separate shared objects.
Preconfigured Bazel build configs to DISABLE default on features:
        --config=noaws          # Disable AWS S3 filesystem support.
        --config=nogcp          # Disable GCP support.
        --config=nohdfs         # Disable HDFS support.
        --config=noignite       # Disable Apacha Ignite support.
        --config=nokafka        # Disable Apache Kafka support.
        --config=nonccl         # Disable NVIDIA NCCL support.
Configuration finished
```

执行`bazel build --config=opt //tensorflow:libtensorflow_cc.so`



bug参考：

gcc版本问题

https://github.com/tensorflow/tensorflow/issues/25323

bazel构建文件报错

https://blog.csdn.net/CSDN_Doubao/article/details/114435146

https://blog.csdn.net/zqwwwm/article/details/121924210

内存小卡死：

https://www.cnblogs.com/GengMingYan/p/15963832.html





在/tensorflow-1.13.1/tensorflow/contrib/makefile下执行` ./build_all_linux.sh`

build_all_linux.sh调用download_dependencies.sh，其中如果下载eigen报错，需要修改download_dependencies.sh中eigen路径，改为镜像路径即可






