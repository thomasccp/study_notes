#Deep learning accelerator

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
- Loading filters from DDR may become a performance bottleneck
