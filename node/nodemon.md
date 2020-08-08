## What is it?
[Nodemon](https://nodemon.io/) is a utility that will monitor for any changes in your source and automatically restart your server.

## How to use it?
Instead of running node server with

```sh
node index.js
```
Install nodemon and add script in `package.json`
```json
"server": "nodemon index.js",
```
and then start it using
```sh
yarn server
```
