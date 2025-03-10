# Light Speed Player v1.13
# The fastest Amiga music player ever

## What is LSP?

LSP (Light Speed Player) is a very fast Amiga music player. It can play any MOD file and *outperforms* all existing Amiga music player in speed.
LSP comes with two different players: standard and insane. Standard is really fast, and insane is ultra fast!
Here is a speed comparaison of different Amiga music players. Numbers are given in "scanlines", measured on basic Amiga 500 OCS (PAL). Smaller is better

![image info](./png/bench_peak.png)

![image info](./png/bench_average.png)

LSP Features  | Normal Mode | Insane Mode
----------|-------------|------------
Code size | ~ 500 bytes | ~16KiB, depends of music
Average Time | 0.83 scanline         | 0.46 scanline
Peak Time | 2.27 scanlines   | 1.34 scanline
Seq Timing support | yes     | no

## LSP is production ready!

LSP is already used in plenty of Amiga (and Atari :)) productions. Some are listed here:

Demo  | Type | Info
----------|-------------|------------
[HAMazing by Desire](https://www.pouet.net/prod.php?which=94348) | Amiga OCS Demo | 1st at 68k Inside 2023
[Cycle-Op by Oxygene](https://www.pouet.net/prod.php?which=94129) | Amiga OCS Demo | 3rd at Revision 2023
[4kAsm by Lemon.](https://www.pouet.net/prod.php?which=88604) | Amiga OCS 4KiB intro | 3rd at Revision 2021
[4k MegaScroller by Oxygene](https://www.pouet.net/prod.php?which=91996) | Atari STE 4KiB intro | 1st at SillyVenture 2022
[Water My Grey Beard by Loonies](https://www.pouet.net/prod.php?which=93409) | Amiga OCS 40KiB | 1st at Gerp 2023
[Frustro by Desire](https://www.pouet.net/prod.php?which=93416) | Amiga OCS 40KiB | 2nd at Gerp 2023
[Clubisque by SMFX & friends](https://www.pouet.net/prod.php?which=93403) | Amiga OCS Demo | 1st at Gerp 2023
[4KMOD by Oxygene & Alcatraz](https://www.pouet.net/prod.php?which=90430) | Atari STE 4KiB Intro | 2nd at Sillyventure 2021

More complete list can be found [here!](https://www.pouet.net/lists.php?which=200)


## LSPConvert.exe

To be so fast, LSP is using its own data format. LSPConvert is a win32 command line tool to convert any .mod file into LSP compatible files.
```c
LSPConvert rink-a-dink.mod -insane
```
This command will produce three files:
- rink-a-dink.lsmusic : music score, to be loaded in any amiga memory
- rink-a-dink.lsbank : wave sound bank, to be loaded in amiga chip memory
- rink-a-dink_insane.asm : dedicated ultra fast player for that music (see below)

```c
LSPConvert options:
        -v : verbose
        -insane : Generate insane mode fast replayer source code
        -getpos : Enable LSP_MusicGetPos function use
        -setpos : Enable LSP_MusicSetPos function use
        -shrink : optimize sample bank size (remove sample bytes that won't be replayed)
        -nosampleoptim : preserve orginal .MOD soundbank layout
        -amigapreview : generate a wav from LSP data (output simulated LSP Amiga player)
        -nosettempo : remove $Fxx>$20 SetTempo support (for very old .mods compatiblity)
```

## LSP Standard : LightSpeedPlayer.asm

LSP standard is a very fast and *small* replayer. Player code is less than 512 bytes! ( it could fit in half a boot sector :) ). Standard player takes 1 rasterline average time. LightSpeedPlayer.asm is low level player. You have to call player tick each frame at the correct music rate. You also have to set DMACon using copper. You can have a look at Example_Insane.asm

## LSP Standard : LightSpeedPlayer_cia.asm

LSP also comes with an easy toolkit player using CIA timer. You don't have to worry about DMACon etc. This toolkit also support variable BPM music. You can have a look at Example_Easy.asm. 

## LSP Insane : modfilename_insane.asm

LSP have a special "insane" mode with an ultra fast replayer. The insane player source code is generated by LSPConvert.exe. This mode is made for dedicated world record, where every cycle count :) Standard mode should be enough for anybody. But if you really need half a scanline to break a new world record, use insane mode! Only drawback of insane mode is the replay code could take up to 30KiB of code, depending of the .mod. ( standard player is less than 512 bytes! )

## Amiga 500 benchmark

You can test the Amiga bootable image floppy disk "benchmark.adf" to see how different amiga players run on your real hardware.

![image info](./png/benchmark_shot.png)


## Credits

* LSP converter, format & player by Arnaud Carré (aka Leonard/Oxygene : [@leonard_coder](https://twitter.com/leonard_coder) )
* LSPConvert uses "micromod" library by Martin Cameron to load & analyze the .mod file
