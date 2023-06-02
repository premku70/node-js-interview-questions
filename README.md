# node-js-interview-questions
Node js ,express js
### Table of Contents

| No. | Questions                                                                                                                                                                                                                        |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     | **Core Nodejs**                                                                                                                                                                                                                   |
| 1   | [What are node js child process?](# What are node js child process)                                                                                                                                                                                                 |
                                                                                                                        |


## Core Nodejs

1.  ### What are node js child process?

In Node.js, the `child_process` module provides functionality to spawn child processes and communicate with them. This module allows you to run external commands or scripts from your Node.js application and interact with their input/output streams.

There are several ways to create child processes in Node.js using the `child_process` module. Here are the main methods:

1. `spawn()`: This method launches a command in a new process and provides a stream-based interface for communication with the child process. It is suitable for long-running processes and streaming large amounts of data. Here's an example:

```javascript
const { spawn } = require('child_process');

const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

ls.on('close', (code) => {
  console.log(`child process exited with code ${code}`);
});
```

2. `exec()`: This method runs a command in a shell and buffers the output for you to access. It is suitable for simple commands and smaller outputs. Here's an example:

```javascript
const { exec } = require('child_process');

exec('ls -lh /usr', (error, stdout, stderr) => {
  if (error) {
    console.error(`exec error: ${error}`);
    return;
  }

  console.log(`stdout: ${stdout}`);
  console.error(`stderr: ${stderr}`);
});
```

3. `execFile()`: This method is similar to `exec()`, but it does not spawn a shell. Instead, it directly executes the specified file. It is suitable for running executable files. Here's an example:

```javascript
const { execFile } = require('child_process');

const child = execFile('node', ['--version'], (error, stdout, stderr) => {
  if (error) {
    console.error(`execFile error: ${error}`);
    return;
  }

  console.log(`stdout: ${stdout}`);
  console.error(`stderr: ${stderr}`);
});
```

 Here are some more features and methods you can use:

1. Communication with Child Processes:
   - `stdin`: You can write data to the standard input of a child process using the `stdin` stream. For example:
     ```javascript
     const { spawn } = require('child_process');
     const grep = spawn('grep', ['hello']);

     grep.stdin.write('hello world\n');
     grep.stdin.end();
     ```

2. Event Handling:
   - `exit`: The `exit` event is emitted when a child process exits. You can listen for this event to perform any necessary cleanup or handle the process termination. For example:
     ```javascript
     const { spawn } = require('child_process');
     const child = spawn('ls', ['-lh', '/usr']);

     child.on('exit', (code, signal) => {
       console.log(`child process exited with code ${code} and signal ${signal}`);
     });
     ```

3. Shell Syntax:
   - If you want to use shell syntax in your command, you can use the `shell` option in the `exec()` and `execFile()` methods. This allows you to execute complex commands or use shell-specific features. For example:
     ```javascript
     const { exec } = require('child_process');

     exec('echo "Hello, world!"', { shell: '/bin/bash' }, (error, stdout, stderr) => {
       // Handle output and errors
     });
     ```

4. Working Directory:
   - By default, child processes are executed in the current working directory of the parent process. You can change this by providing the `cwd` (current working directory) option when spawning a child process. For example:
     ```javascript
     const { spawn } = require('child_process');
     const child = spawn('ls', ['-lh'], { cwd: '/path/to/directory' });
     ```

These are some additional features and options available in the `child_process` module. They provide more control over the execution and interaction with child processes in your Node.js applications.

    **[⬆ Back to Top](#table-of-contents)**



---
