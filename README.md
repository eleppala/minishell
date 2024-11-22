# MINISHELL
![gif](incs/funny.gif)

* [Overview](#overview)
* [Functionality](#functionality)
  * [Summary](#summary)
  * [Tokens, Parsing and Expancion](#tokens-parsing-and-expancion)
  * [Execution](#execution)
  * [Pipes & Redirection](#pipes-and-redirections)
  * [Signals](#signals)
  * [Memory & Error management](#memory-management--error-handling)
* [Possible Improvements](#possible-improvements)
* [Usage](#possible-improvements)
* [Conclusion](#conclusion)
* [Contributors](#contributors)

## OVERVIEW
Minishell is a command-line interpreter mimicking the basic functionality of Bash. This project was a team effort, done together with [eleppala](https://github.com/eleppala). It deepened our understanding of shell internals, command execution, environment handling, and other core operating system concepts.

## FUNCTIONALITY
### Summary
* Displays a prompt while waiting for a new command, has a working history, and finds & launches the correct executable based on the PATH environment variable (both with relative and absolute paths).
* Working history
* Doesn't interpret unclosed quotes or special characters like '\' or ';'.
* Builtin commands:
  * cd, echo (-n), pwd, env, export, unset, exit
* Signal handling: (in interactive mode)
 * ctrl -C displays a new prompt on a new line
 * ctrl-D exits the shell (EOF)
 * ctrl-\ does nothing
* Quote handling:
  * Single quotes to prevent interpreting the metacharacters in the quoted sequence.
  * Double quotes to prevent the shell from interpreting the metacharacters in the quoted sequence except for $ (dollar sign).
* Redirection handling:
  * '<' to redirect input.
  * '>' to redirect output.
  * '<<' with delimiter to open a heredoc.
  *  '>>' to redirect output in append mode.
* Pipe handling:
  * The output of each command in the pipeline is connected to the input of the next command via a pipe.
* Environment Variables:
  * Handle environment variables ('$' followed by a sequence of characters) which should expand to their assigned values.
* '$?':
  * Handle '$?' to expand to the exit status of the most recently executed foreground pipeline.

### Tokens, Parsing and Expancion
* Tokenizer: Splits input based on spaces while categorizing the tokens created. It also takes into account quoted strings and escaped characters.

* Parsing: Formats the tokens into an executable form and provides information on possible redirections and pipes in the given command.
  * During parsing, tokens are expanded to their corresponding values as needed.
  * Environment Variables: These are managed and expanded during command execution. We implemented checks to handle cases where environment variable values contain spaces, ensuring they are split into multiple arguments as required.

### Execution
* For valid paths, execve() runs the command. Otherwise, the shell checks and handles builtins and/or errors.
* Commands without paths are verified for accessibility, and execution is attempted unless the command is a directory.
* The shell handles child processes through forking, signal management, and captures exit statuses to update the shell's state.

### Pipes and Redirections
* Our shell supports piping the output of one command into another, handling heredocs using <<, and redirecting input/output using <, >, and >>.
  * Achieved by managing file descriptors and ensuring that child processes inherit the correct input/output streams.

### Signals
* Signals such as Ctrl-C and Ctrl-D are correctly handled to mimic Bash behavior, whether they occur in a heredoc, a child process, or the parent process. SIGQUIT is ignored.

### Memory Management & Error Handling
* We ensured proper memory allocation and deallocation throughout the shell's lifecycle.
  * Dynamic strings and split tokens are cleaned up after each command execution, and unexpected disruptions are handled at any point during the program's lifecycle.

## USAGE
* Make (all/clean/fclean/re)
`./minishell`

## FUTURE IMPROVEMENTS
* Enhanced handling of special characters and escape sequences
* More comprehensive signal handling
* Udating shell level (SHLVL) environment variable

## CONCLUSION
This project was the first major team effort and the largest project so far for both of us. Working on our project was an invaluable learning experience, which significantly enhanced our understanding of Unix shell internals, project management, and collaboration.

Despite the still remaining areas for improvement, the project allowed us to gain a solid foundation in shell functionality and created a reliable command-line interface that mirrors some of Bash's key features.

## Contributors
* [eleppala](https://github.com/eleppala)
* [asalo](https://github.com/enduserrr)
