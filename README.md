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

## Create a new project

We use [create-react-app](https://github.com/facebook/create-react-app) to setup a new ReactJS project.

Inside your project you will have two application - **server** _(GraphQL)_ and **client** _(ReactJS + Relay)_ ones.

```shell
mkdir ~/workspace
cd ~/workspace
mkdir my-project
cd my-project
npm init react-app client
npm init react-app server
```

## Run project

You need to run client and server apps at the same time _(use two different terminal windows or tabs for this)_.

```shell
# run server app (the API server based on GraphQL)
cd ~/workspace/my-project/server
npm start
# run web app (UI part what we see in the browser)
cd ~/workspace/my-project/client
npm start
```

To stop server, press `Cmd + C` _(or `Ctrl + C`)_.

## Adding TypeScript

You can use `TypeScript` to improve code quality.

This [manual](https://facebook.github.io/create-react-app/docs/adding-typescript) describes how to add `TypeScript` to the project.

## Adding Relay

We use Relay in the client app to comunicate with GraphQL server.

## Adding GraphQL

GraphQL is used on the server app. It's responsible for getting and storing data in the database.


## Additional useful information

### Update to new release

```shell
# To update to he latest version of a package use:
npm update package-name
# To update all packages in the project use:
npm update
```
