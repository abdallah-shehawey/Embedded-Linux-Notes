# Bash Scripting Basics

A guide explaining the fundamental concepts of Bash scripting.

### Shebang

Before anything else, you should start your script with a **Shebang**. This line tells the system which interpreter to use to execute the file.

For example, to use Bash:

```bash
#!/bin/bash
```

### Defining Variables

**Local Variable** A variable that is only available within the script where it is defined.

```bash
VAR="Value"
```

**Global Variable** To make a variable available to any sub-shells or child processes, you must `export` it.

```bash
export VAR="Value"
```

### Running a Bash Script

There are three common ways to execute a Bash script:

1. **Make the file executable** This method requires the script to have a shebang.
    
    ```bash
    # 1. Add execute permissions to the file
    chmod +x your_script_name.sh
    
    # 2. Run the script
    ./your_script_name.sh
    ```
    
2. **Using the `bash` command** This runs the script in a new sub-shell. Environment variables changed by the script will **not** affect your current shell.
    
    ```bash
    bash your_script_name.sh
    ```
    
3. **Using the `source` command** This runs the script in the _current_ shell. Any environment variables changed by the script **will** affect your current shell.
    
    ```bash
    source your_script_name.sh
    ```
    

### User Input

You can get input from the user with the `read` command.

**Reading Input** The `-p` flag is used to display a prompt message without a newline.

```bash
read -p "Please enter your name: " user_name
```

**Printing Output** Use the `echo` command to print the variable's value. You must prefix the variable name with a `$`.

```bash
# Print just the variable
echo $user_name

# or print it within a string
echo "Hello, $user_name!"
or
echo "Hello, ${user_name}" #commonly used and modern way
```


### Command-Line Arguments

You can pass arguments to your script when you run it from the command line.

For example:

```bash
./your_script.sh argument1 "argument 2"
```

Inside the script, you can access these arguments using special variables:

- `$#`: Stores the total number of arguments.
    
    ```bash
    # This command prints the number of arguments
    echo "You provided $# arguments."
    ```
    
- `$0`: The name of the script itself.
    
- `$1`, `$2`, etc.: The arguments themselves. `$1` is the first argument, `$2` is the second, and so on.
    
    ```bash
    # This command prints the first argument
    echo "The first argument is: $1"
    ```
    

### Quoting

The difference between single and double quotes is very important in Bash.

1. **Single Quotes (`'`)** Everything inside single quotes is treated as a literal string. No variable expansion or command substitution will occur.
    
    ```bash
    VAR="World"
    echo 'Hello, $VAR'
    # Output: Hello, $VAR
    ```
    
2. **Double Quotes (`"`)** Double quotes allow for variable expansion and command substitution. Special characters are interpreted.
    
    ```bash
    VAR="World"
    echo "Hello, $VAR"
    # Output: Hello, World
    ```
    
    You can also run commands inside double quotes using `$(...)`:
    
    ```bash
    # This will print "test " followed by the output of the ls command
    echo "test $(ls)"
    ```
    
    To print a special character literally inside double quotes, you can escape it with a backslash (`\`):
    
    ```bash
    # This will print "hello $(ls)" literally
    echo "hello \$(ls)"
    ```
    

### Word Splitting

When a variable contains spaces and is used without quotes, Bash performs "word splitting" and treats each word as a separate argument.

```bash
VARA=Hello
VARB="Hello There"

# This will create one directory named "Hello"
mkdir $VARA

# This will create TWO directories: one named "Hello" and one named "There"
mkdir $VARB

# To treat the variable's value as a single argument, enclose it in double quotes:
mkdir "$VARB" # This creates one directory named "Hello There"
```

### Arithmetic Operations

To perform math in Bash, use the `$((...))` arithmetic expansion syntax.

```bash
x=10
y=5

# Addition
echo $((x + y)) # Output: 15

# Subtraction
echo $((x - y)) # Output: 5

# Multiplication
echo $((x * y)) # Output: 50

# Division
echo $((x / y)) # Output: 2

# Power / Exponentiation
echo $((y ** 2)) # Output: 25

# Incrementing a variable
((x++))
echo $x # Output: 11
```

The following operators are available inside `((...))`: `+`, `-`, `*`, `/`, `%` (modulo), `**` (exponentiation), `++`, `--`, `+=`, `-=`, `==` (equality), `!=` (not equal), `&&` (logical AND), `||` (logical OR).