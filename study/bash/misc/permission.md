## Permissions

File permissions: read (`r`), write (`w`), and execute (`x`).

| Number | Permission            | Presentation |
| :----: | :-------------------- | :----------: |
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
| :----------------------------------------- | :---- | :---- | :---- | :----- | :---- | :--- | :---------------- | :---------------- |
| `d`: a directory <br/> `-`: a regular file | `rw-` | `r--` | `r--` | diosvo | staff | 3016 | Aug 20 15:17      | README.md         |

### Change Permissions

```ruby
chmod <permissions> <filename>
```

Where,

- `permissions` can be read, written, executed, or combined.
- `filename` is the file name for which the permissions must change. This parameter can also be a list of files to change permissions in bulk.

We can change permissions using two modes:

1. **Symbolic** mode: this method uses symbols like `u`, `g`, and `o` to represent users, groups, and others. Permissions are represented as `r`, `w`, and `x` for read, write and execute, respectively. You can modify permissions using `+`, `-` and `=`.

   | User Presentation | Description |
   | :---------------- | :---------- |
   | `u`               | user/ owner |
   | `g`               | group       |
   | `o`               | other       |

   | Operator | Description                                                                           |
   | :------- | :------------------------------------------------------------------------------------ |
   | `+`      | Add the permission to a file or directory                                             |
   | `-`      | Remove the permission                                                                 |
   | `=`      | Set the permission if it's not present. Also, override the permission if it's present |

2. **Absolute** mode: this method represents permissions as 3-digit octal numbers ranging from 0-7.

### Change Ownership

| Description                             | Syntax                            |
| :-------------------------------------- | :-------------------------------- |
| User only                               | `chown <user> <filename>`         |
| User and group ownership simultaneously | `chown <user>:<group> <filename>` |
| Directory ownership                     | `chown -R <user> <dir>`           |
| Group ownership                         | `chown :<group> <filename>`       |
