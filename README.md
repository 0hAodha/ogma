# ogma

A featureful terminal speed-reading tool based on [speedread](https://github.com/pasky/speedread).

Displays text one word at a time using RSVP (Rapid Serial Visual Presentation) aligned on optimal reading points, allowing you to read much faster than normal.

## Installation

ogma requires Perl 5.14 or later and the following CPAN modules:

- `Term::ANSIColor`
- `Term::ReadKey`
- `Time::HiRes`
- `TOML::Tiny`

Install dependencies:

```shell
cpan Term::ANSIColor Term::ReadKey Time::HiRes TOML::Tiny
```

Then place the `ogma` script somewhere on your `$PATH` and make it executable:

```shell
chmod +x ogma
```

## Usage

```shell
ogma [OPTIONS] [file]
```

If no file is given, ogma reads from stdin:

```shell
echo "The quick brown fox" | ogma
```

## Options

| Option | Description |
|---|---|
| `-w`, `--wpm` | Reading speed in words per minute (default: 250) |
| `-r`, `--resume` | Resume from a specific word position |
| `-m`, `--multiword` | Enable multiword mode |
| `-b`, `--bionic` | Enable bionic reading mode |
| `--init` | Write a default configuration file to `~/.config/ogma/config.toml` |

## Controls

| Key | Action |
|---|---|
| `[` | Decrease speed by 10 wpm |
| `]` | Increase speed by 10 wpm |
| `space` | Pause / unpause |
| `b` | Toggle bionic reading mode |
| `m` | Toggle multiword mode |
| `q` / `Ctrl-C` | Quit and print stats and resume point |

When you quit, ogma prints the word position you stopped at, which you can pass to `-r` to resume later:

```shell
ogma -r 1823 book.txt
```

## Reading Modes

**Bionic reading** makes the prefix of each word bold and the remainder normal weight.

**Multiword mode** joins short adjacent words together into a single display unit.

## Configuration

ogma looks for a configuration file at `$XDG_CONFIG_HOME/ogma/config.toml`, falling back to `~/.config/ogma/config.toml`. To generate a default config file:

```shell
ogma --init
```

All values are optional — any key omitted from the config file falls back to the built-in default. Command line flags always override config file values.

```toml
[timing]
wpm       = 250
wordtime  = 0.9   # base time per word
lentime   = 0.04  # additional time per character
commatime = 2.0   # pause multiplier for commas and semicolons
fstoptime = 3.0   # pause multiplier for full stops, question marks, etc.
multitime = 1.2   # time multiplier for multiword groups
firsttime = 0.2   # minimum display time for the first word

[display]
multiword = false  # enable multiword mode by default
bionic    = false  # enable bionic reading mode by default
```

## File Handlers

ogma can preprocess files before reading them, based on their filename or URL. This lets you pass any file or URL directly to ogma rather than piping a preprocessor manually.

Handlers are defined as `[[filehandler]]` entries in the config file. Each handler has a `regex` to match against the input and a `command` to run. Use `%f` in the command as a placeholder for the input — it will be substituted with the filename or URL at runtime.

```toml
[[filehandler]]
regex   = '\.pdf$'
command = 'pdftotext %f -'

[[filehandler]]
regex   = '^https?:\/\/'
command = 'w3m -dump %f'
```

Handlers are matched in the order they appear in the config file — the first match wins, so more specific patterns should be placed before more general ones. If no handler matches, the file is read as plain text.

## License

Based on the original `speedread` by Petr Baudis, licensed under the MIT License (see `LICENSE.MIT`).

Modifications and additions by Aindréas Ó hAodha, licensed under GPL v3 (see `LICENSE.GPL`).

The combined work is distributed under GPL v3.
