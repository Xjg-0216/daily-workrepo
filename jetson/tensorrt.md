TRT主要包含优化器和运行时库两部分：
* **optimizer**: 是进行模型表示的优化， 例如：层融合， 图优化， 量化， Kernel选择等操作，目的是为了找到模型在GPU上，运行时最优的表示形式以及方案（kernel选择）, 这里的Kernel是具体某个算子的实现方法， 例如卷积的实现就有很多，可以根据实现方式，算法，数据布局等进行挑选，优化器会根据网络结构，硬件配置和精度需求等因素，选择最合适当前场景的Kernel进行推理计算，以获取最佳的性能，optimzier是在生产前做准备工作，获取最优方案，供runtime使用
* **runtime**是运行时库，可以实现推理引擎创建，推理计算，引擎销毁等内容，是生产服务中使用的。到这里，就知道TRT会根据GPU特性，任务特性，用户自定义操作等，将pytorch模型转换为TRT运行时库能读取的形式，简单粗暴就是pytorch模型转换为TRT模型
TRT模型文件是高度优化的模型表示形式，在真正使用时被Runtime所加载使用，可以实现高性能推理。