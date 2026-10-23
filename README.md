# SuperTux for PS Vita

A port of [SuperTux](https://github.com/SuperTux/supertux) 0.7.0 to the PlayStation Vita.

SuperTux is a jump'n'run game with strong inspiration from the Super Mario Bros. games. Run and jump through multiple worlds, fighting off enemies by jumping on them, bumping them from below or tossing objects at them, grabbing power-ups and other stuff on the way.

![Screenshot](https://www.supertux.org/images/0_7_0/github_preview.png)

## Installation

1. Download `supertux2.vpk` and `supertux2.zip` from the [releases](https://github.com/DrDecki/supertux-vita/releases).
2. Install `supertux2.vpk` with VitaShell.
3. Extract `supertux2.zip` to `ux0:/data/`. You should end up with `ux0:/data/supertux2/data.zip`. Do not extract `data.zip` itself, the game reads it as it is.
4. Start the game from the LiveArea.

Save games and settings are stored in `ux0:/data/supertux2/user`. The game uses the system language of your Vita, you can still pick another language in the options menu.

## Building

You need [VitaSDK](https://vitasdk.org) with these packages:

```
vdpm sdl2 sdl2_image physfs openal-soft fmt glm freetype libogg libvorbis libpng zlib bzip2 libwebp libjpeg-turbo
```

Then build the VPK:

```
git clone --recursive -b vita https://github.com/DrDecki/supertux-vita
cd supertux-vita
cmake -B build-vita -DCMAKE_TOOLCHAIN_FILE=$VITASDK/share/vita.toolchain.cmake -DCMAKE_BUILD_TYPE=Release
cmake --build build-vita -j$(nproc)
```

The game data has to be packed as an uncompressed zip, the game reads it much faster that way than single files:

```
cd data
python3 -c "import os,zipfile; z=zipfile.ZipFile('../data.zip','w',zipfile.ZIP_STORED); [z.write(os.path.join(r,f),os.path.relpath(os.path.join(r,f),'.')) for r,d,fs in os.walk('.') for f in fs]; z.close()"
```

## Credits

All credit for the game goes to the [SuperTux team](https://github.com/SuperTux/supertux) and its contributors.

## License

SuperTux is licensed under the GNU General Public License version 3 or later, see [LICENSE.txt](LICENSE.txt). The game data uses various licenses, see the files in the `data` folder for details.
