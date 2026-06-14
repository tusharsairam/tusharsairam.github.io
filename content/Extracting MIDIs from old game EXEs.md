Extracting music files from games more than decades ago isn't that hard of a challenge but the setup leading to it is a ride. Running some of the so-bad-they're-good fangames I used to spend time with as a kid but on Ubuntu instead of Windows

For example, studying the Wine terminal logs when running [Yoshi vs. Windows Platinum Edition](https://mfgg.net/index.php?act=resdb&param=02&c=2&id=289):
```
0140:fixme:mcimidi:MIDI_player NIY: SMPTE track start 96:0:3 0.0
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=1
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=2
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=3
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=4
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=5
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=6
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=7
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=8
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=9
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=10
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=11
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=12
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=13
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=14
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=15
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=16
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=17
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=18
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=19
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=20
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=21
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=22
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=23
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=24
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=25
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=26
0140:fixme:mcimidi:MIDI_player NIY: MIDI port=0, track=27
```

The `mcimidi` part -> MCI: This is Windows' [Media Control Interface](https://learn.microsoft.com/en-us/windows/win32/multimedia/media-control-interface--mci), which can only run MIDI files conforming to Standard MIDI Files 1.0. The MIDI sequencer that's part of MCI is responsible for running the MIDI files. Since Wine has no code to interpret SMPTE time-code offsets, it continues to play the MIDI without honouring the initial offset
```
SMPTE track start 96:0:3 0.0
```
The `96` in `96:0:3` means that the offset duration is 96 ticks per frame. Since WINE has no implementation to kickstart this offset, the MIDI music starts immediately at tick 0 instead of tick 96. This causes an unintended but tiny de-sync between the game frame and music. Not perceptible, so not critical 

`NIY: MIDI port=0, track=1`
`NIY` stands for **Not Implemented Yet**. This is also non-critical -- WINE is simply routing all MIDI tracks to port 0. This is a problem when the sequencer is expected to route different MIDI tracks to different ports (synth devices, for example), because then all MIDI tracks would be routed to a single device. Harmless during emulation when a single Fluidsynth instance is running because it would be a generic MIDI arrangement with all tracks coalescing into one port. Plus, inspecting the source code of WINE -> [mcimidi.c](https://github.com/wine-mirror/wine/blob/master/dlls/mciseq/mcimidi.c)
```c
...
case 0x21:
	/* MIDI port (pp) */
	if (FIXME_ON(mcimidi)) {
		BYTE	bt;

		MIDI_mciReadByte(wmm, &bt);	/* == 0 */
		FIXME("NIY: MIDI port=%u, track=%u\n", bt, mmt->wTrackNr);
	}
	break;
```
This simply seems to assign the MIDI track to port 0 and throw a harmless `FIXME`, so the logs themselves indicate no danger!

The track count tells me ==there are 27 MIDI files in there==, so extraction should reveal 27 files in disk, and both WINE and Fluidsynth are detecting and playing them, which is good. Parsing the EXE itself using
```
file yoshwin_platinum.exe

> yoshwin_platinum.exe: PE32 executable (GUI) Intel 80386, for MS Windows, 7 sections
```
So it's a 32-bit EXE with 7 sections

What game engine/builder was used for this? 
```
strings yoshwin_platinum.exe | grep -i -E "game maker|gamemaker|multimedia fusion"

> "Multimedia Fusion Express - Stand alone application"
```
[Multimedia Fusion Express](https://wiki.mfgg.net/index.php?title=Multimedia_Fusion_Express) was the game engine (developed by Clickteam) used to make this. The second clue is that the game needs a `.cca` file, which is the extension used by MMFE. MMFE games package their MIDIs inside the EXE... or that's what I thought. Using [Watto Extract](https://www.watto.org/extract) inside WINE, extracting the files inside the EXE revealed nothing. The program cannot read `.cca` files sadly. [This available CCA extractor](https://forums.sonicretro.org/threads/cca-and-gam-file-format-info-and-more.24592/) also didn't work -- It seems to be focused on extracting only sprites and well, it didn't recognize my `.cca` file

`grep`ping the `.cca` file for a MIDI header returned nothing either. The data is probably compressed and the game dynamically uncompresses it during gameplay. I'm left with two options
1. Run the game and simultaneousy capture extracted MIDI files when frames start
2. Write my own MMF CCA parser
The second option is pointless - There's no benefit to writing a parser for a decades-old CCA file, so I'd rather write a script to dynamically pull files out while the game plays

```
inotifywait -m -r -e create,moved_to \ ~/.wine-pokemon/drive_c/windows/temp \ ~/.wine-pokemon/drive_c/users/$USER/AppData/Local/Temp \ --format '%w%f' | while read f; do file "$f" 2>/dev/null | grep -qi midi && cp "$f" ~/pokemon_test/middump/ && echo "Got: $f" done
```

`$USER` is to be replaced with whatever your username is in the machine. Also, the `pokemon` suffix is purely because when I ran a Pokemon EXE, I had created a custom WINE prefix for that, and that config works

This command watches gameplay and checks if the EXE extracts temporary MIDI files into `Temp`, which it does. As it throws MIDIs into `Temp`, this command grabs them and places them into the target folder, which in this example is `middump/`. The names are always `cncXXXX.mid` or `cncXXX.md`, with `X` being any alphanumeric

