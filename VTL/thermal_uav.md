### 基于学习的红外地理定位方法


```python
python3 train_pix2pix.py --dataset_name=satellite_0_thermalmapping_135 --datasets_folder=datasets --train_batch_size 2 --lr 0.0002 --patience 40 --epochs_num 40 --G_net unet_deep --G_loss_lambda 10.0 --G_gray --D_net patchGAN_deep --GAN_save_freq 5 --GAN_resize 1024 1024 --GAN_epochs_decay 20 --G_contrast
```

在训练期间，数据加载器随机采样查询数据集（红外图像），`triplet mining`搜索`positive`(查询图像的utm(位置)附近的区域)， `negative`（查询图像的`utm`(位置)附近之外的区域）。

每个`triplet`包括以下三个部分：
* one query thermal image
* one positive satellite image
* one negative satellite image

图像输入裁剪为`512x512`大小，为了提高生成质量，对卫星和红外输入图像进行上采样到`1024x1024`， 然后将生成的红外图像下采样到`512x512`， 以用来训练`SGM`， 训练`epoch`为40， `batch size`为8， 使用`UNet`架构进行生成器`G`和`Adam`优化器进行训练，学习速率设置为0.0002, 在20个`epoch`后进行线性学习率衰减。

训练过程：
* 随机采样queries中的5000个（一共9636个），因为这9636个有对应的hard_positive，也就是与之最相近（KNN的径向距离小于）的database。其中，随机采样过程采用每次随机1000个，随机5次，5个dataloader。
* 模型输入为positive样本，经过生成器输出为红外图像，判别器来判别红外图像的真假。
* 






经过`TGM`训练，生成的数据集有`79950`对卫星和红外图像。

数据集：

为了收集红外航空数据，使用了红外成像仪（8.7mm 焦距， 640p分辨率，50度水平视场）。从晚上9点到凌晨4点进行了6次飞行，并相应地将数据集标记为`Boson-nighttime`。为了创建单一的map， 首先运行了一个sfM(structure-from-motion)算法从多个视图重建红外地图，随后，通过将光学卫星图与相同空间分辨率的红外图像进行对齐进行校正。Boson-neighttime数据集所覆盖的地面面积总计33平方公里。最普遍的地图特征是沙漠，其中有一部分是农场，道路和建筑。沙漠地图的低对比度自相似红外特征使得在更精细的尺度上进行地理定位变得困难。

Bing必应的卫星图被裁剪为相应的区域，作为卫星参考地图。将红外地图拼接成512x512大小的红外图像，步长为35个像素。

每个裁剪的红外图像都与相应裁剪的卫星图像成对。Boson-nighttime夜间三次飞行所覆盖的区域被用于训练和验证。其余由其他三个飞行覆盖区域用于测试，Boson-nighttime数据集的训练/验证/测试数据集分别为10256/13011/26538对卫星图像和红外图像。



损失函数：
$L_{TGM}(G, D) = L_{GAN}(G, D) + \lambda_1 L_1(G)$


$L_{GAN}$是最小平方GAN损失。$L_1$是L1损失， $\lambda_1$是L1损失的权重，设为100， 或者10（更好的生成质量）




