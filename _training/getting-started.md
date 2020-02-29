
## Installing Node

In order to use  _Express_  you will first have to install  _Nodejs_  and the  [Node Package Manager (NPM)](https://docs.npmjs.com/)  on your operating system. The following sections explain the easiest way to install the Long Term Supported (LTS) version of Nodejs on Ubuntu Linux 18.04, macOS, and Windows 10.

**Tip:** The sections below show the easiest way to install  _Node_  and  _NPM_  on our target OS platforms. If you're using another OS or just want to see some of the other approaches for the current platforms then see [Installing Node.js via package manager](https://nodejs.org/en/download/package-manager/) (nodejs.org).

### macOS and Windows

Installing  _Node_  and  _NPM_  on Windows and macOS is straightforward because you can just use the provided installer:

1.  Download the required installer:
    1.  Go to [https://nodejs.org/en/](https://nodejs.org/en/)
    2.  Select the button to download the LTS build that is "Recommended for most users".
2.  Install Node by double-clicking on the downloaded file and following the installation prompts.

### Ubuntu 18.04

The easiest way to install the most recent LTS version of Node 10.x is to use the  [package manager](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)  to get it from the Ubuntu  _binary distributions_  repository. This can be done very simply by running the following two commands on your terminal:

```bash
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash - 
sudo apt-get install -y nodejs

```

**Warning:**  Don't install directly from the normal Ubuntu repositories because they contain very old versions of node.

### Testing your Nodejs and NPM installation

The easiest way to test that node is installed is to run the "version" command in your terminal/command prompt and check that a version string is returned:

```bash
> node -v
v10.16.0
```

The  _Nodejs_  package manager  _NPM_  should also have been installed, and can be tested in the same way:

```bash
> npm -v
6.9.0
```

As a slightly more exciting test let's create a very basic "pure node" server that simply prints out "Hello World" in the browser when you visit the correct URL in your browser:

1.  Copy the following text into a file named  **hellonode.js**. This uses pure  _Node_  features (nothing from Express) and some ES6 syntax:
    
    ```js
    //Load HTTP module
    const http = require("http");
    const hostname = '127.0.0.1';
    const port = 3000;
    
    //Create HTTP server and listen on port 3000 for requests
    const server = http.createServer((req, res) => {
    
      //Set the response HTTP header with HTTP status and Content type
      res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
      res.end('Hello World\n');
    });
    
    //listen for request on port 3000, and as a callback function have the port listened on logged
    server.listen(port, hostname, () => {
      console.log(`Server running at http://${hostname}:${port}/`);
    });
    
    ```
    
    The code imports the "http" module and uses it to create a server (`createServer()`) that listens for HTTP requests on port 3000. The script then prints a message to the console about what browser URL you can use to test the server. The `createServer()` function takes as an argument a callback function that will be invoked when an HTTP request is received — this simply returns a response with an HTTP status code of 200 ("OK") and the plain text "Hello World".
    
    **Note:** Don't worry if you don't understand exactly what this code is doing yet! We'll explain our code in greater detail once we start using Express!
    
2.  Start the server by navigating into the same directory as your  `hellonode.js`  file in your command prompt, and calling  `node`  along with the script name, like so:
    
    ```bash
    >node hellonode.js
    Server running at http://127.0.0.1:3000/
    
    ```
    
3.  Navigate to the URL  [http://127.0.0.1:3000](http://127.0.0.1:3000/) . If everything is working, the browser should simply display the string "Hello World".

## Using NPM

Next to  _Node_  itself,  [NPM](https://docs.npmjs.com/) is the most important tool for working with _Node_ applications. NPM is used to fetch any packages (JavaScript libraries) that an application needs for development, testing, and/or production, and may also be used to run tests and tools used in the development process.

**Note:** From Node's perspective,  _Express_  is just another package that you need to install using NPM and then require in your own code.

You can manually use NPM to separately fetch each needed package. Typically we instead manage dependencies using a plain-text definition file named [package.json](https://docs.npmjs.com/files/package.json). This file lists all the dependencies for a specific JavaScript "package", including the package's name, version, description, initial file to execute, production dependencies, development dependencies, versions of  _Node_  it can work with, etc. The  **package.json**  file should contain everything NPM needs to fetch and run your application (if you were writing a reusable library you could use this definition to upload your package to the npm repository and make it available for other users).

### Adding dependencies

The following steps show how you can use NPM to download a package, save it into the project dependencies, and then require it in a Node application.

**Note:** Here we show the instructions to fetch and install the  _Express_  package. Later on we'll show how this package, and others, are already specified for us using the _Express Application Generator_. This section is provided because it is useful to understand how NPM works and what is being created by the application generator.

1.  First create a directory for your new application and navigate into it:
    
    ```bash
    mkdir myapp
    cd myapp
    ```
    
2.  Use the npm  `init`  command to create a  **package.json**  file for your application. This command prompts you for a number of things, including the name and version of your application and the name of the initial entry point file (by default this is  **index.js**). For now, just accept the defaults:
    
    ```bash
    npm init
    ```
    
    If you display the  **package.json**  file (`cat package.json`), you will see the defaults that you accepted, ending with the license.
    
    ```json
    {
      "name": "myapp",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "author": "",
      "license": "ISC"
    }
    
    ```
    
3.  Now install Express in the `myapp` directory and save it in the dependencies list of your  **package.json**  file
4.  ```bash
    npm install express
    ```
    
    The dependencies section of your **package.json**  will now appear at the end of the  **package.json**  file and will include _Express_.
    
    {
      "name": "myapp",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "author": "",
      "license": "ISC",
     **"dependencies": {
        "express": "^4.16.4"
      }**
    }
    
5.  To use the Express library you call the  `require()`  function in your index.js file to include it in your application. Create this file now, in the root of the "myapp" application directory, and give it the following contents:
    
    ```js
    const express = require('express')
    const app = express();
    
    app.get('/', (req, res) => {
      res.send('Hello World!')
    });
    
    app.listen(8000, () => {
      console.log('Example app listening on port 8000!')
    });
    
    ```
    
    This code shows a minimal "HelloWorld" Express web application. This imports the "express" module using  `require()`  and uses it to create a server (`app`) that listens for HTTP requests on port 8000 and prints a message to the console explaining what browser URL you can use to test the server. The  `app.get()`  function only responds to HTTP  `GET`  requests with the specified URL path ('/'), in this case by calling a function to send our  _Hello World!_  message.
    
6.  You can start the server by calling node with the script in your command prompt:
    
    ```bash
    >node index.js
    Example app listening on port 8000
    
    ```
    
7.  Navigate to the URL ([http://127.0.0.1:8000/](http://127.0.0.1:8000/)). If everything is working, the browser should simply display the string "Hello World!".

### Development dependencies

If a dependency is only used during development, you should instead save it as a "development dependency" (so that your package users don't have to install it in production). For example, to use the popular JavaScript Linting tool  [eslint](http://eslint.org/)  you would call NPM as shown:

```
npm install eslint --save-dev
```

The following entry would then be added to your application's **package.json**:

```js
  "devDependencies": {
    "eslint": "^4.12.1"
  }

