20180507

macOS binary distributions of Bazel and TensorFlow assume CPU instructions that are not available in the old Core 2-era processors in the Mac Pro 1,1 and 2,1. To use TensorFlow natively in this environment, you're going to have to build it yourself.

* Step 0: Already have Homebrew on your system

* Step 1: Install Python 3 from Homebrew
brew install python@3

* Step 2: Build Bazel, then put it somewhere sensical
https://docs.bazel.build/versions/master/install-compile-source.html

* Step 3: Build TensorFlow
https://www.tensorflow.org/install/install_sources

* Step 4: Install the package, **specifying the correct version of grpcio.**

`$ pip3 install grpcio==1.9.1 /tmp/tensorflow_pkg/tensorflow-1.8.0-cp36-cp36m-macosx_10_10_x86_64.whl`

This WorksForMe™ as of date of publishing

```Kallistei:~ theodric$ sysctl hw.model
hw.model: MacPro2,1
Kallistei:~ theodric$ uname -a
Darwin Kallistei.grex 14.5.0 Darwin Kernel Version 14.5.0: Sun Jun  4 21:40:08 PDT 2017; root:xnu-2782.70.3~1/RELEASE_X86_64 x86_64 i386 MacPro2,1 Darwin
Kallistei:~ theodric$ ls -lah `which python3`
lrwxr-xr-x 1 theodric staff 34 May  7 13:47 /usr/local/bin/python3 -> ../Cellar/python/3.6.5/bin/python3
Kallistei:~ theodric$ python3
Python 3.6.5 (default, Apr 14 2018, 20:48:31)
[GCC 4.2.1 Compatible Apple LLVM 7.0.2 (clang-700.1.81)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
b'Hello, TensorFlow!'
>>>
```
