# nvidia-tfserve-docker
## BUG 1: nvidia-container-cli: initialization error: nvml error: driver/library version mismatch: unknown

solutions :

1- rebooting

2- check versions: 

    dpkg -l | grep nvidia (look at nvidia-utils-xxx package version), and
    cat /proc/driver/nvidia/version (look at the version of Kernel Module, 460.56 - for example)

3- remove nvidia and reinstall again:

    sudo apt-get --purge remove "*nvidia*"
    
    distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
            
            
    sudo apt-get update
    
    sudo apt-get install -y nvidia-container-toolkit
    
    
    sudo systemctl restart docker
    
    #check
    sudo docker run --rm --runtime=nvidia --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
    
    #output
    
    
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 450.51.06    Driver Version: 450.51.06    CUDA Version: 11.0     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  Tesla T4            On   | 00000000:00:1E.0 Off |                    0 |
    | N/A   34C    P8     9W /  70W |      0MiB / 15109MiB |      0%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+


## BUG 2: CUDA_ERROR_COMPAT_NOT_SUPPORTED_ON_DEVICE: kernel version 440.31.0 does not match DSO version 440.33.1 

solutions :

        sudo add-apt-repository ppa:graphics-drivers/ppa
        sudo apt-get update
        sudo apt-get upgrade
        
Then reboot the computer.


## BUG 3: Could not load dynamic library 'libcudart.so.11.0

    sudo apt update
    sudo apt install build-essential
    gcc --version
    g++ --version
    sudo apt install nvidia-cuda-toolkit
    nvcc --version

First, find out where the "libcudart.so.11.0" is

If you lost other at error stack, you can replace the "libcudart.so.11.0" by your word in below:

sudo find / -name 'libcudart.so.11.0'

Output in my system.This result shows where the "libcudart.so.11.0" is in my system:

/usr/local/cuda-11.1/targets/x86_64-linux/lib/libcudart.so.11.0

If the result shows nothing, please make sure you have install cuda or other staff that must install in your system.
Second, add the path to environment file.

    edit /etc/profile

sudo vim /etc/profile

    append path to "LD_LIBRARY_PATH" in profile file

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.1/targets/x86_64-linux/lib

    make environment file work

source /etc/profile


## Bug 4: nvidia-smi stops working after reboot ununtu 18.04/ NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running

    sudo reboot
    

If you have secure boot enabled, remember to "enroll mok" after reboot. Otherwise nvidia drivers won't be loaded. At least this happened to me.

