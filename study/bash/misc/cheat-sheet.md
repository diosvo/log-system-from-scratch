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

File permissions: read (`R`), write (`W`), and execute (`X`).

| Number | Permission            | Presentation |
| :----: | --------------------- | :----------: |
|   0    | None                  |    `---`     |
|   1    | Execute only          |    `--x`     |
|   2    | Write only            |    `-w-`     |
|   3    | Write & Execute       |    `-wx`     |
|   4    | Read only             |    `r--`     |
|   5    | Read & Execute        |    `r-x`     |
|   6    | Read & Write          |    `rw-`     |
|   7    | Read, Write & Execute |    `rwx`     |

File ownership: user, group, and everyone.

e.g.

```ruby
$ ls -l
total 8
-rw-r--r--  1 diosvo  staff  3016 Aug 20 15:17 README.md
drwxr-xr-x  3 diosvo  staff    96 Aug 19 15:44 scripts
```

In the output above:

| file type                                  | user  | group | other | owner  | group | size | modification date | file/ folder name |
| ------------------------------------------ | ----- | ----- | ----- | ------ | ----- | ---- | ----------------- | ----------------- |
| `d`: a directory <br/> `-`: a regular file | `rw-` | `r--` | `r--` | diosvo | staff | 3016 | Aug 20 15:17      | README.md         |

#### Change Permissions

```ruby
chmod <permissions> <filename>
```

Where,

- `permissions` can be read, write, execute or a combination of them.
- `filename` is the name of the file for which the permissions need to change. This parameter can also be a list if files to change permissions in bulk.

We can change permissions using two modes:

1. **Symbolic** mode: this method uses symbols like `u`, `g`, `o` to represent users, groups, and others. Permissions are represented as `r`, `w`, `x` for read write and execute, respectively. You can modify permissions using `+`, `-` and `=`.

   | User Presentation | Description |
   | ----------------- | ----------- |
   | `u`               | user/ owner |
   | `g`               | group       |
   | `o`               | other       |

   | Operator | Description                                                                          |
   | -------- | ------------------------------------------------------------------------------------ |
   | `+`      | Add the permission to a file or directory                                            |
   | `-`      | Remove the permission                                                                |
   | `=`      | Set the permission if it's not present. Also override the permission if it's present |

2. **Absolute** mode: this method represents permissions as 3-digit octal numbers ranging from 0-7.

#### Change Ownership

| Description                             | Syntax                            |
| --------------------------------------- | --------------------------------- |
| User only                               | `chown <user> <filename>`         |
| User and group ownership simultaneously | `chown <user>:<group> <filename>` |
| Directory ownership                     | `chown -R <user> <dir>`           |
| Group ownership                         | `chown :<group> <filename>`       |
