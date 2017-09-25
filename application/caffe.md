Caffe
===

# Docker

[official guide](https://github.com/BVLC/caffe/tree/master/docker)


```
docker run -ti bvlc/caffe:cpu caffe --version
```

```
docker run -ti bvlc/caffe:cpu ipython
import caffe
...
```

# Ubuntu

sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install libopenblas-dev 
