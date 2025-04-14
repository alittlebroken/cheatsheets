# Setting up your project for testing
The below set of steps allow you to setup a new or existing ReactJS project to use Jest for testing

## Install packages

```bash
npm install jest jestdom jest-environment-jsdom --save-dev
npm install @testing-library/react @testing-library/jest-dom --save-dev
npm install @babel/preset-env @babel/preset-react
```

## Configure Jest

Run the following commands from the root fo your project directory

``bash
code jest.config.js
``

- Copy and paste the following code into the file
```js
export default {
    testEnvironment: "jsdom",
    moduleNameMapper: {
        "\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$": "<rootDir>/__mocks__/fileMock.js",
        "\\.(css|less|css.ts|css.js)$": "<rootDir>/__mocks__/styleMock.js"
    }
}
```
- save and close the file

- Create the scripts to mock CSS and IMG files
```bash
mkdir __mocks__
cd __mocks__
code fileMock.js
```

- Copy and paste the following code into the file
```js
export {};
export default "file-stub";
```
- save and close the file

```bash
code styleMock.js
```

- Copy and paste the following code into the file
```js
export default {};
```
- save and close the file

## Configure babel

All commands should be run from the project root

```bash
code babel.config.js
```

- Copy the following code into the file
```js
export default {
    presets: ['@babel/preset-env', ['@babel/preset-react', { "runtime": "automatic" }]]
}
```
- Save and close the file
