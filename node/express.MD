# Installation & configuration of NodeJS and Express

Follow the below guide to get a basic NodeJS server running express locally or in a container

## Installation

I assume that you have NodeJS installed locally already

* First up intialise the project
```bash
mkdir nodejs-express-app
cd nodejs-express-app
npm init -y
```

* Install the main packages
```bash
npm install express cors helmet joi express-rate-limit dotenv
```

* Install the dev dependencies
```bash
npm instal --save-dev jest nodemon
```

* You can also install any other optional packages here, I usually use the following in my projects
```bash
npm install knex dayjs jsonwebtoken multer
```
## Configuration

* Create the various sub folders after installation
```bash
cd nodejs-express-app
mkdir src
mkdir src/routes/api/v1
mkdir src/services
mkdir src/controllers
mkdir src/utils
mkdir src/config
mkdir src/middleware
mkdir src/database
mkdir uploads
```

* Create the server.js entry point for the app
```bash
code server.js
```

* Copy and past the following into the file
```bash
import config from './src/config/config.js';
import app from "./src/app.js";

app.listen(config.APPPORT, err => {
    if(err){
        throw error;
    }
    console.log("Server listening on port: ", config.APPPORT);
});
```
* Save and close the file

* Now creat the main app.js file
```bash
code src/app.js
```

* Copy and paste the following code into it
```bash
import express from 'express';
import cors from 'cors';
import joi from 'joi';
import helmet from 'helmet';
import { rateLimit } from 'express-rate-limit'

/** Import Middleware **/
import { logging } from './middleware/logging.js';

const app = express();
app.use(express.json());
app.use(express.urlencoded({extended: false}));

/** Apply CORS to all routes */
const corsOptions = {
    origin: "http://localhost:5173"
};
app.use(cors(corsOptions));

/** Apply rate limits **/
app.use(rateLimit({
    windowMs: 15 * 60 * 1000,    // Remember requests for 15 minutes
    limit: 20,                   // Number of requests to allow
    standardHeaders: true,
    legacyHeaders: false
}));

/** Assign the logging middleware for all routes **/
app.use(logging);

/** Apply helmet to the routes *//
app.use(helmet());

/** Routes **/

export default app
```
* Now save and close the file

* Create the config file
```bash
code src/config/config.js
```

* Copy and paste the following code into it
```bash
import dotenv from 'dotenv';
dotenv.config();

const APPPORT = process.env.APP_PORT || 3000;

const config = {
    APPPORT,
}
```
* Save and close the file

* Create the local env file
```bash
code .env
```
* Copy and paste the following code into it
```bash
APP_PORT=3000
```
* Save and close the file

* Create the local .gitignore file
```bash
code .gitignore
```
* Copy and paste the following code into it
```bash
node_modules
.env
uploads
```
* Save and close the file

* Update package.json 
```bash
code package.json
```

* Modify the scripts section to be the same as below
```bash
"scripts": {
    "dev": "nodemon server.js",
    "test": "jest"
  }
```

* Modify the main entry to read as follows
```bash
"main": "server.js",
```

* Add the following directly after the main entry
```bash
"type": "module",
```

* Save and close the file
