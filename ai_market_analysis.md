# Applications:
1. Image recognition
2. Voice recognition

# Markets:
1. Mobile phone and tablets
- **Cost/mm^2**? 2017 28nm is $0.08/mm^2, ie. 4G quad core 50mm^2 costs $4; 4G 8 core 100mm^2 costs $8
- **Performance/W**? Big core: 10-100Gops/W (INT8); Small core: 100G-1Tops/W; GPU: 300Gops/W; Custom deep learning accelerator: 1Tops/W+?
- **Compute power**? Total system power < 2.5W, how much for deep learning? 1.5W? -> 1T/W means 1.5T
- CPU is okay for intermedittent compute such as voice recognition
- GPU/accelerator is needed for continuous compute such as AR. Nvidia DLA: no cmd decode, no speculative exec, no out-of-order exec
- **SRAM/cache size and bandwidth**? Can the weight small enough to be stored in internal SRAM? If not, is the DDR bandwidth enough?
- Accelerators: Nvidia DLA, VeriSilicon Vivante
- Photo editing, AR, voice assistance
- Android NN -> TensorFlow/TensorFlow Lite
- AR: 3D sensor (depth info) + camera (2D image) -> identify 3D objects -> 3D modelling -> combine images

2. Surveillance
- Sensor/MIPI -> ISP -> encoder -> network storage
- DLA/DSP gets image from sensor/MIPI or ISP
- Ambarella, TI, HiSilicon
- NB-IOT: <500mW power when activted by sensor, <10mW at idle
- GPU 200mm^2 cost $40

3. Home
- Set-top box, digital TV, smart speaker
- DSP for echo removal, denoising, voice recognition
- Natural language processing and neural network calculation are done on the cloud

4. Automotive
5. Robot
6. Drone
