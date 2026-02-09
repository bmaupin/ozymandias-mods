# Modding game music

## Research

#### General

- Music stored in Ozymandias/Ozymandias/Content/FMOD/Desktop
- Master.bank
  - Master file that stores events
- Master.strings.bank
  - Stores filenames
- Music/\*.assets.bank
  - Stores the actual music tracks
- Music/\*.bank
  - ???

#### Tools

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
