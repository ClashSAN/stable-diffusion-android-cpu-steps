# stable-diffusion-android-cpu-steps
I quantized a couple of onnx models to int8 [with this script](https://github.com/LowinLi/stable-diffusion-streamlit/blob/main/src/stable-diffusion-streamlit/pages/model/quantization.py), and found I can use the onnxdiffusers pipeline on android in cpu mode.
(the method also works with x64 raspberry pi (rasbian) off the bat, you can also do this with a persistent TailsOS to make it portable)


    1. Install Termux and AnLinux from f-droid
    2. Download Ubuntu by copying the command from AnLinux to Termux. then start with the script.
    3. apt update and apt upgrade packages
    4. install python 3.10 (python3), pip and git
    5. clone https://github.com/azuritecoin/OnnxDiffusersUI [commit](4ca9651065af1b17f36010c28cb0f6bdaff78457)
    6. install diffusers (currently v0.10.2), transformers, torch, and onnxruntime
    7. download an onnx model, This is quantized to int8 so, at 256x256 it takes +1.3g ram peak, regular inference it's at +0.9
    8. run the txt2img script, best test settings are `--steps 8 --scheduler dpms --height 256 --width 256`

- I get 10-14s/it on snapdragon 865+ with 256x256.

- sd mini int8 onnx model: https://drive.filen.io/f/2d917512-0566-4903-ab55-e3f3415ed18f#MqhCRo5kCbOcaloWuYKlaLTUaDRhUwMd

There is a way to increase the speed: 
- Qualcomm - https://onnxruntime.ai/docs/execution-providers/SNPE-ExecutionProvider.html
- Generic Android - https://onnxruntime.ai/docs/execution-providers/NNAPI-ExecutionProvider.html

Int8 works well for < 448x448 dimensions, glitchy picture higher than that size..
Fp16 onnx model has appeared to use less memory for smaller sizes than fp32 at < 512x512 , but the memory seems similar or increased at 512x512 and the generation time has also increased.

My phone has 6gb ram and uses 2.3gb ram standby running LineageOS 19 (Android 12).
You may want to remove some background processes and use a non-bloated controllable OS to ensure you have enough ram for the task.
I can watch videos and do other things while images are generated in the background.

I have also tested this on a standard google pixel xl (4gb ram) running the standard os, the maximum size i can generate is 320x384. It's running 57% slower than snapdragon 865+.
