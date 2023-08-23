## 📄 Curriculum

1. [Inception](docs/1-inception.md)

**NOTE:**

- [Cheat-sheet](./misc/cheat-sheet.md)
- [Change permission files and ownership](./misc/permission.md)

### Shell Scripting Real-Time Scenarios

Find:

- Find and kill all the zombie processes.
- First 10 biggest files in the file system and write the output to a file.
- The files created and their sizes. It should accept the number of days as input. Or a from and to date format as inputs.

Automate the process of:

- Creating new user accounts on a Linux server and setting up their permissions and SSH access.
- Rotating log files and compressing old files to save disk space.
- Updating a list of servers with the latest security patches.

Others:

- Gracefully unmount a disk.
- Send email.
- Monitor CPU, Memory, and Disk usage and send the output to a file in table format and send an alert if either of them exceeds a certain threshold.
- List of users logged in by date and write it to an output file.
- Copy files recursively to remote hosts.
- Displays the number of failed login attempts by IP address and location.
- Parses a log file and forwards a specific value with a timestamp to an output file.
- Check the status of a list of URLs and send an email notification if any of them are down.

### Ensure you include the following concepts in the shell scripts you are writing:

1. [Variable definitions](scripts/bash-scripting-on-linux/variables)

- One of the reasons why we include a dollar sign in front of the variable name?

  -> Help us avoid name collisions

  ```ruby
  $ ls="Hello Again"
  $ ls
  > ... # print list files in current working directory
  $ echo $ls
  > Hello Again
  ```

- Any variables in bash are tied to that session will be wiped out once you close the window (e.g. type `exit`)
- We use double quotes in bash, it's going to treat the variables inside the `echo` statement to show what the variables equal rather than the names themselves.
- Sub-shell (e.g. `$(pwd)`) allows you to execute the command in the background.
- Within the environment, many default variables are always declared (e.g. `$USER`). Type `env` to view the environment variables within the Linux system.
- Lowercase and uppercase variable names have an important distinction:
  - Uppercase: is commonly a system variable, something that's built-in.
  - Lowercase: our variables.

2. Use [cut, awk, grep, and sed](https://blog.knoldus.com/play-with-text-in-linux-grep-cut-awk-sed/)
3. Handling Input/ Output / Error redirection.
4. [Conditions/ If else statements](scripts/bash-scripting-on-linux/conditions)

   We can type `man test` to get all expressions that could be used.

   | Syntax                | Description                                        |
   | :-------------------- | :------------------------------------------------- |
   | 1. Numeric Comparison |
   | `-eq`                 | Equal                                              |
   | `-ne`                 | Not equal                                          |
   | `-gt`                 | Greater than                                       |
   | 2. Files              |                                                    |
   | `-f` / `-d`           | A check for the existence of files and directories |
   | `command -v $package` | Check for the existence of a command               |

5. Case statement scripts.
6. Loops (For/Do-While)
7. [Exist status](scripts/bash-scripting-on-linux/exist-codes)

- `ls -l /misc` - Bash perceives success and failure
- `$?` - Check the `$?` variable to determine a command's exit code, returns:

  - `0` - the command was successful
  - An exit code of anything other than zero constitutes failure

  => Why do we need to know what an exact code is?

8. Command line arguments.

## 🗞️ Blogs

- [How to Learn Linux Shell Scripting for DevOps](https://devopscube.com/linux-shell-scripting-for-devops/)

## 📚 Courses

📑

- [The Bash Guide](https://guide.bash.academy/)
- [How to Change File Permissions and Ownership in Linux](https://www.freecodecamp.org/news/linux-chmod-chown-change-file-permissions/)

📹

- [Bash Scripting on Linux](https://www.youtube.com/playlist?list=PLT98CRl2KxKGj-VKtApD8-zCqSaN2mD4w) > 🗂️ study/bash/scripts/bash-scripting-on-linux
- [Linux Crash Course](https://www.youtube.com/playlist?list=PLT98CRl2KxKHKd_tH3ssq0HPrThx2hESW)
