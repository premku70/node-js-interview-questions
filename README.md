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

In Node.js, the `child_process` module provides functionality to spawn child processes and communicate with them. 
This module allows you to run external commands or scripts from your Node.js application and interact with their input/output streams.

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



    **[⬆ Back to Top](#table-of-contents)**

2. ### What is Fs Module?
                                                                                                                     |                                              In Node.js, the `fs` module is built-in and provides an API for interacting with the file system. 
It allows you to perform various file-related operations, such as reading from and writing to files,creating and deleting files and directories,and modifying file permissions.
Here's an overview of some common operations you can perform using the `fs` module:

1. Reading Files:
   - `fs.readFile()`: Reads the contents of a file asynchronously. It takes the file path and an optional encoding as parameters. Example:
     ```javascript
     const fs = require('fs');

     fs.readFile('file.txt', 'utf8', (err, data) => {
       if (err) throw err;
       console.log(data);
     });
     ```

   - `fs.readFileSync()`: Reads the contents of a file synchronously. It takes the file path and an optional encoding as parameters. Example:
     ```javascript
     const fs = require('fs');

     try {
       const data = fs.readFileSync('file.txt', 'utf8');
       console.log(data);
     } catch (err) {
       console.error(err);
     }
     ```

2. Writing Files:
   - `fs.writeFile()`: Writes data to a file asynchronously, overwriting the file if it already exists. It takes the file path, data to be written, and an optional encoding as parameters. Example:
     ```javascript
     const fs = require('fs');

     fs.writeFile('file.txt', 'Hello, world!', (err) => {
       if (err) throw err;
       console.log('File written successfully.');
     });
     ```

   - `fs.writeFileSync()`: Writes data to a file synchronously, overwriting the file if it already exists. It takes the file path, data to be written, and an optional encoding as parameters. Example:
     ```javascript
     const fs = require('fs');

     try {
       fs.writeFileSync('file.txt', 'Hello, world!');
       console.log('File written successfully.');
     } catch (err) {
       console.error(err);
     }
     ```

3. File and Directory Operations:
   - `fs.unlink()`: Deletes a file asynchronously. Example:
     ```javascript
     const fs = require('fs');

     fs.unlink('file.txt', (err) => {
       if (err) throw err;
       console.log('File deleted successfully.');
     });
     ```

   - `fs.readdir()`: Reads the contents of a directory asynchronously. Example:
     ```javascript
     const fs = require('fs');

     fs.readdir('/path/to/directory', (err, files) => {
       if (err) throw err;
       console.log(files);
     });
     ```

   - `fs.mkdir()`: Creates a directory asynchronously. Example:
     ```javascript
     const fs = require('fs');

     fs.mkdir('newDir', (err) => {
       if (err) throw err;
       console.log('Directory created successfully.');
     });
     ```

   - `fs.rmdir()`: Deletes a directory asynchronously. Example:
     ```javascript
     const fs = require('fs');

     fs.rmdir('dirToDelete', (err) => {
       if (err) throw err;
       console.log('Directory deleted successfully.');
     });
     ```

4. File Metadata and Information:
   - `fs.stat()`: Retrieves information about a file asynchronously, such as size, permissions, and timestamps. Example:
     ```javascript
     const fs = require('fs');

     fs.stat('file.txt', (err, stats) => {
       if (err) throw err;
       console.log(stats);
     });
     ```

   - `fs.existsSync()`: Checks if a file or directory

 exists synchronously. Example:
     ```javascript
     const fs = require('fs');

     if (fs.existsSync('file.txt')) {
       console.log('File exists.');
     } else {
       console.log('File does not exist.');
     }
     ```


5. Renaming and Moving Files:
   - `fs.rename()`: Renames a file or moves it to a different location asynchronously. Example:
     ```javascript
     const fs = require('fs');

     fs.rename('oldFile.txt', 'newFile.txt', (err) => {
       if (err) throw err;
       console.log('File renamed successfully.');
     });
     ```

6. File Permissions:
   - `fs.chmod()`: Changes the permissions of a file asynchronously. Example:
     ```javascript
     const fs = require('fs');

     fs.chmod('file.txt', 0o755, (err) => {
       if (err) throw err;
       console.log('File permissions changed successfully.');
     });
     ```

7. File Streams:
   - `fs.createReadStream()`: Creates a readable stream to read data from a file. Example:
     ```javascript
     const fs = require('fs');

     const readableStream = fs.createReadStream('file.txt', 'utf8');
     readableStream.on('data', (chunk) => {
       console.log(chunk);
     });
     ```

   - `fs.createWriteStream()`: Creates a writable stream to write data to a file. Example:
     ```javascript
     const fs = require('fs');

     const writableStream = fs.createWriteStream('output.txt', 'utf8');
     writableStream.write('Hello, world!');
     writableStream.end();
     ```

8. File System Events:
   - `fs.watch()`: Watches for changes in a file or directory and emits events when they occur. Example:
     ```javascript
     const fs = require('fs');

     fs.watch('file.txt', (event, filename) => {
       console.log(`Event: ${event}`);
       console.log(`Filename: ${filename}`);
     });
     ```

9. File Path Operations:
   - `fs.join()`: Joins multiple path segments into a single path string. Example:
     ```javascript
     const fs = require('fs');
     const path = require('path');

     const filePath = path.join(__dirname, 'files', 'file.txt');
     ```

   - `fs.resolve()`: Resolves a sequence of paths or path segments into an absolute path. Example:
     ```javascript
     const fs = require('fs');
     const path = require('path');

     const absolutePath = path.resolve('files', 'file.txt');
     ```

10. File Read/Write Options:
   - `fs.readFile()` and `fs.readFileSync()`:
     - You can pass an object as an optional parameter to specify additional options for reading files, such as the `flag` to control file opening behavior.
     - Example:
       ```javascript
       const fs = require('fs');

       fs.readFile('file.txt', { encoding: 'utf8', flag: 'r' }, (err, data) => {
         if (err) throw err;
         console.log(data);
       });
       ```

   - `fs.writeFile()` and `fs.writeFileSync()`:
     - You can pass an object as an optional parameter to specify additional options for writing files, such as the `mode` to set the file mode/permissions.
     - Example:
       ```javascript
       const fs = require('fs');

       fs.writeFile('file.txt', 'Hello, world!', { mode: 0o644 }, (err) => {
         if (err) throw err;
         console.log('File written successfully.');
       });
       ```

11. File System Constants:
   - The `fs` module provides several constants that represent file system-related values, such as file access modes, file types, and file system error codes.
   - Example:
     ```javascript
     const fs = require('fs');

     console.log(fs.constants.S_IRWXU); // File access mode constant
     console.log(fs.constants.S_IFREG); // File type constant
     console.log(fs.constants.ENOENT); // Error code constant
     ```

12. Directory Operations:
   - `fs.readdirSync()`: Reads the contents of a directory synchronously and returns an array of file names.
   - Example:
     ```javascript
     const fs = require('fs');

     const files = fs.readdirSync('/path/to/directory');
     console.log(files);
     ```

   - `fs.mkdtemp()`: Creates a temporary directory asynchronously and generates a unique temporary directory path.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.mkdtemp('/tmp/test-', (err, folder) => {
       if (err) throw err;
       console.log(`Temporary directory created: ${folder}`);
     });
     ```

13. File Watching Options:
   - `fs.watch()`:
     - You can pass options to customize the behavior of the `fs.watch()` method, such as the `persistent` option to control if the watcher stays active after an event is emitted.
     - Example:
       ```javascript
       const fs = require('fs');

       fs.watch('file.txt', { persistent: false }, (event, filename) => {
         console.log(`Event: ${event}`);
         console.log(`Filename: ${filename}`);
       });
       ```

14. File System Promises:
   - Starting from Node.js 14, the `fs` module provides experimental promises-based versions of its functions. You can use `fs.promises` to access the promise-based versions.
   - Example:
     ```javascript
     const fs = require('fs').promises;

     fs.readFile('file.txt', 'utf8')
       .then((data) => {
         console.log(data);
       })
       .catch((err) => {
         console.error(err);
       });
     ```


15. File Watching:
   - `fs.watchFile()`: Watches a file for changes by periodically polling its metadata. It emits a `change` event when the file is modified.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.watchFile('file.txt', (curr, prev) => {
       console.log('File modified.');
       console.log('Current file stats:', curr);
       console.log('Previous file stats:', prev);
     });
     ```

16. File Descriptors:
   - `fs.open()`: Opens a file and returns a file descriptor, which is a unique identifier for the opened file. You can use the file descriptor with other `fs` methods to perform operations on the file.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.open('file.txt', 'r', (err, fd) => {
       if (err) throw err;

       // Perform operations using the file descriptor (fd)
       // ...

       fs.close(fd, (err) => {
         if (err) throw err;
         console.log('File closed successfully.');
       });
     });
     ```

17. File Truncation:
   - `fs.truncate()`: Truncates a file to a specified length asynchronously. If the file is larger than the specified length, it is truncated to the given size. If the file is smaller, its size remains unchanged.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.truncate('file.txt', 100, (err) => {
       if (err) throw err;
       console.log('File truncated successfully.');
     });
     ```

18. Symbolic Links:
   - `fs.symlink()`: Creates a symbolic link asynchronously. You can create either a file or directory symbolic link.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.symlink('/path/to/target', '/path/to/symlink', 'file', (err) => {
       if (err) throw err;
       console.log('Symbolic link created successfully.');
     });
     ```

19. File Compression:
   - The `fs` module provides methods to work with compressed files using zlib and other compression libraries.
   - Example:
     ```javascript
     const fs = require('fs');
     const zlib = require('zlib');

     const readStream = fs.createReadStream('file.txt');
     const writeStream = fs.createWriteStream('compressed.gz');
     const gzip = zlib.createGzip();

     readStream.pipe(gzip).pipe(writeStream);
     ```

20. File Permissions:
   - `fs.access()`: Tests the user's permissions for a file or directory asynchronously.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.access('file.txt', fs.constants.R_OK | fs.constants.W_OK, (err) => {
       if (err) throw err;
       console.log('File can be read and written successfully.');
     });
     ```


1. File Watching with Recursion:
   - `fs.watch()`: By default, `fs.watch()` does not watch subdirectories. However, you can enable recursive watching by passing the `recursive: true` option. This allows you to monitor changes in subdirectories as well.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.watch('/path/to/directory', { recursive: true }, (event, filename) => {
       console.log(`Event: ${event}`);
       console.log(`Filename: ${filename}`);
     });
     ```

2. File Stats:
   - `fs.stat()`: Returns an instance of the `fs.Stats` class, which provides information about a file or directory.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.stat('file.txt', (err, stats) => {
       if (err) throw err;

       console.log('File size:', stats.size);
       console.log('Is a directory?', stats.isDirectory());
       console.log('Is a file?', stats.isFile());
       console.log('File permissions:', stats.mode);
       console.log('Last modified:', stats.mtime);
     });
     ```

3. File Read/Write Buffers:
   - `fs.readFile()` and `fs.writeFile()`: Instead of specifying an encoding, you can pass a buffer to read from or write to files.
   - Example:
     ```javascript
     const fs = require('fs');

     fs.readFile('file.txt', (err, data) => {
       if (err) throw err;

       const buffer = Buffer.from(data);
       console.log('File content as buffer:', buffer);
     });

     const buffer = Buffer.from('Hello, world!');
     fs.writeFile('file.txt', buffer, (err) => {
       if (err) throw err;
       console.log('File written successfully.');
     });
     ```

4. File Watching with fs.FSWatcher:
   - `fs.watch()` and `fs.watchFile()` return an instance of the `fs.FSWatcher` or `fs.StatsWatcher` class respectively, allowing you to control and manage the watchers.
   - Example:
     ```javascript
     const fs = require('fs');

     const watcher = fs.watch('file.txt', (event, filename) => {
       console.log(`Event: ${event}`);
       console.log(`Filename: ${filename}`);
     });

     watcher.on('error', (err) => {
       console.error('Watcher error:', err);
     });

     watcher.on('close', () => {
       console.log('Watcher closed.');
     });
     ```

5. File System Promises API:
   - The `fs.promises` API provides promise-based versions of the `fs` methods, allowing you to use promises and async/await syntax instead of callbacks.
   - Example:
     ```javascript
     const fs = require('fs').promises;

     async function readFile() {
       try {
         const data = await fs.readFile('file.txt', 'utf8');
         console.log('File content:', data);
       } catch (err) {
         console.error(err);
       }
     }

     readFile();
     ```

---
3. ### Node js Fork Vs Spwan Vs Exec?

In Node.js, there are three main ways to create child processes: `fork()`, `spawn()`, and `exec()` functions. Each of these methods has its own purpose and use cases. Let's go through each of them:

1. `fork()`: The `fork()` function is used to create a new Node.js process that is a copy of the parent process. It is specifically designed for creating child processes that run JavaScript modules. The `fork()` method allows for communication between the parent and child processes using IPC (Inter-Process Communication) channels, which makes it convenient for building applications that require parallel processing or worker systems.

2. `spawn()`: The `spawn()` function is used to launch a new process in Node.js. It allows you to execute commands in a separate process and provides streaming access to the input/output of the child process. You can use it to execute any command available in your system's shell, such as running executables, scripts, or other CLI tools. The `spawn()` function returns a `ChildProcess` object that represents the spawned process and allows you to interact with it.

3. `exec()`: The `exec()` function is similar to `spawn()` but provides a simpler interface for executing shell commands. It takes a command as a string and runs it in a shell. Unlike `spawn()`, `exec()` buffers the command's output and provides it in a callback function. It is useful for executing single commands and getting the resulting output or handling errors.

To summarize:
- Use `fork()` when you want to create child processes that run JavaScript modules and require communication between the parent and child processes.
- Use `spawn()` when you need to execute commands or scripts and want streaming access to the input/output of the child process.
- Use `exec()` when you want to execute a single command in a shell and retrieve the command's output or handle errors.

Each of these methods has its strengths and use cases, so choose the one that best suits your specific requirements.
 Let's go through an example for each method:

1. `fork()` Example:
   ```javascript
   // parent.js
   const { fork } = require('child_process');

   // Create a child process by forking the current module
   const child = fork('child.js');

   // Send a message to the child process
   child.send({ hello: 'world' });

   // Listen for messages from the child process
   child.on('message', (message) => {
     console.log('Received message from child:', message);
   });
   ```

   ```javascript
   // child.js
   process.on('message', (message) => {
     console.log('Received message from parent:', message);

     // Send a message back to the parent process
     process.send({ greetings: 'Hello from the child' });
   });
   ```

   In this example, the parent process forks the `child.js` module as a separate child process. They communicate by sending messages back and forth using `process.send()` and `process.on('message')`.

2. `spawn()` Example:
   ```javascript
   const { spawn } = require('child_process');

   // Execute the "ls" command to list files in the current directory
   const ls = spawn('ls', ['-l', '-a']);

   // Listen for the output of the command
   ls.stdout.on('data', (data) => {
     console.log(`Output: ${data}`);
   });

   // Listen for any errors that occur during execution
   ls.on('error', (err) => {
     console.error(`Error: ${err.message}`);
   });

   // Listen for the command to exit
   ls.on('close', (code) => {
     console.log(`Command exited with code ${code}`);
   });
   ```

   This example uses `spawn()` to execute the `ls -l -a` command, which lists files in the current directory. It captures the output of the command through the `stdout` stream and handles errors and the command's exit.

3. `exec()` Example:
   ```javascript
   const { exec } = require('child_process');

   // Execute the "echo" command to print a message
   exec('echo Hello, World!', (error, stdout, stderr) => {
     if (error) {
       console.error(`Error: ${error.message}`);
       return;
     }

     console.log(`Output: ${stdout}`);
     console.error(`Error output: ${stderr}`);
   });
   ```

   In this example, `exec()` is used to execute the `echo` command to print a message. The resulting output is captured in the callback function and can be accessed through `stdout`. Any errors or error output from the command are also handled.

Certainly! Here are a few more examples to illustrate the usage of `fork()`, `spawn()`, and `exec()` in different scenarios:

1. `fork()` Example with Event Emitter:
   ```javascript
   // parent.js
   const { fork } = require('child_process');
   const child = fork('child.js');

   // Listen for custom events emitted by the child process
   child.on('customEvent', (data) => {
     console.log('Received custom event data:', data);
   });

   // Send a custom event to the child process
   child.emit('customEvent', { message: 'Hello from parent' });
   ```

   ```javascript
   // child.js
   const EventEmitter = require('events');
   const customEmitter = new EventEmitter();

   // Listen for custom events emitted by the parent process
   customEmitter.on('customEvent', (data) => {
     console.log('Received custom event data:', data);
   });

   // Emit a custom event to the parent process
   customEmitter.emit('customEvent', { message: 'Hello from child' });
   ```

   This example showcases communication between the parent and child processes using custom events. Both the parent and child processes create an event emitter instance and exchange data by emitting and listening to custom events.

2. `spawn()` Example with Capturing Command Output:
   ```javascript
   const { spawn } = require('child_process');

   // Execute the "git" command to fetch a remote repository
   const gitFetch = spawn('git', ['fetch', 'origin']);

   // Capture the output of the command
   let commandOutput = '';
   gitFetch.stdout.on('data', (data) => {
     commandOutput += data;
   });

   // Listen for the command to exit
   gitFetch.on('close', (code) => {
     console.log('Command output:', commandOutput);
     console.log(`Command exited with code ${code}`);
   });
   ```

   This example demonstrates using `spawn()` to execute the `git fetch origin` command to fetch updates from a remote repository. The output of the command is captured and stored in the `commandOutput` variable, which is then logged once the command finishes executing.

3. `exec()` Example with Error Handling:
   ```javascript
   const { exec } = require('child_process');

   // Execute a command that may produce an error
   exec('some-invalid-command', (error, stdout, stderr) => {
     if (error) {
       console.error(`Error: ${error.message}`);
       return;
     }

     console.log(`Output: ${stdout}`);
     console.error(`Error output: ${stderr}`);
   });
   ```

   In this example, `exec()` is used to execute a command (`some-invalid-command`) that is intentionally invalid. Since the command is invalid, an error occurs, and the error message is logged. Additionally, the `stdout` and `stderr` streams are captured and can be accessed for further analysis.

Certainly! Here are a few more examples showcasing the usage of `fork()`, `spawn()`, and `exec()` in different scenarios:

1. `fork()` Example with File System Operations:
   ```javascript
   // parent.js
   const { fork } = require('child_process');
   const child = fork('child.js');

   // Send a file path to the child process
   const filePath = '/path/to/file.txt';
   child.send(filePath);

   // Listen for messages from the child process
   child.on('message', (message) => {
     console.log('Received file contents:', message);
   });
   ```

   ```javascript
   // child.js
   const fs = require('fs');
   process.on('message', (filePath) => {
     fs.readFile(filePath, 'utf8', (err, data) => {
       if (err) {
         console.error(`Error reading file: ${err.message}`);
         return;
       }
       process.send(data);
     });
   });
   ```

   In this example, the parent process forks the `child.js` module. It sends a file path to the child process, which then reads the contents of the file using the `fs.readFile()` method. The child process sends the file contents back to the parent process using `process.send()`.

2. `spawn()` Example with Streaming Data:
   ```javascript
   const { spawn } = require('child_process');

   // Execute a command that generates streaming data
   const command = 'node';
   const args = ['streamingScript.js'];
   const streamingProcess = spawn(command, args);

   // Stream and process the output of the command
   streamingProcess.stdout.on('data', (data) => {
     // Process the data as it comes in
     console.log(`Received data chunk: ${data}`);
   });

   // Listen for the command to exit
   streamingProcess.on('close', (code) => {
     console.log(`Command exited with code ${code}`);
   });
   ```

   In this example, `spawn()` is used to execute a command (`node streamingScript.js`) that generates streaming data. The output is captured through the `stdout` stream, and you can process it as data chunks arrive. You can perform real-time operations on the received data.

3. `exec()` Example with Environment Variables:
   ```javascript
   const { exec } = require('child_process');

   // Execute a command that relies on environment variables
   const envVariable = 'SOME_VAR=example';
   const command = `echo $SOME_VAR`;
   const options = { env: { ...process.env, envVariable } };

   exec(command, options, (error, stdout, stderr) => {
     if (error) {
       console.error(`Error: ${error.message}`);
       return;
     }

     console.log(`Output: ${stdout}`);
     console.error(`Error output: ${stderr}`);
   });
   ```

   This example demonstrates using `exec()` to execute a command (`echo $SOME_VAR`) that relies on an environment variable (`SOME_VAR`). The `options` object is used to pass the environment variable to the child process, along with the current environment variables from `process.env`. The resulting output is captured and logged.
