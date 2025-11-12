<!---
{
  "id": "e2b7c9f1-4d2a-4a3e-9f1b-2c3d4e5f6a7b",
  "teaches": "Storing Functions",
  "depends_on": [],
  "author": "Exercise Sheet Assistant",
  "first_used": "2025-05-08",
  "keywords": ["bash", "source", "functions", "shell scripting", "redirection"]
}
--->

# Storing Functions

> In this exercise, you will learn how to define and persist shell functions by redirecting them into a file and sourcing them in new sessions. Furthermore, we will explore how to use the `>` and `>>` operators for file redirection, understand their differences, and recognize common pitfalls associated with their use.

---

## Introduction

In Unix-like systems, the `source` command (or its shorthand `.`) is a powerful tool that allows you to execute commands from a file within the current shell environment. This means that any functions, variables, or configurations defined in the sourced file become immediately available in your current session.

A session refers to a running instance of the shell—your current command-line interface environment. Within this session, you can define identifiers such as functions and variables, and assign them values. However, these identifiers and their values are held only in memory, meaning they are temporary. When you close the session, everything stored in memory is lost. This impermanence is critical: shell sessions do not automatically preserve your work across restarts.

Historically, due to the lack of convenient text editors or graphical interfaces, users had to rely on direct command-line input to define functions or setup configurations. To avoid repeating these definitions in every session, users would "store" their work—function definitions, variable exports, configuration setups—in text files. These files could then be sourced to "load" the necessary components into memory whenever needed.

Thus, there is a crucial distinction between **memory**, where function and value identifiers are remembered only during a session, and **storage**, which refers to the persistent archiving of these definitions in files for reuse across sessions.

By mastering these concepts, you'll be equipped to manage your shell environment more effectively, automate tasks, and understand the underlying mechanisms of shell scripting.

### Further Readings and Other Sources

* [Bash Source Command - Linuxize](https://linuxize.com/post/bash-source-command/)
* [How To Use the Bash Source Command And Why - NameHero](https://www.namehero.com/blog/how-to-use-the-bash-source-command-and-why/)
* [Include Files in a Bash Shell Script With source Command - Baeldung](https://www.baeldung.com/linux/source-include-files)
* [Source files in a bash script - Stack Overflow](https://stackoverflow.com/questions/16011245/source-files-in-a-bash-script)
* [Source Command in Linux with Examples | GeeksforGeeks](https://www.geeksforgeeks.org/source-command-in-linux-with-examples/)

---

## Tasks

### 1. Define a Function and Store It in a File

First, you'll simulate an environment where you have no access to a text editor and must create a shell function using terminal commands only.

1. Open your terminal.

2. Define a function called `greet` that takes one argument and prints a greeting. This function will also export a variable named `LAST_GREETING`:

   ```bash
   echo 'greet() {
       export LAST_GREETING="Hello, $1!"
       echo "$LAST_GREETING"
   }' > my_functions.sh
   ```

   This command writes the function definition into `my_functions.sh`. If the file already exists, it will be overwritten. You have now stored your function definition for future use.

3. Confirm the contents of the file:

   ```bash
   cat my_functions.sh
   ```

### 2. Source the Stored Function in a New Session

You now want to use the stored function in a fresh shell environment.

1. Open a new shell session:

   ```bash
   bash
   ```

2. Source your `my_functions.sh` to load the function and environment variable:

   ```bash
   source my_functions.sh
   ```

3. Call the function:

   ```bash
   greet "World"
   echo "$LAST_GREETING"
   ```

   You should see the greeting and the exported variable value printed.

4. Exit the session:

   ```bash
   exit
   ```

### 3. Append Another Function to the Stored File Using `>>`

Now you’ll add more functionality without erasing what's already in the file.

1. Append a `farewell` function that also exports a variable:

   ```bash
   echo 'farewell() {
       export LAST_FAREWELL="Goodbye, $1!"
       echo "$LAST_FAREWELL"
   }' >> my_functions.sh
   ```

2. Inspect the file:

   ```bash
   cat my_functions.sh
   ```

   It should now contain both `greet` and `farewell` functions.

3. Source and test the new function:

   ```bash
   source my_functions.sh
   farewell "Alice"
   echo "$LAST_FAREWELL"
   ```

### 4. Experimenting with Overwrite and Append

Let’s demonstrate the risks and differences of using `>` versus `>>`.

1. Overwrite the file (dangerous if not careful):

   ```bash
   echo 'greet() {
       export LAST_GREETING="Hi again, $1!"
       echo "$LAST_GREETING"
   }' > my_functions.sh
   ```

2. Check the file contents:

   ```bash
   cat my_functions.sh
   ```

   The `farewell` function is now gone.

3. Re-add it properly with `>>`:

   ```bash
   echo 'farewell() {
       export LAST_FAREWELL="Bye again, $1!"
       echo "$LAST_FAREWELL"
   }' >> my_functions.sh
   ```

---

## Questions

1. What is the primary purpose of the `source` command in Bash?
2. How do the `>` and `>>` operators differ in behavior and use?
3. What are the consequences of using `>` on an existing file?
4. Why is using `export` important in sourced shell functions?
5. How can you prepare a file of shell functions for repeated use in future sessions?
6. What is the difference between memory and storage in the context of shell scripting?

---

## Advice

When working in constrained environments or scripting setups where a text editor isn't viable, using echo and redirection can be a powerful workaround. It enforces good discipline in file management and deepens your understanding of shell behavior. Remember to use `>>` to avoid losing work, and always inspect your files before sourcing them. Exporting variables makes them available outside the function scope, which is essential for passing data within a session.

It’s vital to distinguish between temporary memory, which vanishes with your session, and persistent storage, which enables lasting reuse. Think of sourcing as a way to memorize a set of tools from a stored archive. This approach, while old-school, remains a valuable technique in many real-world scenarios.

In fact, with just these basic techniques—defining functions, storing them in files, exporting variables, and sourcing them—you already have the fundamental tools needed to build virtually everything modern computing systems offer. While the lack of abstraction and tooling makes this extremely uncomfortable for large-scale development, the conceptual power is there. You're essentially manipulating a live programmable environment with logic, memory, and storage—just at the most elemental level.

For continued practice, explore related sheets like \[managing\_shell\_environments.md] for deeper insight into persistent session setups and environment management.
