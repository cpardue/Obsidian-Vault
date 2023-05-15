
VI is a powerful text editor that is included with most Unix-based systems.

## Modes

VI has two modes:
- **Command mode**: This is the default mode. In this mode, you can enter commands to edit and navigate through the text.
- **Insert mode**: In this mode, you can enter and edit text.

## Basic Commands

Here are some basic VI commands:

### Navigation

- `h`: Move left
- `j`: Move down
- `k`: Move up
- `l`: Move right
- `w`: Move to the beginning of the next word
- `b`: Move to the beginning of the previous word
- `e`: Move to the end of the current word
- `0`: Move to the beginning of the current line
- `$`: Move to the end of the current line
- `G`: Move to the last line of the file
- `nG`: Move to line `n`

### Editing

- `i`: Enter insert mode at the current cursor position
- `I`: Enter insert mode at the beginning of the current line
- `a`: Enter insert mode after the current cursor position
- `A`: Enter insert mode at the end of the current line
- `o`: Insert a new line below the current line and enter insert mode
- `O`: Insert a new line above the current line and enter insert mode
- `x`: Delete the character at the current cursor position
- `X`: Delete the character before the current cursor position
- `dd`: Delete the current line
- `D`: Delete from the current cursor position to the end of the line
- `cc`: Change (delete and enter insert mode) the current line
- `C`: Change (delete and enter insert mode) from the current cursor position to the end of the line

### Saving and quitting

- `:w`: Save the file
- `:wq`: Save and quit
- `:q`: Quit (if the file has not been modified)
- `:q!`: Quit without saving (if the file has been modified)

## Advanced Commands

Here are some advanced VI commands:

### Search and replace

- `/pattern`: Search for the specified pattern
- `n`: Repeat the last search
- `:%s/old/new/g`: Replace all occurrences of "old" with "new"
- `:%s/old/new/gc`: Replace all occurrences of "old" with "new" and confirm each replacement

### Undo and redo

- `u`: Undo the last change
- `ctrl + r`: Redo the last change

### Copy and paste

- `yy`: Copy the current line
- `p`: Paste the copied text after the current line
- `P`: Paste the copied text before the current line

### Visual mode

- `v`: Enter visual mode
- `V`: Enter visual line mode
- `ctrl + v`: Enter visual block mode
- `d`: Delete the selected text
- `y`: Copy the selected text
