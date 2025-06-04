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
