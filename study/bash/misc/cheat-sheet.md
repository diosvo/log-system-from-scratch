A cheat sheet of the commands I use most for Linux

### Bash Shortcuts

| Command    | Description                                      |
| ---------- | ------------------------------------------------ |
| `Ctrl + R` | Search history                                   |
| `^abc^123` | Run previous command, replacing `abc` with `123` |

### Directory Operations

| Operation | Options | Syntax             | Description             |
| --------- | ------- | ------------------ | ----------------------- |
| `pwd`     |         | `mkdir <dir_name>` | Make a directory        |
| `mkdir`   |         |                    | Print working directory |
| `ls`      |         |                    | List files              |
|           | `-l`    |                    | Long listing format     |

### File Operations

| Operation | Syntax                  | Description                  |
| --------- | ----------------------- | ---------------------------- |
| `touch`   | `touch <file>`          | Create file                  |
| `cat`     | `cat <file_1> <file_2>` | Concatenate files and output |

### Other ones

- `echo $SHELL` - which shell we are actually using within the terminal window

### Permissions

- 0 = None `---`
- 1 = Execute only `--X`
- 2 = Write only `-W-`
- 3 = Write & Execute `-WX`
- 4 = Read Only `R--`
- 5 = Read & Execute `R-X`
- 6 = Read & Write `RW-`
- 7 = Read, Write & Execute `RWX`

eg: 777 - each number represents each object User - Group - Everyone

**NOTE**

`R`: Read
`W`: Write
`X`: Execute
