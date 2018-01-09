# Deep learning accelerator

1. Hardware
- Intel Arria 10 1150 FPGA
- 1.3 TFLOPS FP32 / 2.6 TFLOPS FP16
- 8TB/s on-chip memory bandwidth
- Caching for minimal DDR bandwidth usage
- Pipelined execution of all layers on FPGA
- Arithmetic optimisations
- Block FP16
- OpenCL kernels connected in a systolic fashion

2. Parallelism
- Output feature depth K -> number of processing elements (PEs)
- Input feature depth C -> size of dot-products in PEs
- Output feature width Q -> number of dot-products in a PE
- Filter width S -> number of dot-products in a PE

3. Caching
- Entire input image and all intermediate features are kept in stream buffer
- All filter weights for one output feature map are kept in PE caches
- Double buffering on stream buffer and PE caches

4. Arithmetic optimizations
- Winograd transformation
- Standard convolution: 3x3xdepth multiplications for 1 output
- Winograd F(4,3): 6x3xdepth multiplications for 4 outputs
- 2x saving in number of multiplications

5. Fully-connected layers
- Image batching to fully utilise compute resources
- Filter weights used in computations is higher than conv layers, i.e. less re-use of filter weights
- Storing filters in PE caches does not give any benefit
- Loading filters from DDR may become a performance bottleneck (DDR bandwidth bound)
- To alleviate this issue, stream in filters (in oppose to streaming in features during conv)
- Also, load and cache images from DDR (in oppose to loading and cachine filter from DDR)

6. Block floating point FP16
- Arria 10 has hardened DSP blocks for native FP32 computation
- To use FP16, use DSPs in 18x18 fixed-point mode
- Align 8 floats to have same exponent + 10-bit mantissa, then perform dot-product in 18x18 fixed-point, refenerate 16-bit output in floating-point

7. Performance
- Arria 10 @ 303MHz
- 45W TDP
- 1.3 TFLOPS in single precision
- AlexNet throughput 1020 img/s

8. More about conv execution
- Input and output features are double buffered in stream buffers
- Input features: width W, height H, depth C
- Input vectorization factors: Wvec, Cvec
- Output vectorization factors: Qvec, Kvec
- Filters: SxR, vectorization factor Svec
- Kvec PEs compute Kvec output feature maps at any given time, i.e. K/Kvec tiles to compute K output feature maps
- 1 x Wvec x Cvec input feature stick convolves with 1 x Svec x Cvec filter weights -> produce a Qvec output feature
- Arch: Cvec x Kvec _ format
- Features: stored in a double buffer and broadcast to PEs in a daisy-chain fashion evey cycle (each PE receives a stick of input feature for processing, and passes the data to an adjacent PE)
- Filters: each PE contains Wvec dot-product units -> 1 x Wvec x Cvec is convolved every cycle
- Filters: take Cvec x Wvec features and Cvec x Wvec transformed weights as input
- Filters: Accumulators are implemented as shift registers. Latency L means the result of the dot-product is back L cycles later -> each PE keeps L different partial sums that belong to L different output features (interleaved in W and H direction)
- Filters: weights are stored in PE caches, Wvec x Cvec memory banks to get one filter weight loaded from to PE every cycle. Use double buffering to overlap convolutions with the PE cache updates, i.e. load caches for a conv layer, at the same time prefetch filter weights for the next conv layer onto the caches
