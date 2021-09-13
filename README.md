# HOW TO TUNE PERFORMANCE OF NVIDIA GPUS


## 0. Enable overclocking on headless Nvidia GPUs on linux

Run the following command to generate an xorg.conf file in /etc/X11:

```bash
sudo nvidia-xconfig -a --cool-bits=24 --allow-empty-initial-configuration
```

This will create virtual screens for all GPUs so that you can overclock all of them.


## 1. Change power limit

```bash
sudo nvidia-smi -i $GPU\_ID -pl $POWERLIMIT
```

where GPU\_ID is the number of the GPU, and POWERLIMIT is any integer between the min and max power limit allowed on the GPU (check nvidia-smi -q).
You want to reduce power limit so that it doesn't impact performance that much, but reduces the amount of generated heat significantly (also keeps fans from spinning too fast).
From random tests, it seems like hashrate doesn't decrease until you go down to ~125 W.

Example:
```bash
sudo nvidia-smi -i 0 -pl 140
```

## 2. Change GPU core clock offset

```bash
nvidia-settings -a "[gpu:$GPU\_ID]/GPUGraphicsClockOffsetAllPerformanceLevels=$OFFSET"
```

Where OFFSET is the MHz delta.

NOTE: there is another setting called GPUGraphicsClockOffset[$PWR\_MODE] but it doesn't seem to be working.

Example:
```bash
nvidia-settings -a "[gpu:0]/GPUGraphicsClockOffsetAllPerformanceLevels=-100"
```

## 3. Change GPU memory clock offset

Basically the same as for core clock offset, but instead the command is this:
```bash
nvidia-settings -a "[gpu:0]/GPUMemoryTransferRateOffsetAllPerformanceLevels=100"
```
