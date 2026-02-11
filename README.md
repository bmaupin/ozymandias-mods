# Ozymandias mods

## Add additional game music

Adding additional game music isn't possible. See [here](docs/notes/music.md) for more information.

Here's a workaround to play additional music in Linux:

1. Create a new directory somewhere to hold the music
1. (Optional) Extract game music using an [FMOD tool](docs/notes/music.md) and copy to the directory
1. Copy additional music to the directory
1. Go to the game properties in Steam and set _Launch Options_ to:
   ```
   mpv --no-audio-display --shuffle ~/Music/Ozymandias/ & %command% && killall mpv
   ```
   (Replace `~/Music/Ozymandias/`) with the directory you put the music in
   - This will automatically start playing the music in that directory in a random order when the game is started and stop when the game is stopped
1. In the game, go to _Settings_ and turn _Music Volume_ all the way down
