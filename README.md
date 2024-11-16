# retroarch_STM32MPU
Trying to get retroarch working on a STM32MP157C-DK2.

## Setup SDK

1. Setup [SDK](https://wiki.st.com/stm32mpu/wiki/Getting_started/STM32MP1_boards/STM32MP157x-DK2/Develop_on_Arm%C2%AE_Cortex%C2%AE-A7/Install_the_SDK)
1.  
```cd $HOME/STM32MPU_workspace/Developer-Package
 source SDK/environment-setup-cortexa7t2hf-neon-vfpv4-ostl-linux-gnueabi
 ```
1. 
``` mkdir $HOME/STM32MPU_workspace/Developer-Package/stm32mp1-openstlinux-24.06.26
 mkdir $HOME/STM32MPU_workspace/Developer-Package/stm32mp1-openstlinux-24.06.26/sources
```
1. `cd $HOME/STM32MPU_workspace/Developer-Package/stm32mp1-openstlinux-24.06.26/sources`
1. `git clone https://github.com/libretro/RetroArch.git retroarch`
1. `cd retroarch`
1. `/configure --host=arm-ostl-linux-gnueabi --disable-vg --enable-neon --enable-kms --enable-egl --enable-opengles --disable-opengl --enable-floathard --enable-alsa`
1. `sed -i 's/"-I/usr/include/"/"-I/root/STM32MPU_workspace/Developer-Package/SDK/sysroots/cortexa7t2hf-neon-vfpv4-ostl-linux-gnueabi/usr/include/"/g config.mk
1. `make -j16`
1. `scp retroarch root@192.168.2.120:/usr/local`
1. compile kernel with following: https://www.kernelconfig.io/config_snd_sequencer using [this](https://wiki.stmicroelectronics.cn/stm32mpu/wiki/Getting_started/STM32MP1_boards/STM32MP157x-DK2/Develop_on_Arm%C2%AE_Cortex%C2%AE-A7/Modify,_rebuild_and_reload_the_Linux%C2%AE_kernel) and [this](https://wiki.stmicroelectronics.cn/stm32mpu/wiki/Menuconfig_or_how_to_configure_kernel#Menuconfig_and_Developer_Package) and do the first one again after modifying

## To run

1. minicom in to kit
1. `psplash-drm-quit`
1. ~~`modetest -s 34:480x800-60 -d &`~~ currently starting kmscube first to correctly set display
1. make sure to set the following in /home/weston/.config/retroarch/retroarch.cfg
``` 
audio_device = "hw:0,0"
audio_driver = "alsa"
```
1. `su -l weston -c "/usr/local/retroarch --fullscreen"`

## TODO

1. Auto start everything, remove psplash, set display correctly, start retroarch
1. fix closing games (currently crashes/unresponsive)
1. volume control
1. backlight control
1. hardware buttons?

## Stupid stuff
My kit doesn't support 90 degree display rotation and retroarch has a [issue open for 6 years now about UI rotation](https://github.com/libretro/RetroArch/issues/6770)
