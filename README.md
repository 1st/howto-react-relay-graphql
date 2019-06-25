# How to setup ReactJS with Relay and GraphQL

This is a short step-by-step guide about how to setup ReactJS with Relay and GraphQL support.

I've found many ways how to setup GraphQL with Relay, but some ways are old or not working.
Here you will find how to setup your project in few minutes and start writing your code that uses GraphQL API to fetch and store data in the database.

You will have links to the external resources where you can read more datails about each component of the system. But here will be described only steps that you need to follow to setup your project and have it up and running.

I use `TypeScrip` to make code more protected from potential bugs. Here will be provided instructions how to use ReactJS and Relay with TypeScript support.

*Note:* We use `npm` in this manual. You can use `yarn` if you want.

## Prepare your system

I'm writing all examples for macOS. Thus, some commands may be diffferent on Linux or Windows.

You need to have installed [node.js](https://nodejs.org) _(it comes with `npm 6+`)_. On macOS you can do it with [Homebrew](https://brew.sh):
```sh
brew install node
```

[Watchman](https://facebook.github.io/watchman/) is used to run process in background and when file changes - it recompiles the source code.

```shell
brew install watchman
```

## Create a new project

We use [create-react-app](https://github.com/facebook/create-react-app) to setup a new ReactJS project.

Inside your project you will have two application - **server** _(GraphQL)_ and **client** _(ReactJS + Relay)_.

```shell
mkdir ~/workspace && cd ~/workspace
mkdir my-project && cd my-project
# create a client app
npm init react-app client --typescript
# create a server app
mkdir server && cd server
npm init
```

We use `TypeScript` to improve code quality. Read more about [how to add TypeScript](https://facebook.github.io/create-react-app/docs/adding-typescript) to a ReactJS project.

## Adding Relay

We use Relay in the client app to comunicate with GraphQL server.

```shell
cd ~/workspace/my-project/client
npm install --save react react-dom react-relay
# then add some development-only packages
npm install --save-dev babel-plugin-relay relay-compiler graphql
```

Now, in all places where you need to write GraphQL query, use next import:
```js
import graphql from 'babel-plugin-relay/macro';
// instead of:
//   import { graphql } from "babel-plugin-relay"
```

Use this [manual](https://facebook.github.io/create-react-app/docs/adding-relay) for more details.

## Adding GraphQL

GraphQL is used on the server app. It's responsible for getting and storing data in the database.

We use `nodejs` and `express` framework to run our back-end (server) app.

```shell
cd ~/workspace/my-project/server
npm install --save express express-graphql graphql
```

## Change your `package.json`

You have two apps - server and client. Now you need to make changes to the `package.json` file in each app.

### `client/package.json`

Make few changes in the `package.json` file:
- change the `start` job to do not open web browser automatically when you start the app
- add `relay` job under `scripts` to compile code with Relay support

```json
{
  "scripts": {
    "start": "BROWSER=none react-scripts start",
    "relay": "relay-compiler --src ./src --schema ./schema.graphql --extensions js jsx"
  }
}
```

### `server/package.json`

Add a `start` job to run web server with `npm start`:

```json
{
  "scripts": {
    "start": "node src/index.js"
  }
}
```

## Fill some code

## Server

Create a new file `src/index.js` in the `server` directory with next content:
```js
# File: server/src/index.js
var express = require('express');
var graphqlHTTP = require('express-graphql');
var { buildSchema } = require('graphql');
const fs = require('fs');


const schema_def = fs.readFileSync('src/schema.graphgl', 'utf8');
var schema = buildSchema(schema_def);
var root = { hello: () => 'Hello world!' };

var app = express();
app.use('/api', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true,
}));
app.listen(4000, () => console.log('Now browse to localhost:4000/api'));
```

Also create a file with GraphQL schema definition:
```
# File: server/src/schema.graphgl
type Query {
    hello: String
}

type User {
  id: ID!
  name: String!
  links: [Link!]
  votes: [Vote!]
}

type Link {
  url: String!
  postedBy: User
  votes: [Vote!]
}

type Vote {
  user: User
  link: Link
}
```

## Client

...

## Run project

You need to run client and server apps at the same time _(use two different terminal windows or tabs for this)_.

```shell
# run a server app (the API server based on GraphQL)
cd ~/workspace/my-project/server
npm start
```

Now open this link http://localhost:4000/api in a web browser. You can play with your GraphQL server, making requests and validating responses.

```shell
# run a web app (UI part that we see in the browser)
cd ~/workspace/my-project/client
npm relay --watch
npm start
```

To open the web app, navigate to http://localhost:3000 and start changing your ReactJS app.

To stop server, press `Cmd + C` _(or `Ctrl + C` on some systems)_.

----

## Additional useful information

### How to update to a new release

```shell
# To update to he latest version of a package use:
npm update package-name
# To update all packages in the project use:
npm update
```

You can read more in this [manual](https://facebook.github.io/create-react-app/docs/installing-a-dependency).

### Use `VSCode`

The [VSCode](https://code.visualstudio.com) is the code editor with the best support of TypeScript. It has good support of ReactJS and plenty of other languages and frameworks. It works fast and updates every month with new features.
