---
tags:
  - shell
  - golang
---

[Github Repo](https://github.com/t3snake/shell-go-codecrafters)  
[Codecrafters Specs](https://app.codecrafters.io/courses/shell/overview)  

## Code Summary

### Main REPL Loop (`main.go`)

- **REPL Loop:**  
	The `execREPL` function loops indefinitely, printing a prompt (`$ `) and then waiting to read user input via a custom raw-mode line reader.

- **Command Parsing & Execution:**  
	- The input is parsed into arguments by the `parseArgs` function, which handles quotes, escapes, and spaces.
	- The shell first checks for built-in commands (`echo`, `cd`, `pwd`, `history`, `type`, and `exit`).  
	- If a command is not a built-in, it looks for the executable in the system `PATH` using `isInPath` and executes it via `execPathCmd`.

- **Redirection:**  
	The `fileForRedirect` function inspects the arguments for redirection operators (such as `>`, `2>`, `>>`, `2>>`) and, if found, the output is written using `writeResultToFile`.  
	- `>` or `1>` writes (or overwrites) `stdout` to file mentioned  
	- `>>` or `1>>` appends `stdout` to file mentioned  
	- `2>` appends `stderr` to file mentioned  
	- `2>>` appends `stderr` to file mentioned  
 
### Raw Terminal Input and Line Editing (`terminal_read_line.go`)

- **Raw Mode:**  
  The terminal is switched to raw mode (using `golang.org/x/term`) so that the shell can handle each keypress immediately.

- **Input Handling:**  
	Characters are read one-by-one. Special handling is provided for:
	- **Tab (`\t`):**  
		Triggers auto-completion by determining the last word (using `calculateLastWord`), searching the prefix tree (from `auto_completion.go`), and either autocompleting directly or listing possible completions.
	- **Backspace (`\b` or ASCII 127):**  
		Removes the last character from the input and updates the screen (using ANSI escape sequences to move the cursor and clear characters).
	- **Ctrl+C (ASCII 3):**  
		Returns an error (`SIGINT`), which in the REPL loop results in an exit with code 130.
	- **Newline/Carriage Return (`\n`, `\r`):**  
		Ends the current input line and returns the collected input.
	- **Arrow Keys:**  
		Supports up/down navigation for command history.
		
		```
		NOTE: Arrow keys and others are not single bytes but multiple
		
		Up:    ASCII 27 + 91 + 65
		Down:  ASCII 27 + 91 + 66
		Right: ASCII 27 + 91 + 67
		Left:  ASCII 27 + 91 + 68
		```

- **Utility Functions:**  
	- `echoLetterAndAppendToBuffer` prints a typed character and appends it to the input buffer.
	- `redrawBuffer` clears the current input area and redraws the updated buffer when needed (e.g., after a deletion or history navigation).

### Auto-Completion (`auto_completion.go`)

- **Prefix Tree (Trie) Construction:**  
	The prefix tree is built in `buildAutocompletionDB` from both the list of built-in commands and external commands pulled from the directories listed in the system’s `PATH` (via `getCommandsFromPath` and `getCommandsFromDir`).

- **Insertion and Searching:**  
	- `addToPrefixTree` inserts each command into the tree.
	- `searchPrefixTree` walks the tree based on the current input prefix to find matching commands.
	- `getAllChildrenAsList` recursively collects all completions from a given node.

- **Common Denominator:**  
	The function `getCompletionDenominator` is used to identify and auto-complete the common prefix among multiple completions as a partial completion.

### History Handling (`history.go`)

- **History Storage:**  
	A simple array stores the history entries (each containing the full command string and an identifier).

- **Updating History:**  
	The `addToHistory` function appends new commands to the history, to be later navigated using the arrow keys in the raw input mode or listed with the built in history command.

## Overall Workflow

1. **Start-up:**  
	The shell initializes the working directory for `pwd`, builds the auto-completion database, and initializes the history array.
2. **REPL Execution:**  
	The main loop in `main.go` prints the prompt, calls `terminalReadLine` to gather a full user command, and then parses and executes that command.
3. **Command Processing:**  
	- Built-in commands are processed internally.
	- External commands are located (using the PATH) and executed.
	- Redirection operators cause the output/error to be written to the specified file.
4. **Cleanup:**  
	Upon exiting (or on receiving a signal like `Ctrl+C`), the shell returns the appropriate exit code.