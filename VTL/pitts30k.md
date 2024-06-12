
### Pitts30k




Pittsburgh(Pitts250k) 包含从谷歌街景(Google Street View)下载的250k数据集图像和从街景生成的24k查询图像,这些图像的拍摄时间跨度很大,长达数年. 作者将这些数据集划分成了3个大致相等的部分,用于训练\验证和测试,每个子集大概包含83k`数据库图像`和8k`查询图像`, 在地理上进行划分,确保集合包含独立的图像. 为了方便更快的训练,对于一些实验,使用了一个较小的自己(Pitts30K),在每个train/test/valid集合中包含10k的数据库图像. 并且这些图像在地理上也是不相交的。


本数据库Google可查，数据库官网网址：
https://blog.mapillary.com/update/2020/04/27/Mapillary-Street-Level-Sequences.html


由于Pitts30K数据集在matlab文件中，同时需要进行一些数据预处理， 仓库[https://github.com/gmberton/VPR-datasets-downloader](https://github.com/gmberton/VPR-datasets-downloader)对其进行标准化。


整理后的数据集格式：
```c
.
└── datasets
    └── dataset_name
        └── images
            ├── train
            │   ├── database
            │   └── queries
            ├── val
            │   ├── database
            │   └── queries
            └── test
                ├── database
                └── queries
```


matlab结构文件路径： 
![alt text](<Screenshot from 2024-05-22 23-38-01.png>)
pitts30k_train.mat文件中包含10000张数据库图像路径，以及位置坐标，使用UTM表示，在处理过程中，将其转换为为纬度latitude, 经度longitude。此外，包含7416张查询图像路径，以及位置坐标。


pitts30k_val.mat文件中包含10000张数据库图像路径，以及位置坐标，使用UTM表示，在处理过程中，将其转换为为纬度latitude, 经度longitude。此外，包含7608张查询图像路径，以及位置坐标。


pitts30k_test.mat文件中包含10000张数据库图像路径，以及位置坐标，使用UTM表示，在处理过程中，将其转换为为纬度latitude, 经度longitude。此外，包含6816张查询图像路径，以及位置坐标。

一共51840张图像


