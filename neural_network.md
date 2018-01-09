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
- Simply stacking layers cause higher training and testing error
- Learned shallow model -> add extra layers as "identity"
- H(x) = F(x) + x, H(x) is the desired mapping, train the weight layer to fit F(x)

SqueezeNet
- Designed to improve efficiency (storage and speed)
- Not model compression
- Focus on more efficient conv, reduce network hyperparameters while maintaining accuracy
- Reference: SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB (https://arxiv.org/abs/1602.07360)
- **Fire module**: squeezenet (1x1 conv) + expand (concat a 1x1 conv and a 3x3 conv)
- Input: HxWxM
- Squeeze: kernel 1x1, S1 (<M) feature maps -> HxWxS1
- Expand a: kernel 1x1, Ea feature maps -> HxWxEa
- Expand b: kernel 3x3, Eb feature maps -> HxWxEb
- Concat: HxWx(Ea+Eb)
- Ea = Eb = 4xS1 < M
- 50x reduction in model size vs. AlexNet (4.8MB vs 240M)
- Deep compression achieves further reduction (0.66MB 8 bit, 0.47MB 6 bit)
- conv1 96, maxpool/2, fire2 128, fire3 128, fire4 256, maxpool/2, fire5 256, fire6 384, fire7 384, fire8 512, maxpool/2, fire9 512, conv10 1000, global avgpool, softmax
