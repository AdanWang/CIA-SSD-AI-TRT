# CIA-SDD-AI-TRT

CIA-SSD-AI-TRT(**CIA-SSD ALL IN TensorRT**,NMS not implemented in TensorRT,implemented in c++) 

CIA-SSD consists of five parts:
- preprocess: generate voxel, it is implemented in voxelGenerator.cu,it is a TensorRT plugin
- 3D backbone: 3D backbone include 3D sparse Convolution and 3D Submanifold Convolution. sparseConv3dlayer.cu is a TensorRT plugin for 3D sparse conv, and submConv3dlayer.cu is a TensorRT plugin for 3D subm conv.
- neck: this part is mainy implemented by TensorRT aip, because they are all general modules. the function of sparse2Dense.cu is  from sparse tensor to dense tensor
- head: this part is mainy implemented by TensorRT aip.
- postprocess: it includes anchorGenerate and decoder, they are implemented by generateAnchorDecode.cu, it is also a plugin.
- 3D NMS: it comes from  https://github.com/NVIDIA-AI-IOT/CUDA-PointPillars/blob/main/src/postprocess.cpp

## Config

- all config in params.h
- FP16/FP32 can be selected by USE_FP16
- GPU id can be selected by DEVICE
- NMS thresh in params.h

## How to Run

1. build CIA-SSD-AI-TRT and run

```
cd CIA-SSD-AI-TRT
mkdir build
cd build
cmake ..
make
sudo ./cia-ssd-ai-trt -s             // serialize model to plan file i.e. 'cia-ssd-ai-trt.engine'
sudo ./cia-ssd-ai-trt -d    // deserialize plan file and run inference, lidar points will be processed.

```
**one frame takes 1-2 seconds on my laptop with Intel(R) Core(TM) i5-7300HQ and nvidia 1050ti, it is very slow, needs to be optimized in  the future.**

2. show predicted 3D boxes in the lidar frame 

```
fristly install python moudles by tools/requirements.txt
cd tools
python show_box_in_points.py, type C, you can show lidar points and boxes one by one,

```
![Image text](https://raw.githubusercontent.com/jingyue202205/CIA-SSD-AI-TRT/master/pics/000010.png)
![Image text](https://raw.githubusercontent.com/jingyue202205/CIA-SSD-AI-TRT/master/pics/snapshot.png)



## More Information

Reference code:

[CIA-SSD](https://github.com/Vegeta2020/CIA-SSD)  

[spconv](https://github.com/poodarchu/spconv)

[tensorrtx](https://github.com/wang-xinyu/tensorrtx) 

[tensorrt_plugin](https://github.com/NVIDIA/TensorRT/tree/main/plugin)

[CUDA-PointPillars](https://github.com/NVIDIA-AI-IOT/CUDA-PointPillars)

[frustum_pointnets_pytorch](https://github.com/simon3dv/frustum_pointnets_pytorch)





