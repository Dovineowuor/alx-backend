# 0x03. Queuing System in JS

This repository showcases configuration files for handson projects on Redis server. 

## Resources
### Read or watch:
> [Redis quick start](https://intranet.alxswe.com/rltoken/8xeApIhnxgFZkgn54BiIeA)<br>
> [Redis client interphase](https://intranet.alxswe.com/rltoken/1rq3ral-3C5O1t67dbGcWg)<br>
> [Redis client for Node JS](https://intranet.alxswe.com/rltoken/mRftfl67BrNvl-RM5JQfUA)<br>
> [Kue](https://intranet.alxswe.com/rltoken/yTC3Ci2IV2US24xJsBfMgQ) depreciated but still use in industyr<br>


## Learning Objectives

At the end of this project, you are expected to be able to [explain to anyone](https://intranet.alxswe.com/rltoken/7yh7c3Zyy1RyUsdwlfsyDg), <b>without the help of Google</b>: <br>

> How to run a Redis server on your machine<br>
> How to run simple operations with the Redis client<br>
> How to use a Redis client with Node JS for basic operations<br>
> How to store hash values in Redis<br>
> How to deal with async operations with Redis<br>
> How to use Kue as a queue system<br>
> How to build a basic Express app interacting with a Redis server<br>
> How to the build a basic Express app interacting with a Redis server and queue<br>


## Requirements
> All of your code will be compiled/interpreted on Ubuntu 18.04, Node 12.x, and Redis 5.0.7
> All of your files should end with a new line
> A README.md file, at the root of the folder of the project, is mandatory
> Your code should use the js extension

## Required Files for the Project

`package.json`<br>

``
{
    "name": "queuing_system_in_js",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "lint": "./node_modules/.bin/eslint",
      "check-lint": "lint [0-9]*.js",
      "test": "./node_modules/.bin/mocha --require @babel/register --exit",
      "dev": "nodemon --exec babel-node --presets @babel/preset-env"
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
      "chai-http": "^4.3.0",
      "express": "^4.17.1",
      "kue": "^0.11.6",
      "redis": "^2.8.0"
    },
    "devDependencies": {
      "@babel/cli": "^7.8.0",
      "@babel/core": "^7.8.0",
      "@babel/node": "^7.8.0",
      "@babel/preset-env": "^7.8.2",
      "@babel/register": "^7.8.0",
      "eslint": "^6.4.0",
      "eslint-config-airbnb-base": "^14.0.0",
      "eslint-plugin-import": "^2.18.2",
      "eslint-plugin-jest": "^22.17.0",
      "nodemon": "^2.0.2",
      "chai": "^4.2.0",
      "mocha": "^6.2.2",
      "request": "^2.88.0",
      "sinon": "^7.5.0"
    }
  }
``
<br>

`.babelrc`<br>
``
{
  "presets": [
    "@babel/preset-env"
  ]
}
``

## and ...
Don’t forget to run `$ npm install` when you have the `package.json`


# Tasks

### 0. Install a redis instance
Download, extract, and compile the latest stable Redis version (higher than 5.0.7 - [Redis-5.0.7](https://redis.io/download/)):

```
$ wget http://download.redis.io/releases/redis-6.0.10.tar.gz
$ tar xzf redis-6.0.10.tar.gz
$ cd redis-6.0.10
$ make
```

<br>

> Start Redis in the background with `src/redis-server`<br>
``$ src/redis-server &``
<br>

> Make sure that the server is working with a ping ``src/redis-cli ping``
``PONG``
<br>
> Using the Redis client again, set the value School for the key `Holberton`<br>

``127.0.0.1:[Port]> set Holberton School``<
``OK``
``127.0.0.1:[Port]> get Holberton``
``"School"``

> Kill the server with the process id of the redis-server (hint: use `ps` and `grep`)<br>

``$ kill [PID_OF_Redis_Server]``

<br>

> Copy the dump.rdb from the redis-5.0.7 directory into the root of the Queuing project.

Requirements:

> Running `get Holberton` in the client, should return `School`

<br>

### Repo:
> GitHub repository: alx-backend
> Directory: 0x03-queuing_system_in_js
> File: README.md, dump.rdb

<br>

## 1. Node Redis Client

Install node_redis using npm

Using Babel and ES6, write a script named `0-redis_client.js`. It should connect to the Redis server running on your machine:

	> It should log to the console the message `Redis client connected to the server` when the connection to Redis works correctly
	> It should log to the console the message `Redis client not connected to the server: ERROR_MESSAGE` when the connection to Redis does not work
### Requirements:

	> To import the library, you need to use the keyword `import`

	bob@dylan:~$ ps ax | grep redis-server
 	2070 pts/1    S+     0:00 grep --color=auto redis-server
	bob@dylan:~$ 
	bob@dylan:~$ npm run dev 0-redis_client.js 

	> queuing_system_in_js@1.0.0 dev /root
	> nodemon --exec babel-node --presets @babel/preset-env "0-redis_client.js"

	[nodemon] 2.0.4
	[nodemon] to restart at any time, enter `rs`
	[nodemon] watching path(s): *.*
	[nodemon] watching extensions: js,mjs,json
	[nodemon] starting `babel-node --presets @babel/preset-env 0-redis_client.js`
	Redis client not connected to the server: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED 127.0.0.1:6379
	Redis client not connected to the server: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED 127.0.0.1:6379
	Redis client not connected to the server: Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED 127.0.0.1:6379
	^C
	bob@dylan:~$ 
	bob@dylan:~$ ./src/redis-server > /dev/null 2>&1 &
	[1] 2073
	bob@dylan:~$ ps ax | grep redis-server
	 2073 pts/0    Sl     0:00 ./src/redis-server *:6379
	 2078 pts/1    S+     0:00 grep --color=auto redis-server
	bob@dylan:~$
	bob@dylan:~$ npm run dev 0-redis_client.js 

	> queuing_system_in_js@1.0.0 dev /root
	> nodemon --exec babel-node --presets @babel/preset-env "0-redis_client.js"

	[nodemon] 2.0.4
	[nodemon] to restart at any time, enter `rs`
	[nodemon] watching path(s): *.*
	[nodemon] watching extensions: js,mjs,json
	[nodemon] starting `babel-node --presets @babel/preset-env 0-redis_client.js`
	Redis client connected to the server
	^C
	bob@dylan:~$

### Repo:
> GitHub repository: `alx-backend`
> Directory: `0x03-queuing_system_in_js`
> File: `0-redis_client.js`