```

**Note:** "[Linters](https://en.wikipedia.org/wiki/Lint_(software))" are tools that perform static analysis on software in order to recognise and report adherence/non-adherance to some set of coding best practice.

### Running tasks

In addition to defining and fetching dependencies you can also define  _named_  scripts in your  **package.json**  files and call NPM to execute them with the  [run-script](https://docs.npmjs.com/cli/run-script)  command. This approach is commonly used to automate running tests and parts of the development or build toolchain (e.g., running tools to minify JavaScript, shrink images, LINT/analyse your code, etc).

**Note:**  Task runners like  [Gulp](http://gulpjs.com/)  and  [Grunt](http://gruntjs.com/)  can also be used to run tests and other external tools.

For example, to define a script to run the  _eslint_  development dependency that we specified in the previous section we might add the following script block to our  **package.json**  file (assuming that our application source is in a folder /src/js):

```js
"scripts": {
  ...
  "lint": "eslint src/js"
  ...
}

```

To explain a little further,  `eslint src/js`  is a command that we could enter in our terminal/command line to run  `eslint`  on JavaScript files contained in the  `src/js`  directory inside our app directory. Including the above inside our app's package.json file provides a shortcut for this command —  `lint`.

We would then be able to run  _eslint_  using NPM by calling:

```
npm run-script lint
# OR (using the alias)
npm run lint
```

This example may not look any shorter than the original command, but you can include much bigger commands inside your npm scripts, including chains of multiple commands. You could identify a single npm script that runs all your tests at once.


