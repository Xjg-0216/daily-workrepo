
#### ResNet18

![[20171213211422068.png]]

### SGM

输入是 \[batch_size * 12, 3, 512, 512]

经过backbone， \[48, 256, 32, 32]

在聚合之前best setup加了add_bn， 因此有一个Conv + BN, 输出为 \[48, 64, 32, 32]

之后是NetVLAD， 输出为\[48, 4096]， \[48, 2]