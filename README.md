# api documentation for  [stdio (v0.2.7)](https://github.com/sgmonda/stdio)  [![npm package](https://img.shields.io/npm/v/npmdoc-stdio.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-stdio) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-stdio.svg)](https://travis-ci.org/npmdoc/node-npmdoc-stdio)
#### Standard input/output management with NodeJS

[![NPM](https://nodei.co/npm/stdio.png?downloads=true)](https://www.npmjs.com/package/stdio)

[![apidoc](https://npmdoc.github.io/node-npmdoc-stdio/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-stdio_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-stdio/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-stdio/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-stdio/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Sergio Garcia Mondaray",
        "email": "sgmonda@gmail.com",
        "url": "http://www.sgmonda.com"
    },
    "bugs": {
        "url": "https://github.com/sgmonda/stdio/issues"
    },
    "dependencies": {},
    "description": "Standard input/output management with NodeJS",
    "devDependencies": {
        "jasmine-node": "1.14.2",
        "jshint": "2.5.1"
    },
    "directories": {},
    "dist": {
        "shasum": "a1c57da10fe1cfaa0c3bf683c9d0743d1b660839",
        "tarball": "https://registry.npmjs.org/stdio/-/stdio-0.2.7.tgz"
    },
    "engines": {
        "node": "*"
    },
    "gitHead": "4016c4f3e81c94936302b12c52d70b31c2ea9897",
    "homepage": "https://github.com/sgmonda/stdio",
    "keywords": [
        "input",
        "console",
        "output",
        "terminal",
        "system",
        "arguments",
        "cli"
    ],
    "license": "MIT",
    "main": "main.js",
    "maintainers": [
        {
            "name": "sgmonda",
            "email": "sgmonda@gmail.com"
        }
    ],
    "name": "stdio",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/sgmonda/stdio.git"
    },
    "scripts": {
        "jshint": "jshint main.js tests",
        "test": "jasmine-node --matchall tests/"
    },
    "version": "0.2.7"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module stdio](#apidoc.module.stdio)
1.  [function <span class="apidocSignatureSpan">stdio.</span>getopt (config, helpTail, argv, testing)](#apidoc.element.stdio.getopt)
1.  [function <span class="apidocSignatureSpan">stdio.</span>progressBar (max, increment)](#apidoc.element.stdio.progressBar)
1.  [function <span class="apidocSignatureSpan">stdio.</span>question (question, options, callback)](#apidoc.element.stdio.question)
1.  [function <span class="apidocSignatureSpan">stdio.</span>read (callback)](#apidoc.element.stdio.read)
1.  [function <span class="apidocSignatureSpan">stdio.</span>readByLines (lineProcessor, callback)](#apidoc.element.stdio.readByLines)
1.  object <span class="apidocSignatureSpan">stdio.</span>progress
1.  object <span class="apidocSignatureSpan">stdio.</span>reading
1.  object <span class="apidocSignatureSpan">stdio.</span>util

#### [module stdio.getopt](#apidoc.module.stdio.getopt)
1.  [function <span class="apidocSignatureSpan">stdio.</span>getopt (config, helpTail, argv, testing)](#apidoc.element.stdio.getopt.getopt)
1.  [function <span class="apidocSignatureSpan">stdio.getopt.</span>preProcessArguments (argv)](#apidoc.element.stdio.getopt.preProcessArguments)

#### [module stdio.progress](#apidoc.module.stdio.progress)
1.  [function <span class="apidocSignatureSpan">stdio.progress.</span>progressBar (max, increment)](#apidoc.element.stdio.progress.progressBar)

#### [module stdio.question](#apidoc.module.stdio.question)
1.  [function <span class="apidocSignatureSpan">stdio.</span>question (question, options, callback)](#apidoc.element.stdio.question.question)

#### [module stdio.reading](#apidoc.module.stdio.reading)
1.  [function <span class="apidocSignatureSpan">stdio.reading.</span>read (callback)](#apidoc.element.stdio.reading.read)
1.  [function <span class="apidocSignatureSpan">stdio.reading.</span>readByLines (lineProcessor, callback)](#apidoc.element.stdio.reading.readByLines)

#### [module stdio.util](#apidoc.module.stdio.util)
1.  [function <span class="apidocSignatureSpan">stdio.util.</span>textDistance (a, b)](#apidoc.element.stdio.util.textDistance)



# <a name="apidoc.module.stdio"></a>[module stdio](#apidoc.module.stdio)

#### <a name="apidoc.element.stdio.getopt"></a>[function <span class="apidocSignatureSpan">stdio.</span>getopt (config, helpTail, argv, testing)](#apidoc.element.stdio.getopt)
- description and source-code
```javascript
function getopt(config, helpTail, argv, testing) {

	argv = argv || process.argv;
	config = config || {};
	var programName = argv[1].split('/').pop();

	// Help option cannot be overrided
	if (config.help) {
		throw new Error('"--help" option is reserved for the automatic help message. You cannot override it when calling getopt().');
	}

	var usedKeys = {};
	var key, option;
	for (option in config) {

		key = config[option].key;

		if (!key) {
			continue;
		}

		// Short options name has to be unique
		if (usedKeys[key]) {
			throw new Error('Short key "' + key + '" is repeated in getopt() config. You cannot use the same key for two options.');
		}

		// Multiple option is not compatible with args count
		if (config[option].multiple && ('args' in config[option])) {
			throw new Error('"args" count cannot be specified for options marked with "multiple" flag.');
		}
		usedKeys[key] = true;
	}

	// Print help message when executing with --help or -h
	if (argv.indexOf('--help') !== -1) {
		printHelpMessage(config, helpTail, programName);
		process.exit(0);
	}

	// Build de options/arguments map
	var cmdOptions = extractArgumentsMap(argv, config);

	// Check every mandatory option is specified
	for (option in config) {
		if (option === '_meta_') {
			continue;
		}
		if (config[option].mandatory && !(option in cmdOptions)) {
			if (testing) {
				return null;
			}
			console.log('Missing "%s" argument.\nTry "--help" for more information.', option);
			process.exit(-1);
		}
		if (!(option in cmdOptions)) {
			continue;
		}

		// Check all required arguments have been specified for each option
		var requiredArgumentsCount = config[option].args;
		var providedArgumentsCount = cmdOptions[option] === true ? 0 : (Array.isArray(cmdOptions[option]) ? cmdOptions[option].length :
1);
		if (requiredArgumentsCount > 1 && providedArgumentsCount !== requiredArgumentsCount) {
			if (testing) {
				return null;
			}
			console.log('Option "--%s" requires %d arguments, but %d were provided. Try "--help" for more information.', option, requiredArgumentsCount
, providedArgumentsCount);
			printHelpMessage(config, helpTail, programName);
			process.exit(-1);
		}
	}

	// Check expected positional arguments are provided
	var providedArgs = 0;
	if (Array.isArray(cmdOptions.args) && cmdOptions.args.length > 0) {
		providedArgs = cmdOptions.args.length;
	}
	if (config._meta_ && config._meta_.args && providedArgs !== config._meta_.args) {
		console.log('%d positional arguments (without option flag) are required, but %d have been provided.', config._meta_.args, providedArgs
);
		printHelpMessage(config, helpTail, programName);
		process.exit(-1);
	}
	if (config._meta_ && config._meta_.minArgs && providedArgs < config._meta_.minArgs) {
		console.log('At least %d positional arguments (without option flag) are required, but %d have been provided.', config._meta_.minArgs
, providedArgs);
		printHelpMessage(config, helpTail, programName);
		process.exit(-1);
	}
	if (config._meta_ && config._meta_.maxArgs && providedArgs > config._meta_.maxArgs) {
		console.log('Too many positional arguments (without option flag) provided. The maximum allowed is %d, but %d have been provided
.', config._meta_.maxArgs, providedArgs);
		printHelpMessage(config, helpTail, programName);
		process.exit(-1);
	}

	// Apply default values
	for (option in config) {
		if (typeof config[option] === 'object' && 'default' in config[option]) {
			if ((!Array.isArray(config[option]['default']) && parseInt(config[option].args, 10) > 1) ||
				(Array.isArray(config[option]['default']) && config[option]['default'].length !== parseInt(config[option].args, 10))) {
				throw new Error('Default value of an option must have the same length as specified by its "args" attribute');
			}
			cmdOptions[option] = cmdOptions[option] || config[option]['default'];
		}

	}

	// A function to print help message manually
	cmdOptions.printHelp = function () {
		printHelpMessage(config, helpTail, programName);
	};

	return cmdOptions;
}
```
- example usage
```shell
...
* Read standard input, at once or line by line.
* Make command-line questions

### 2.1. Parse Unix-like command line options

'''javascript
var stdio = require('stdio');
var ops = stdio.getopt({
    'check': {key: 'c', args: 2, description: 'What this option means'},
    'map': {key: 'm', description: 'Another description', mandatory: true},
    'kaka': {key: 'k', args: 2, mandatory: true},
    'ooo': {key: 'o'}
});
console.log(ops);
'''
...
```

#### <a name="apidoc.element.stdio.progressBar"></a>[function <span class="apidocSignatureSpan">stdio.</span>progressBar (max, increment)](#apidoc.element.stdio.progressBar)
- description and source-code
```javascript
progressBar = function (max, increment) {
	var p = new ProgressBar(max, increment);
	return p;
}
```
- example usage
```shell
...
### 2.5. Show a progress bar

The following code will create a progress bar of 100 pieces and increments of 10. These values do not affect how the progress bar
 is shown, so a progress bar with 100 steps will be equals to a progress bar with 4 steps. Every call to 'tick()' increments the
 bar value. Remaining time is estimated dynamically:

'''javascript
var stdio = require('./main.js');

var pbar = stdio.progressBar(100, 10);
var i = setInterval(function () {
	pbar.tick();
}, 1000);
pbar.onFinish(function () {
	console.log('finish');
	clearInterval(i);
});
...
```

#### <a name="apidoc.element.stdio.question"></a>[function <span class="apidocSignatureSpan">stdio.</span>question (question, options, callback)](#apidoc.element.stdio.question)
- description and source-code
```javascript
function askQuestion(question, options, callback) {

	// Options can be omited
	if (typeof options === 'function') {
		callback = options;
		options = null;
	}

	// Throw possible errors
	if (!question) {
		throw new Error('Stdio prompt question is malformed. It must include at least a question text.');
	}
	if (options && (!Array.isArray(options) || options.length < 2)) {
		throw new Error('Stdio prompt question is malformed. Provided options must be an array with two options at least.');
	}

	<span class="apidocCodeCommentSpan">/**
	 * Prints the question
	 **/
</span>	var performQuestion = function () {
		var str = question;
		if (options) {
			str += ' [' + options.join('/') + ']';
		}
		str += ': ';
		process.stdout.write(str);
	};

	var tries = MAX_PROMPT_TRIES;

	process.stdin.resume();

	var listener = function (data) {

		var response = data.toString().toLowerCase().trim();

		if (options && options.indexOf(response) === -1) {
			console.log('Unexpected answer. %d retries left.', tries - 1);
			tries--;
			if (tries === 0) {
				process.stdin.removeListener('data', listener);
				process.stdin.pause();
				callback('Retries spent');
			} else {
				performQuestion();
			}
			return;
		}
		process.stdin.removeListener('data', listener);
		process.stdin.pause();
		callback(false, response);
	};

	process.stdin.addListener('data', listener);
	performQuestion();
}
```
- example usage
```shell
...
'''

### 2.4. Show prompt questions and wait user's answer

The following code will ask the user for some info and then print it.

'''javascript
stdio.question('What is your name?', function (err, name) {
    stdio.question('How old are you?', function (err, age) {
        stdio.question('Are you male or female?', ['male', 'female'], function (err, sex) {
            console.log('Your name is "%s". You are a "%s" "%s" years old.', name, sex, age);
        });
    });
});
'''
...
```

#### <a name="apidoc.element.stdio.read"></a>[function <span class="apidocSignatureSpan">stdio.</span>read (callback)](#apidoc.element.stdio.read)
- description and source-code
```javascript
function read(callback) {

	if (!callback) {
		throw new Error('no callback provided to readInput() call');
	}

	var inputdata = '';
	process.stdin.resume();

	var listener = function (text) {
		if (!text) {
			return;
		}
		inputdata += String(text);
	};

	process.stdin.on('data', listener);

	process.stdin.on('end', function () {
		process.stdin.removeListener('data', listener);
		callback(inputdata);
	});
}
```
- example usage
```shell
...

### 2.2. Read standard input at once

The following code will read the whole standard input at once and put it into 'text' variable.

'''javascript
var stdio = require('stdio');
stdio.read(function(text){
    console.log(text);
});
'''

Obviously it is recommended only for small input streams, for instance a small file:

'''
...
```

#### <a name="apidoc.element.stdio.readByLines"></a>[function <span class="apidocSignatureSpan">stdio.</span>readByLines (lineProcessor, callback)](#apidoc.element.stdio.readByLines)
- description and source-code
```javascript
function readByLines(lineProcessor, callback) {

	var index = 0;

	var readline = require('readline');
	var rl = readline.createInterface({
		input: process.stdin,
		output: process.stdout,
		terminal: false
	});
	rl.on('line', function (line) {
		lineProcessor(line, index);
		index++;
	});
	if (callback) {
		rl.on('close', callback);
	}
}
```
- example usage
```shell
...

### 2.3. Read standard input line by line

The following code will execute dynamically a function over every line, when it is read from the standard input:

'''javascript
var stdio = require('stdio');
stdio.readByLines(function lineHandler(line, index) {
    // You can do whatever you want with every line
    console.log('Line %d:', index, line);
}, function (err) {
    console.log('Finished');
});
'''
...
```



# <a name="apidoc.module.stdio.getopt"></a>[module stdio.getopt](#apidoc.module.stdio.getopt)

#### <a name="apidoc.element.stdio.getopt.getopt"></a>[function <span class="apidocSignatureSpan">stdio.</span>getopt (config, helpTail, argv, testing)](#apidoc.element.stdio.getopt.getopt)
- description and source-code
```javascript
function getopt(config, helpTail, argv, testing) {

	argv = argv || process.argv;
	config = config || {};
	var programName = argv[1].split('/').pop();

	// Help option cannot be overrided
	if (config.help) {
		throw new Error('"--help" option is reserved for the automatic help message. You cannot override it when calling getopt().');
	}

	var usedKeys = {};
	var key, option;
	for (option in config) {

		key = config[option].key;

		if (!key) {
			continue;
		}

		// Short options name has to be unique
		if (usedKeys[key]) {
			throw new Error('Short key "' + key + '" is repeated in getopt() config. You cannot use the same key for two options.');
		}

		// Multiple option is not compatible with args count
		if (config[option].multiple && ('args' in config[option])) {
			throw new Error('"args" count cannot be specified for options marked with "multiple" flag.');
		}
		usedKeys[key] = true;
	}

	// Print help message when executing with --help or -h
	if (argv.indexOf('--help') !== -1) {
		printHelpMessage(config, helpTail, programName);
		process.exit(0);
	}

	// Build de options/arguments map
	var cmdOptions = extractArgumentsMap(argv, config);

	// Check every mandatory option is specified
	for (option in config) {
		if (option === '_meta_') {
			continue;
		}
		if (config[option].mandatory && !(option in cmdOptions)) {
			if (testing) {
				return null;
			}
			console.log('Missing "%s" argument.\nTry "--help" for more information.', option);
			process.exit(-1);
		}
		if (!(option in cmdOptions)) {
			continue;
		}

		// Check all required arguments have been specified for each option
		var requiredArgumentsCount = config[option].args;
		var providedArgumentsCount = cmdOptions[option] === true ? 0 : (Array.isArray(cmdOptions[option]) ? cmdOptions[option].length :
1);
		if (requiredArgumentsCount > 1 && providedArgumentsCount !== requiredArgumentsCount) {
			if (testing) {
				return null;
			}
			console.log('Option "--%s" requires %d arguments, but %d were provided. Try "--help" for more information.', option, requiredArgumentsCount
, providedArgumentsCount);
			printHelpMessage(config, helpTail, programName);
			process.exit(-1);
		}
	}

	// Check expected positional arguments are provided
	var providedArgs = 0;
	if (Array.isArray(cmdOptions.args) && cmdOptions.args.length > 0) {
		providedArgs = cmdOptions.args.length;
	}
	if (config._meta_ && config._meta_.args && providedArgs !== config._meta_.args) {
		console.log('%d positional arguments (without option flag) are required, but %d have been provided.', config._meta_.args, providedArgs
);
		printHelpMessage(config, helpTail, programName);
		process.exit(-1);
	}
	if (config._meta_ && config._meta_.minArgs && providedArgs < config._meta_.minArgs) {
		console.log('At least %d positional arguments (without option flag) are required, but %d have been provided.', config._meta_.minArgs
, providedArgs);
		printHelpMessage(config, helpTail, programName);
		process.exit(-1);
	}
	if (config._meta_ && config._meta_.maxArgs && providedArgs > config._meta_.maxArgs) {
		console.log('Too many positional arguments (without option flag) provided. The maximum allowed is %d, but %d have been provided
.', config._meta_.maxArgs, providedArgs);
		printHelpMessage(config, helpTail, programName);
		process.exit(-1);
	}

	// Apply default values
	for (option in config) {
		if (typeof config[option] === 'object' && 'default' in config[option]) {
			if ((!Array.isArray(config[option]['default']) && parseInt(config[option].args, 10) > 1) ||
				(Array.isArray(config[option]['default']) && config[option]['default'].length !== parseInt(config[option].args, 10))) {
				throw new Error('Default value of an option must have the same length as specified by its "args" attribute');
			}
			cmdOptions[option] = cmdOptions[option] || config[option]['default'];
		}

	}

	// A function to print help message manually
	cmdOptions.printHelp = function () {
		printHelpMessage(config, helpTail, programName);
	};

	return cmdOptions;
}
```
- example usage
```shell
...
* Read standard input, at once or line by line.
* Make command-line questions

### 2.1. Parse Unix-like command line options

'''javascript
var stdio = require('stdio');
var ops = stdio.getopt({
    'check': {key: 'c', args: 2, description: 'What this option means'},
    'map': {key: 'm', description: 'Another description', mandatory: true},
    'kaka': {key: 'k', args: 2, mandatory: true},
    'ooo': {key: 'o'}
});
console.log(ops);
'''
...
```

#### <a name="apidoc.element.stdio.getopt.preProcessArguments"></a>[function <span class="apidocSignatureSpan">stdio.getopt.</span>preProcessArguments (argv)](#apidoc.element.stdio.getopt.preProcessArguments)
- description and source-code
```javascript
function preProcessArguments(argv) {

	var processedArgs = [];
	argv.forEach(function (arg) {

		// If the argument is not an option, do not touch it
		if (arg[0] !== '-' || isNumericalArgument(arg)) {
			processedArgs.push(arg);
			return;
		}

		// For collapsed options, like "-abc" instead of "-a -b -c"
		if (arg[0] === '-' && arg[1] !== '-' && arg.length > 2 && arg.indexOf('=') === -1) {
			processedArgs = processedArgs.concat(arg.slice(1).split('').map(function (x) {
				return '-' + x;
			}));
			return;
		}

		// The general case, without collapsed options or assignments
		if (arg[0] !== '-' || arg.indexOf('=') === -1) {
			processedArgs.push(arg);
			return;
		}

		// For assignment options, like "-b=2" instead of "-b 2"
		arg = arg.match(/(.*?)=(.*)/);
		processedArgs.push(arg[1]);
		processedArgs.push(arg[2]);
	});
	return processedArgs;
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.stdio.progress"></a>[module stdio.progress](#apidoc.module.stdio.progress)

#### <a name="apidoc.element.stdio.progress.progressBar"></a>[function <span class="apidocSignatureSpan">stdio.progress.</span>progressBar (max, increment)](#apidoc.element.stdio.progress.progressBar)
- description and source-code
```javascript
progressBar = function (max, increment) {
	var p = new ProgressBar(max, increment);
	return p;
}
```
- example usage
```shell
...
### 2.5. Show a progress bar

The following code will create a progress bar of 100 pieces and increments of 10. These values do not affect how the progress bar
 is shown, so a progress bar with 100 steps will be equals to a progress bar with 4 steps. Every call to 'tick()' increments the
 bar value. Remaining time is estimated dynamically:

'''javascript
var stdio = require('./main.js');

var pbar = stdio.progressBar(100, 10);
var i = setInterval(function () {
	pbar.tick();
}, 1000);
pbar.onFinish(function () {
	console.log('finish');
	clearInterval(i);
});
...
```



# <a name="apidoc.module.stdio.question"></a>[module stdio.question](#apidoc.module.stdio.question)

#### <a name="apidoc.element.stdio.question.question"></a>[function <span class="apidocSignatureSpan">stdio.</span>question (question, options, callback)](#apidoc.element.stdio.question.question)
- description and source-code
```javascript
function askQuestion(question, options, callback) {

	// Options can be omited
	if (typeof options === 'function') {
		callback = options;
		options = null;
	}

	// Throw possible errors
	if (!question) {
		throw new Error('Stdio prompt question is malformed. It must include at least a question text.');
	}
	if (options && (!Array.isArray(options) || options.length < 2)) {
		throw new Error('Stdio prompt question is malformed. Provided options must be an array with two options at least.');
	}

	<span class="apidocCodeCommentSpan">/**
	 * Prints the question
	 **/
</span>	var performQuestion = function () {
		var str = question;
		if (options) {
			str += ' [' + options.join('/') + ']';
		}
		str += ': ';
		process.stdout.write(str);
	};

	var tries = MAX_PROMPT_TRIES;

	process.stdin.resume();

	var listener = function (data) {

		var response = data.toString().toLowerCase().trim();

		if (options && options.indexOf(response) === -1) {
			console.log('Unexpected answer. %d retries left.', tries - 1);
			tries--;
			if (tries === 0) {
				process.stdin.removeListener('data', listener);
				process.stdin.pause();
				callback('Retries spent');
			} else {
				performQuestion();
			}
			return;
		}
		process.stdin.removeListener('data', listener);
		process.stdin.pause();
		callback(false, response);
	};

	process.stdin.addListener('data', listener);
	performQuestion();
}
```
- example usage
```shell
...
'''

### 2.4. Show prompt questions and wait user's answer

The following code will ask the user for some info and then print it.

'''javascript
stdio.question('What is your name?', function (err, name) {
    stdio.question('How old are you?', function (err, age) {
        stdio.question('Are you male or female?', ['male', 'female'], function (err, sex) {
            console.log('Your name is "%s". You are a "%s" "%s" years old.', name, sex, age);
        });
    });
});
'''
...
```



# <a name="apidoc.module.stdio.reading"></a>[module stdio.reading](#apidoc.module.stdio.reading)

#### <a name="apidoc.element.stdio.reading.read"></a>[function <span class="apidocSignatureSpan">stdio.reading.</span>read (callback)](#apidoc.element.stdio.reading.read)
- description and source-code
```javascript
function read(callback) {

	if (!callback) {
		throw new Error('no callback provided to readInput() call');
	}

	var inputdata = '';
	process.stdin.resume();

	var listener = function (text) {
		if (!text) {
			return;
		}
		inputdata += String(text);
	};

	process.stdin.on('data', listener);

	process.stdin.on('end', function () {
		process.stdin.removeListener('data', listener);
		callback(inputdata);
	});
}
```
- example usage
```shell
...

### 2.2. Read standard input at once

The following code will read the whole standard input at once and put it into 'text' variable.

'''javascript
var stdio = require('stdio');
stdio.read(function(text){
    console.log(text);
});
'''

Obviously it is recommended only for small input streams, for instance a small file:

'''
...
```

#### <a name="apidoc.element.stdio.reading.readByLines"></a>[function <span class="apidocSignatureSpan">stdio.reading.</span>readByLines (lineProcessor, callback)](#apidoc.element.stdio.reading.readByLines)
- description and source-code
```javascript
function readByLines(lineProcessor, callback) {

	var index = 0;

	var readline = require('readline');
	var rl = readline.createInterface({
		input: process.stdin,
		output: process.stdout,
		terminal: false
	});
	rl.on('line', function (line) {
		lineProcessor(line, index);
		index++;
	});
	if (callback) {
		rl.on('close', callback);
	}
}
```
- example usage
```shell
...

### 2.3. Read standard input line by line

The following code will execute dynamically a function over every line, when it is read from the standard input:

'''javascript
var stdio = require('stdio');
stdio.readByLines(function lineHandler(line, index) {
    // You can do whatever you want with every line
    console.log('Line %d:', index, line);
}, function (err) {
    console.log('Finished');
});
'''
...
```



# <a name="apidoc.module.stdio.util"></a>[module stdio.util](#apidoc.module.stdio.util)

#### <a name="apidoc.element.stdio.util.textDistance"></a>[function <span class="apidocSignatureSpan">stdio.util.</span>textDistance (a, b)](#apidoc.element.stdio.util.textDistance)
- description and source-code
```javascript
textDistance = function (a, b) {

	if(a.length === 0) {
		return b.length;
	}
	if(b.length === 0) {
		return a.length;
	}
	
	var matrix = [];
	
	// increment along the first column of each row
	var i;
	for(i = 0; i <= b.length; i++){
		matrix[i] = [i];
	}
	
	// increment each column in the first row
	var j;
	for(j = 0; j <= a.length; j++){
		matrix[0][j] = j;
	}
	
	// Fill in the rest of the matrix
	for(i = 1; i <= b.length; i++){
		for(j = 1; j <= a.length; j++){
			if(b.charAt(i-1) == a.charAt(j-1)){
				matrix[i][j] = matrix[i-1][j-1];
			} else {
				matrix[i][j] = Math.min(matrix[i-1][j-1] + 1, Math.min(matrix[i][j-1] + 1, matrix[i-1][j] + 1));
			}
		}
	}
	
	return matrix[b.length][a.length];
}
```
- example usage
```shell
...
	 * Find the nearest option of a provided one
	 **/
	function getNearestOption(option) {
		var options = Object.keys(config);
		var minDistance = Infinity;
		var nearestWord = null;
		options.forEach(function (o) {
			var d = util.textDistance(o, option);
			if (d < minDistance) {
				minDistance = d;
				nearestWord = o;
			}
		});
		if (minDistance < 3) {
			return nearestWord;
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
