# Modding game music

## Replace game music

Use a tool like [FSB/BANK Extractor & Rebuilder](https://github.com/IZH318/FSB-BANK-Extractor-Rebuilder)

## Add new game music

Not really possible without rebuilding the FMOD project. We could add new bank files to Music/, but then we'd need to update Music_Game event in Music.bank to reference them, and more importantly add all the new strings with the filenames to Master.strings.bank. This would be prohibitively complicated

## Research

#### General

- Music stored in Ozymandias/Ozymandias/Content/FMOD/Desktop
- Tracks are in a proprietary FMOD format
  - See [FMOD notes](./fmod.md)

#### Files

- Master.strings.bank
  - Stores all strings for music events
    ```
    $ strings Master.strings.bank | grep -A 10 Tracks
    Tracks/
    1_Mediterranean Passage
    3_Phoenician_Scripture
    5_New_World
    7_Pyramids
    9_Persian_Dawn
    1_Tyrian_Purple
    3_Ngolo
    5_History
    7_Caspian_Sea
    9_Nyatiti
    ```
- Music/\*.assets.bank
  - Stores the actual music tracks
  - 48000 sample rate (this is important for calculating duration)
  - Stingers: win/lose tracks
  - Music: transition tracks?
- Music/\*.bank
  - Metadata for tracks
- Music.bank
  - GUID: `000d f80b 826c cf4e a48f e8c0 4651 94e3`
    - Only referenced in Master.strings.bank and Music.bank
  - Contains the `Music_Game` event
    - GUID `24d4 5250 8fe8 a94b aa91 d1b2 06ff 9428`
    - Only referenced in Master.strings.bank and Music.bank
- Music.assets.bank
  - Contains transition tracks to be played in between each of the soundtrack tracks

#### Tools

- [FSB/BANK Extractor & Rebuilder](https://github.com/IZH318/FSB-BANK-Extractor-Rebuilder)
  - Annoying to install but useful for examining detailed bank and track information
  - Make sure to copy 64-bit versions of required files (see release notes)
- [Fmod Bank Tools](https://github.com/Wouldubeinta/Fmod-Bank-Tools)
  - Can be used to replace music in .assets.bank files
  - ⚠️ Music must be same duration or less
- [FMOD Studio](https://www.fmod.com/download) (the official tool)
  - Used to create .bank files
  - ⚠️ Can't work with existing .bank files, only create new ones
    - Require's the game's project file to add new ones
- [FmodBankGUIDReplacer](https://github.com/TheHorscht/FmodBankGUIDReplacer)
  - List event GUIDs in a bank file
  - Replace event GUIDs

## Testing

```
$ wine guid_replacer.exe Ozymandias/Master.bank
Found a GUID at offset 4408 - {e8b9c26b-3938-4dd0-985b-a7d0187e5325} - 6bc2b9e83839d04d985ba7d0187e5325
Found a GUID at offset 4562 - {8e4322c5-29cf-483b-878d-a0e419ea3518} - c522438ecf293b48878da0e419ea3518
Found a GUID at offset 4744 - {638692d7-f8c8-4e2d-b393-4b29a239a832} - d7928663c8f82d4eb3934b29a239a832
Found a GUID at offset 4898 - {6f0ae2e2-308c-471e-a5f0-a9e2e2a4472f} - e2e20a6f8c301e47a5f0a9e2e2a4472f
```

```
$ wine guid_replacer.exe Ozymandias/Music/Music.bank 2>/dev/null
Found a GUID at offset 1622 - {5052d424-e88f-4ba9-aa91-d1b206ff9428} - 24d452508fe8a94baa91d1b206ff9428
Found a GUID at offset 1902 - {e03f148f-1b4f-4dae-bf19-2d83ae5914b3} - 8f143fe04f1bae4dbf192d83ae5914b3
```

```
$ wine 2>/dev/null guid_replacer.exe Ozymandias/Music/Stingers.bank
Found a GUID at offset 1556 - {e8b9c26b-3938-4dd0-985b-a7d0187e5325} - 6bc2b9e83839d04d985ba7d0187e5325
Found a GUID at offset 1710 - {543d7c9d-400f-4715-b044-f851fac2c021} - 9d7c3d540f401547b044f851fac2c021
Found a GUID at offset 1898 - {85e75efc-41fe-44ba-a5a4-3b809fa413ba} - fc5ee785fe41ba44a5a43b809fa413ba
```

```
$ wine 2>/dev/null guid_replacer.exe Ozymandias/Music/Track_Tyrian_Purple.bank
Found a GUID at offset 748 - {d2dafa45-930b-4d98-a0aa-c98d542f7d0b} - 45fadad20b93984da0aac98d542f7d0b

$ wine 2>/dev/null guid_replacer.exe Ozymandias/Music/Track_Tyrian_Purple.assets.bank
```

#### Manual steps to replace game music

👉 Not practical at all, just use a tool

1. Open the .bank (not .assets.bank file) for the track in the Music/ directory
   - e.g. _Track_Tyrian_Purple.bank_
1. Find the `EVNTEVTB` tag (starting around 0x2e0)
1. Get the track GUID starting 4 bytes after the tag (around 0x2ec)
   - e.g. `45fa dad2 0b93 984d a0aa c98d 542f 7d0b`
1. Get the track duration starting 0x140 bytes from the beginning of `EVNTEVTB` (around 0x420)
   - e.g. `b1fa eb00`, big endian = `0x00ebfab1` = 15465137
1. Divide by the sample rate (48000) to get the duration
   - e.g. 15465137 / 48000 = 322.19 seconds
1. Replace .asset.bank file with new audio
1. Update .bank file with new duration
