# stable-diffusion-android-cpu-steps
I quantized a couple of onnx models to int8, and found I can use the onnxdiffusers pipeline on android in cpu mode.
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
