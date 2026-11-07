# How Sushi runs a quoted argument

I worked on Sushi in my operating systems course at Suffolk. Here’s a short transcript from a local build. The angle brackets make it easy to see exactly what the program received.

```console
sushi> printf '<%s>\n' "two words"
<two words>
```

**At the prompt.** Flex recognizes `"two words"` as one token and removes the surrounding quotes. Bison puts it into the argument list alongside the command and the format string. The space between the words survives.

**In the child process.** The shell calls `fork()`. In the child, `execvp()` looks up `printf` and runs it with that argument list. The parent waits with `waitpid()` before displaying the next prompt.

You can try the same example in a Unix terminal. Remove the double quotes around `two words` and `printf` will print two lines, one for each argument. That small change at the prompt is enough to see what the lexer has to preserve.

[Back to selected projects](../README.md#selected-projects)
