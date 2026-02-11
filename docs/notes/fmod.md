# FMOD notes

## General

- FMOD provides C, C++, C#, WASM, and JavaScript libraries
  - Almost all tools use these libraries instead of parsing the raw bank files
- FMOD refers to each piece of audio as an "event"

## Bank files

.bank is a sort of container file with different tags for the sections

#### .bank file format

- Header starts with `52 49 46 46` (`RIFF`)
- Bank GUID (in little endian) starts 12 bytes after the start of the `PROJBNKI` tag (around 0x30)
- Contains events starting with the tag `EVNTEVTB`
  - The GUID of the event (in little endian) starts 12 bytes after the start of the tag

#### Master.bank

> Master Bank is the primary sound bank of the sound system.
>
> The Master Bank defines:
>
> - The sound buses
> - Mixer and Routing
> - Global Parameters

(https://modding.scssoft.com/index.php?title=Documentation/Engine/Sound)

#### Master.strings.bank

- Contains all strings used by FMOD, such as
  - File names
  - Directory names
  - Event names
- "The strings bank contains all your events’ names and paths, and so is needed both when your code references events by name and path" (https://qa.fmod.com/t/can-master-bank-be-renamed-to-something-else/20546/2)

#### .assets.bank

- These files contain the actual audio
- Normally they contain no events
- .assets.bank files store the actual audio tracks
  - Contain no events, but instead FSB (FMOD Sample Bank) chunks
- These files are normally paired with a .bank file with the same name
  - It contains an event and metadata for the assets file
