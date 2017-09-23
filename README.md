# What is this repository for?

Node.js app architecture showcase. You can start your Node.js projects building on this boilerplate.

# Architecture Overview

The app is designed to use a layered architecture. The architecture is heavily influenced by the Clean Architecture. The code style being used is based on the [airbnb js style guide](https://github.com/airbnb/javascript)

## Data Layer

The data layer is implemented using repositories, that hide the underlying data sources (database, network, cache, etc), and provides an abstraction over them so other parts of the application that make use of the repositories, don't care about the origin of the data and are decoupled from the specific implementations used, like the Sequelize ORM (PostgresSQL) that is used by this app.
Furthermore, the repositories are responsible to map the entities they fetch from the data sources to the models used in the applications. This is important to enable the decoupling.

## Domain Layer

The domain layer is implemented using services. They depend on the repository interfaces to get the app models and apply the business rules on them. They are not coupled to a specific database implementation and can be reused if we add more data sources to the app or even if we change the database for example from Postgres to MongoDB.

## Routes/Controller Layer

This layer is being used in the express app and depends on the domain layer (services). Here we define the routes that can be called from outside. The services are always used as the last middleware on the routes and we must not rely on res.locals from express to get data from previous middlewares. That means that the middlewares registered before should not alter data being passed to the domain layer. They are only allowed to act upon the data without modification, like for example validating the data and skipping calling `next()`.

## Entry point

The entry point for the applications is the [server.js](./src/server.js) file. It does __not__ depend on express.js or other node.js frameworks. It is responsible for instantiating the application layers, connecting to the db, mounting the http server to the specified port and handling the signals for graceful shutdown.

# Quick start

### Prerequisites:

Define an `.env` file at the root directory of the project containing all the environment variables needed. You can find the keys needed for the env vars key-value pairs in the [configuration](./src/configuration/index.js) file.

#### Use Docker:

You can use Docker to start the app. The [Dockerfile](./Dockerfile) is already provided for you.

#### Use the npm scripts:

`npm run debug` for debugging using nodemon to auto restart the server on changes.

*or*

`npm run build` to build the project (removes the flow annotations) and then,</br>
`npm start` to start the server.


## Packages and Tools

- [Node v8+](http://nodejs.org/)
- [Express](https://npmjs.com/package/express)
- [Sequelize](https://www.npmjs.com/package/sequelize)
- [Nodemon](https://github.com/remy/nodemon)
- [Mocha](https://www.npmjs.com/package/mocha)
- [Chai](https://www.npmjs.com/package/chai)
- [ESLint](https://www.npmjs.com/package/eslint)
- [Sinon](https://www.npmjs.com/package/sinon)
- [Supertest](https://github.com/visionmedia/supertest)
- [Flow](https://github.com/facebook/flow)

## License

```
Copyright 2017 Alexandros Koufatzis.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.