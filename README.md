# DCGAN-CIFAR-10
入门级DCGAN

环境：Google Colab + Google Drive （注意Google Colab的读写文件）  
框架：TensorFlow的Keras  
关于参数的选择：论文里提到了一些参数，但是这样照搬我发现迭代了很久也是噪音。然后上网查了一下，发现知乎上有人说在这个数据集上的优化器可以选择RMSprop(lr=0.0008, clipvalue=1.0, decay=1e-8)（而不是论文里提到的Adam），果然就解决问题了。


Generator搭建：  
G中的输入可以采用全连接层但是需要reshape成一个4 dimension的tensor(Batch_size, Height, Width, Channle)  
pooling层改成卷积层，G用的fractional-strided convolutions，就是逆卷积，需要上采样  
需要BatchNormalization，但是不对G的output layer使用BN  
G的激活函数用的ReLU，output层用的Tanh；  

Discriminator搭建：  
D中用全卷积层代替池化层，strided convolutions  
取消全连接层：D中最后的卷积层可以先flatten然后送入一个sigmoid分类器  
用batchnorm，但是不对D的input layer使用BN  
在D中所有层的激活函数都是用LeakyReLU  
