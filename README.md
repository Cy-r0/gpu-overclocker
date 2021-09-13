# What does this script do

This contains some overclocking profiles for my GPUs.

Execute it and pass it the name of the profile you want to set (either 'default' or 'mining' for now).

Example:
```bash
./gpu-overclocker default
```

It might run some things as sudo, so it may ask for the password.


# How to manually overclock nvidia gpus on linux


## 0. Enable overclocking on headless Nvidia GPUs on linux

Run the following command to generate an xorg.conf file in /etc/X11:

```bash
sudo nvidia-xconfig -a --cool-bits=24 --allow-empty-initial-configuration
```

This will create virtual screens for all GPUs so that you can overclock all of them.


## 1. Change power limit

```bash
sudo nvidia-smi -i $GPU_ID -pl $POWERLIMIT
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
nvidia-settings -a "[gpu:$GPU_ID]/GPUGraphicsClockOffsetAllPerformanceLevels=$OFFSET"
```

Where OFFSET is the MHz delta.

NOTE: there is another setting called GPUGraphicsClockOffset[$PWR\_MODE] but it doesn't seem to be working.

Example:
```bash
nvidia-settings -a "[gpu:0]/GPUGraphicsClockOffsetAllPerformanceLevels=-100"
```

## 3. Change GPU memory clock offset

Basically the same as core clock offset, but instead the command is this:
```bash
nvidia-settings -a "[gpu:0]/GPUMemoryTransferRateOffsetAllPerformanceLevels=100"
```
