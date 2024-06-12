
rknn模型的推理运行有两种方式：
* 直接在PC上利用模拟环境调用rknn模型进行推理运算
* 利用rk3588的硬件NPU进行推理运算（需要在PC和RK3588开发板之间进行数据传输）

为使用RKNPU，需要先在PC上运行RKNN-Toolkit2工具，将训练好的模型转换成RKNN格式的模型，然后在开发板上使用RKNN C API或Python API进行推理。 

RKNN-Toolkit2是一套软件开发工具包，供用户在PC和Rockchip NPU平台上进行模型转换、推理和性能评估。 
RKNN-Toolkit-Lite2为Rockchip NPU平台提供Python编程接口，帮助用户部署RKNN模型，加速AI应用落地。 
RKNN Runtime为Rockchip NPU平台提供C/C++编程接口，帮助用户部署RKNN模型，加速AI应用落地。 RKNPU内核驱动负责与NPU硬件交互，已经开源，可以在Rockchip内核代码中找到。

#### demo流程：


#### 1. 准备模型


进入rknn_model_zoo/examples/yolov5/model目录，运download_model.sh脚本，该脚本将下载一个可用的YOLOv5 ONNX模型，并存放在当前model目录中，参考命令如下：

```shell
# 进入 rknn_model_zoo/examples/yolov5/model 目录
cd Projects/rknn_model_zoo/examples/yolov5/model
# 运行 download_model.sh 脚本，下载 yolov5 onnx 模型
# 例如，下载好的 onnx 模型存放路径为 model/yolov5s_relu.onnx
./download_model.sh
```
#### 2. 模型转换

进入 rknn_model_zoo/examples/yolov5/python 目录，运行 convert.py 脚本，该脚本将原始的ONNX 模型转成 RKNN 模型，参考命令如下：
```shell
# 进入 rknn_model_zoo/examples/yolov5/python 目录
cd Projects/rknn_model_zoo/examples/yolov5/python
# 运行 convert.py 脚本，将原始的 ONNX 模型转成 RKNN 模型
# 用法: python convert.py model_path [rk3566|rk3588|rk3562] [i8/fp][output_path]

python convert.py ../model/yolov5s_relu.onnx rk3588
i8 ../model/yolov5s_relu.rknn
```

#### 3. 运行板端demo

进入 rknn_model_zoo/examples/yolov5/python 目录，运行 yolov5.py 脚本，便可通过`连板调试`的方式在板端运行 YOLOv5 模型，参考命令如下：

```shell
# 进入 rknn_model_zoo/examples/yolov5/python 目录
cd Projects/rknn_model_zoo/examples/yolov5/python
# 运行 yolov5.py 脚本，在板端运行 yolov5 模型
# 用法: python yolov5.py --model_path {rknn_model} --target
{target_platform} --img_show
# 其中，如果带上 --img_show 参数，则会显示结果图片
# 注：这里以 rk3588 平台为例，如果是其他开发板，则需要修改命令中的平台类型
python yolov5.py --model_path ../model/yolov5s_relu.rknn --target
rk3588 --img_show
# 如果想先在计算机端运行原始的 onnx 模型，可以参考以下命令
# 用法: python yolov5.py --model_path {onnx_model} --img_show
python yolov5.py --model_path ../model/yolov5s_relu.onnx --img_show
```

#### 4. 评估




#### 5. 板端部署

在PC端运行RKNN C Demo（需要用到python convert后的RKNN模型）， 将C/C++源代码编译成可执行文件，然后将可执行文件、模型文件、测试图片等相关文件推送到板端上，最后在板端运行可执行文件。

需要使用 rknn_model_zoo 目录下的build-linux.sh 脚本进行编译。在运行 build-linux.sh 脚本之前，需要指定编译器的路径GCC_COMPILER 为本地的 GCC 编译器路径。即在 build-linux.sh 脚本中，需要加入以下命令：
```shell
# 添加到 build-linux.sh 脚本的开头位置即可
GCC_COMPILER=Projects/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu
```

然后在 rknn_model_zoo 目录下，运行 build-linux.sh 脚本，参考命令如下：

```shell
# 进入 rknn_model_zoo 目录
cd Projects/rknn_model_zoo
# 运行 build-linux.sh 脚本
# 用法:./build-linux.sh -t <target> -a <arch> -d <build_demo_name> [-
b <build_type>] [-m]
# -t : target (rk356x/rk3588) # 平台类型，rk3568/rk3566 都统一为 rk356x
# -a : arch (aarch64/armhf) # 板端系统架构
# -d : demo name # 对应 examples 目录下子文件夹的名称，如 yolov5、mobilenet
# -b : build_type(Debug/Release)
# -m : enable address sanitizer, build_type need set to Debug
./build-linux.sh -t rk356x -a aarch64 -d yolov5
```

编译完成后，会在 rknn_model_zoo 目录下产生 install 文件夹， 其中有编译好的可执行文件，以及测试图片等相关文件。参考目录结构如下：


```shell
install
├── rk356x_linux_aarch64 # rk356x 平台
└── rk3588_android_arm64-v8a # rk3588 平台
└── rknn_yolov5_demo
├── lib # 依赖库
├── model # 存放模型、测试图片等文件
└── rknn_yolov5_demo # 可执行文件
```



