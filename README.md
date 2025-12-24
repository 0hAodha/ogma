# ogma

A featureful terminal speed-reading tool based on [speedread](https://github.com/pasky/speedread).

Displays text one word at a time using RSVP (Rapid Serial Visual Presentation) aligned on optimal reading points, allowing you to read much faster than normal.

## Usage
```shell
ogma -w 500 tea.txt
```

## Controls

* `[` - slow down by 10%
* `]` - speed up by 10%
* `b` - toggle bionic reading mode
* `m` - toggle multiword mode
* space - pause (and show the last two lines)

## Options

* `-w WORDS_PER_MINUTE` - Set reading speed (default: 250)
* `-r RESUME_POINT` - Resume from a specific word position
* `-m` - Join short adjacent words together
* `-b` - Enable bionic reading (make the prefix of a word bold and the rest normal weight)

## License

Based on the original `speedread` by Petr Baudis, licensed under the MIT License (see LICENSE.MIT).

Modifications and additions by Aindréas Ó hAodha, licensed under GPL v3 (see LICENSE.GPL).

The combined work is distributed under GPL v3.
