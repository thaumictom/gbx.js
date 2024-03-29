# gbx-ts

a slim, fast and easy to set up read-only Gamebox (GBX) parser written in TypeScript

## Installation & Usage

### Node.js

Install the package using your desired package manager.

gbx-ts is fully compatible with yarn, pnpm, deno and bun.

```
npm install gbx
```

Additionally, if you plan to process the body of GBX files, you will need to install the optional dependency [lzo-ts](https://github.com/thaumictom/lzo-ts). Please note that lzo-ts is licensed under the GPL-3.0 License.

```
npm install lzo-ts
```

Instantiate a new GBX object with a data stream. If you are using TypeScript, use a [supported class](#supported-gbx-file-types) as generic for full code completion.

```ts
import { GBX, CGameCtnChallenge } from 'gbx';
import { promises } from 'fs';

const stream = await promises.readFile('path/to/file.Map.Gbx');

const gbx = new GBX<CGameCtnChallenge>(stream);

const result = await gbx.parse();

console.log(result.mapName);
```

### Web environment

If you are using the library in a web environment, you can include it directly from a CDN.

```html
<script src="https://www.unpkg.com/gbx"></script>
<script lang="ts">
	const { GBX } = gbx;

	new GBX(stream);
	// ...
</script>
```

If you plan to process the body of GBX files, also add [lzo-ts](https://github.com/thaumictom/lzo-ts). Please note that it is licensed under the GPL-3.0 License.

```html
<script src="https://www.unpkg.com/lzo-ts"></script>
```

## Reference

### Parameters

- `stream` A Uint8Array or number array with the GBX file data.
- `loglevel` (optional) A number to determine the loglevel.
  - `0` Errors only
  - `1` Warnings and errors (default)
  - `2` Info, warnings and errors
  - `3` Debug info, info, warnings and errors

### Methods

- `parseHeaders()` asynchronously parses the headers of the GBX file and returns the parsed class.
- `parse()` Asynchronously parses the GBX file and returns the parsed class.
  - This method requires the optional dependency [lzo-ts](https://github.com/thaumictom/lzo-ts).

### Game versions

gbx-ts provides a few methods to check the game version of the _parsed_ GBX file taken from [gbx-tool-api](https://github.com/bigbang1112-cz/gbx-tool-api/blob/main/Src/GbxToolAPI/GameVersion.cs).

- `isTM2020()` Returns true if the game is Trackmania 2020.
- `isManiaPlanet()` Returns true if the game is based on ManiaPlanet (includes Turbo and 2020)
- `isTurbo()` Returns true if the game is Trackmania Turbo. This method is not highly accurate.
- `isTMF()` Returns true if the game is Trackmania Forever. This method is not highly accurate.

### Utilities

gbx-ts comes with a few utility functions that you can import with `import { Utils } from 'gbx';`

- `Utils.getCheckpointCount(gbx: CGameCtnChallenge | CGameCtnGhost)` Returns a number with the amount of checkpoints in a map or ghost.
- `Utils.getRespawnsCount(ghost: CGameCtnGhost)` Returns a number with the amount of respawns in a ghost.
- `Utils.getCheckpointTimes(ghost: CGameCtnGhost)` Returns an array with the checkpoint times in a ghost.
- `Utils.getSectorTimes(ghost: CGameCtnGhost)` Returns an array with the sector times (delta of each checkpoint time) of a ghost.
- `Utils.getRespawnsByCheckpoint(ghost: CGameCtnGhost)` Returns an array of the amount of respawns per checkpoint in a ghost.

## Limitations

gbx-ts

- is read-only, meaning it can only parse GBX files and not write them.
- does not support all GBX file types, only the ones that have been implemented so far.
- tries to read all chunks of each class, but there might be some unknown, although skippable, chunks. An exception are any form of MediaTracker data, as it is not yet implemented. gbx-ts will try to force skip these.

Please refer to [alternative libraries](#alternative-libraries) if you need a more complete solution.

### Supported GBX file types

| Class                                                        | File extension            |
| ------------------------------------------------------------ | ------------------------- |
| [CGameCtnChallenge](/src/Classes/CGameCtnChallenge.ts)       | .Map.Gbx / .Challenge.Gbx |
| [CGameCtnReplayRecord](/src/Classes/CGameCtnReplayRecord.ts) | .Replay.Gbx               |
| [CGameCtnGhost](/src/Classes/CGameCtnGhost.ts)               | .Ghost.Gbx                |

## License

- gbx.js is licensed under the MIT License.
- Processing the body of GBX files requires the use of the optional dependency [lzo-ts](https://github.com/thaumictom/lzo-ts), which is licensed under the GPL-3.0 License.

## Credits

Special thanks to [BigBang1112](https://github.com/BigBang1112) for explaining his work on [gbx-net](https://github.com/BigBang1112/gbx-net), which was a great help in understanding the GBX file format, and giving me advice on how to improve the library.

### Alternative libraries

- [gbx-net](https://github.com/BigBang1112/gbx-net), a complete and in-depth read & write GBX parser library written in C#.
- [ManiaPlanetSharp](https://github.com/stefan-baumann/ManiaPlanetSharp), a GBX parser library and viewer written in C#.
- [pygbx](https://github.com/donadigo/pygbx), a GBX parser library written in Python.
