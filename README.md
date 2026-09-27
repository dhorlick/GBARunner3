# GBARunner3, Y2B edition

https://github.com/dhorlick/GBARunner3

GBARunner3 variant that maps Y button presses to B button inputs, to better reflect the GBA's gamepad layout

That may not work with some Rhythm Games that get X and Y input via interrupts.

This GBARunner3 version also has the ability to load "hicode" games (inherited from https://github.com/Gericom/GBARunner3/tree/feature/cache-hicode branch), like Super Mario Advance 4 Super Mario Bros. 3 with Bonus e-Reader Levels.

# Getting the .NDS File

If you prefer not to build it yourself, you can get one from the [latest release](https://github.com/dhorlick/GBARunner3/releases/).

# Building instructions

These instructions were written for a Linux system.

Install podman, so we can get the correct older versions of the [devkitPro tools](https://github.com/devkitpro) that GBARunner3 needs to build.

Aptitude: `sudo apt install podman`
Pacman: `sudo pacman -S podman`

Extract what we need from the 2023 Docker image to /opt/devkitpro…

```sh
podman create --name dkp_temp docker.io/devkitpro/devkitarm:20230501
podman cp dkp_temp:/opt/devkitpro - | sudo tar -C /opt -xf -
podman rm dkp_temp
```

Then, create environment variables and update your path
```sh
export DEVKITPRO=/opt/devkitpro
export DEVKITARM=$DEVKITPRO/devkitARM
export PATH=$PATH:$DEVKITPRO/tools/bin:$DEVKITARM/bin
```

You might want the above in your .bashrc, etc., if you expect to ever need it again.

Next, recursively clone the project to some convenient location:

`git clone --recurse-submodules git@github.com:dhorlick/GBARunner3.git`

Finally, build.

```sh
cd code
make
```

If all goes well, the .NDS file should get created in code/bootstrap

# Deploying to a Nintendo DS

Put GBARunner3.nds at the root of your previously set up [DSpico](https://www.lnh-team.org/) or R4 card. Copy the GBA BIOS file (details elsewhere) and JSON configs directory into your "_gba" folder:
   * GBARunner3.nds
   * gbar3-frontend.nds
   * _gba/
      * bios.bin
      * configs/
	     * 2G0P00.json
	     * ⋮

I recommend running GBARunner3 via [gbar3-frontend](https://github.com/flashcarts/gbar3-frontend). But I understand that, alternately, you can use TWiLight Menu++ plus GBARunner3 file mappings.

I have tested this mod succcessfully on a DSi XL and a DS Lite.