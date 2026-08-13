# Terminal Commands

A **terminal** is an interface that allows you to interact with and perform operations on your computer using commands.

## Basic Terminal Commands

| Command | Description                          |
| ------- | ------------------------------------ |
| `pwd`   | Print the current working directory  |
| `cd`    | Change the current working directory |
| `ls`    | List files and directories           |
| `mkdir` | Create a new directory               |
| `touch` | Create a new empty file              |
| `cat`   | Display the contents of a file       |
| `vi`    | Open and edit a file                 |
| `mv`    | Move or rename a file or directory   |
| `cp`    | Copy a file or directory             |

---

## 1. `pwd`

`pwd` stands for **Print Working Directory**.

It displays the path of the directory you are currently working in.

```bash
pwd
```

Example:

```text
/home/user/projects
```

---

## 2. `cd`

`cd` stands for **Change Directory**.

It is used to move from one directory to another.

```bash
cd directory_name
```

Example:

```bash
cd projects
```

### Go to the parent directory

```bash
cd ..
```

### Go to the home directory

```bash
cd ~
```

---

## 3. `ls`

`ls` is used to **list files and directories** in the current directory.

```bash
ls
```

Example:

```text
file.txt
project
notes.md
```

### List detailed information

```bash
ls -l
```

---

## 4. `mkdir`

`mkdir` stands for **Make Directory**.

It is used to create a new directory.

```bash
mkdir directory_name
```

Example:

```bash
mkdir projects
```

This creates a directory named `projects`.

---

## 5. `touch`

`touch` is commonly used to **create a new empty file**.

```bash
touch filename
```

Example:

```bash
touch notes.txt
```

This creates an empty file named `notes.txt`.

---

## 6. `cat`

`cat` is commonly used to **display the contents of a file**.

```bash
cat filename
```

Example:

```bash
cat notes.txt
```

If `notes.txt` contains:

```text
Hello World
```

The command outputs:

```text
Hello World
```

---

## 7. `vi`

`vi` is a text editor that can be used to create and edit files.

```bash
vi filename
```

Example:

```bash
vi notes.txt
```

### Basic `vi` workflow

When you open a file using `vi`, you start in **Normal Mode**.

To start typing/editing:

```text
Press i
```

This enters **Insert Mode**.

After editing the file:

```text
Press Esc
```

This takes you back to **Normal Mode**.

To save and exit:

```text
:wq
```

Then press:

```text
Enter
```

### Force save and exit

```text
:wq!
```

> `!` forces the operation when necessary, such as overriding certain restrictions.

### Quick `vi` workflow

```text
vi notes.txt
      ↓
Press i
      ↓
Write/Edit content
      ↓
Press Esc
      ↓
:wq
      ↓
Enter
```

---

## 8. `mv`

`mv` is used to **move or rename** files and directories.

### Rename a file

```bash
mv oldname newname
```

Example:

```bash
mv old.txt new.txt
```

This renames `old.txt` to `new.txt`.

### Move a file

```bash
mv file.txt /path/to/directory/
```

Example:

```bash
mv notes.txt documents/
```

This moves `notes.txt` into the `documents` directory.

### Move and rename at the same time

```bash
mv old.txt documents/new.txt
```

---

## 9. `cp`

`cp` is used to **copy files and directories**.

### Copy a file

```bash
cp oldname newname
```

Example:

```bash
cp notes.txt notes_backup.txt
```

This creates a copy of `notes.txt` named `notes_backup.txt`.

### Copy a directory

When copying a directory and its contents, use the `-r` (**recursive**) option:

```bash
cp -r old_directory new_directory
```

Example:

```bash
cp -r project project_backup
```

This copies the `project` directory and everything inside it to `project_backup`.

---

## Quick Reference

```text
pwd          → Print current working directory
cd           → Change directory
ls           → List files and directories
mkdir        → Create directory
touch        → Create empty file
cat          → Display file contents
vi           → Edit a file
mv           → Move or rename file/directory
cp           → Copy file
cp -r        → Copy directory recursively
```

## Important Difference: `mv` vs `cp`

```text
mv → Move → Original is moved/renamed
cp → Copy → Original remains
```

Example:

```bash
mv file.txt newfile.txt
```

The original `file.txt` no longer exists under that name.

```bash
cp file.txt newfile.txt
```

Both `file.txt` and `newfile.txt` exist.
