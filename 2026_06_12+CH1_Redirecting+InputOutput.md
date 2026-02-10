# 🐧 Mastering I/O Redirection: The Path to the Top 1%

---

# 1. Core Concepts: Standard Input, Output, and Error

In the Unix theme of "everything is a file," programs usually produce two types of output and take one type of input.

*   **Standard Output (stdout - File Descriptor 1):** The program's results. By default, this is linked to the screen.
*   **Standard Error (stderr - File Descriptor 2):** Status and error messages. By default, this is also linked to the screen.
*   **Standard Input (stdin - File Descriptor 0):** Input provided to the program. By default, this is attached to the keyboard.

---

# 2. Redirecting Output (stdout)

We can use I/O redirection to send results to files instead of the screen.

## 2.1. The `>` Operator (Overwrite)
Use `>` to redirect standard output to a file.

**⚠️ WARNING:** If the file already exists, this operator **rewrites** it from the beginning (truncates it to zero length).

```bash
ls -l /usr/bin > ls-output.txt
```
Pro Trick: You can use this to truncate a file or create a new, empty file by providing no command:
```bash
> ls-output.txt
```
The >> Operator (Append)
To avoid deleting existing content, use >>. This appends the new output to the end of the file.
```bash
ls -l /usr/bin >> ls-output.txt
ls -l /usr/bin >> ls-output.txt
```
**The file now contains the listing repeated three times.**

--------------------------------------------------------------------------------
## 2.2. Redirecting Error (stderr)
If you redirect only stdout using >, error messages will still appear on your screen. This is because stdout (1) and stderr (2) are different streams.
To capture errors, you must refer to the File Descriptor 2:
```bash
ls -l /bin/usr 2> ls-error.txt
```
Redirecting Both (stdout + stderr)
To capture everything (results and errors) in a single file:
1. Traditional Method (Order is Critical): Redirect output first, then redirect stderr (2) to stdout (1).
2. Note: If you reverse the order (2>&1 >output.txt), stderr will still go to the screen,.
3. Modern Bash Method: A streamlined notation to redirect both streams at once.
The Bit Bucket: /dev/null
Sometimes "silence is golden." If you want to throw away output (specifically errors), redirect them to the system's "bit bucket":
```bash
ls -l /bin/usr 2> /dev/null
```
--------------------------------------------------------------------------------
### 2.2. Redirecting Input (stdin)
Commands like cat normally read from the keyboard (stdin) if no file arguments are given.
The cat Command
• Concatenate: Joins files together.
• Create Files: Type 
```bash 
cat > file.txt 
```
type your text, and hit Ctrl-D (EOF) to save.

• Redirect Input: The < operator changes the source of standard input from the keyboard to a file.

--------------------------------------------------------------------------------
### 3. Pipelines: The Real Power (|)

The pipe operator | connects the standard output of one command to the standard input of another.
command1 | command2
🛑 CRITICAL: The Difference Between `>` and `|`
• `>` connects a command to a file.
• `|` connects a command to another command.
The "Server Destroyer" Mistake: Do not confuse them. A user once typed ls > less inside /usr/bin. This overwrote the less executable binary with the text output of ls, destroying the system command. Treat redirection with respect,.

--------------------------------------------------------------------------------
### 4. Essential Filters and Tools
Filters are commands used in pipelines to take data, transform it, and output it.
### sort and uniq
• `sort`: Arranges lines of text.
• `uniq`: Removes duplicate lines from a sorted list.
	◦ `uniq -d`: Shows only the duplicate lines.
### wc (Word Count)
Counts lines, words, and bytes.
• `wc -l`: Counts lines. Useful for counting items in a list.
### grep
Prints lines matching a pattern.
• `-i`: Ignore case (case insensitive).
• `-v`: Invert match (print lines that do not match).
### head and tail
View the start or end of a stream.
• `head -n 5`: First 5 lines.
• `tail -n 5`: Last 5 lines.
• `tail -f`: Real-time monitoring. usage: tail -f /var/log/messages. It watches the file and prints new lines as they are written,.
### tee
The plumbing fitting. It reads stdin and copies it to both stdout and a file. Great for capturing intermediate stages of a pipeline without breaking the flow,.
```bash
ls /usr/bin | tee ls.txt | grep zip
```
--------------------------------------------------------------------------------
### 5. Final Philosophy
Why is this the Top 1% mindset?
Windows is like a Game Boy: It's shiny, but you can only play the games provided. You cannot change how it works because it's sealed shut.
Linux is like the world's largest Erector Set: It is a collection of parts (struts, screws, motors). You can build whatever you want. The system takes on the shape of your imagination.
Master I/O redirection, and you stop playing the game—you start building the system.
