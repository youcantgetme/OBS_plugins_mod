NVENC keyframe interval modification for OBS
============================================
> Tutorial for all the OBS releases (SLOBS included) , DLL for OBS 25.0.8

![image](https://github.com/youcantgetme/OBS_plugins_mod/blob/master/NVENC-keyintval/desc.png)

*** No more proxy files for 4K editing ***

The default keyframe interval/keyframe intval/GOP (group of frames) value of OBS's NVENC encoder are either `250` at 0=auto or `frame rate`(30/60) at 1 second.

This leaves huge decode loading while editing, especially at 2K above resolution, and could be eased by decreasing keyframe intval.

OBS provide intval by seconds unit option only, x264 encoder can use x264 option to modify it with frames unit, so code modification or binary patch is inevitable on NVENC.
 
 
# Binart editing
- Open obs-ffmpeg.dll with HEX capable editor (administrator required while in Program Files folder)

- find `41 BD FA`

0000dad9h: 45 85 ED 74 11 8B 47 0C 33 D2 41 0F AF C5 F7 77 

0000dae9h: 10 44 8B E8 EB 06 `41 BD FA` 00 00 00 49 8D 4E 14 

The `FA` means 250 in decimal, change it with another value in hexadecimal.

- Save

- Open OBS, switch Output mode to Advanced and select NVENC at Output tab 

- set Keyframe Interval to any seconds then apply, then set to 0 and apply again, this prevents odd behaviour of OBS to make change work.

- recording a 3 seconds clip 

- open Help > View Current log file, see if the keyint changes to assigned value instead 250.


# Notice

- Quality impact

GOP is the most important role to save the bitrate usage , decrease it means extra bitrate required to maintain quality.

Which means not ideal for streaming either , unless with additional recording encoder config in OBS.

- What number should I use ?

Depends on edit station and storage space you have, the normal stream recording are 60(@30 FPS) or 120(@60 FPS).

Set the value down to 10 and compare the loading while editing , then increase it to bitrate and hardware balance.

## DLL installation 
The DLL for OBS 25.0.8 , interval is set to 5 frames which is very low, 200%+ bitrate is recommended.

replace `obs-ffmpeg.dll` under `obs-plugins\64bit` folder
