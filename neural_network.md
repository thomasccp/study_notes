Preliminary
- Convolution layer
- Fully connected layer
- Non-linear layer
- Pooling layer
- Element-wise layer

Optimisation
- Data quantization
- Weight reduction
- Low bit-width
- Winograd algorithm
- Loop unrolling

AlexNet
- ILSVRC 2012 (top-5 error: 16.4%)
- 8 layers
- conv1: 11x11 filter size, 96 output depth, 4 stride
- pool1: 27x27
- conv2: 5x5 filter size, 256 output depth
- pool2: 13x13
- conv3: 3x3 filter size, 384 output depth
- conv4: 3x3 filter size, 384 output depth
- conv5: 3x3 filter size, 256 output depth
- pool5: 6x6
- fc1: 4096 output size
- fc2: 4096 output size
- fc3: 1000 output size

VGG
- ILSVRC 2014 (top-5 error: 7.3%)
- 19 layers

GoogleNet
- ILSVRC 2014 (top-5 error: 6.7%)
- 22 layers

ResNet
- ILSVRC 2015 (top-5 error: 3.57%)
- 152/101/50/34/18 layers
