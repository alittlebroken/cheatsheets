# Using REACT with Parcel

This guide will show you how to use React wth Parcel

## Installation
*** [NOTE] *** This guide was written using windows 8.1 ( yes I know )

* Create the root project directory
```
  mkdir <project_name>
  cd <project_name>
```

* Initialise NPM
```
  npm init -y
```

* Install Parcel
```
  npm install --save-dev Parcel
```

* Install React
```
  npm install react react-dom
```

* Install testing software
```
  npm install --save-dev jest jest-dom @testing-library/react @testing-library/dom @testing-library/jest-dom @babel/preset-env @babel/preset-react 
```

### Folder structure

* Run the following commands to create the folder structure from inside the projects root folder
```
  mkdir \__mocks__
  mkdir \__tests__
  mkdir \public
  mkdir \src\components
  mkdir \src\features
  mkdir \src\hooks
  mkdir \src\helpers
  mkdir \src\apis
```

## Configuration

### Parcel config

* In the project root run the following commands
```
type nul >.parcelrc
```

* Open the file and copy the following text into, save then close the file
```
  {
    "extends": "@parcel/config-default",
    "transformers": {
      "*.{js,mjs,jsx,cjs,ts,tsx}": [
        "@parcel/transformer-js",
        "@parcel/transformer-react-refresh-wrap"
      ]
    }
  }
```

### Jest and React Testing Library config

* In the projects root filder type the following
```
  type nul >jest.config.js
```

* Open the file and then copy and paste the following text into it. Save and then close the file
```
  export default {
    testEnvironment: "jsdom",
    moduleNameMapper: {
        "\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$": "<rootDir>/__mocks__/fileMock.js",
        "\\.(css|less|css.ts|css.js)$": "<rootDir>/__mocks__/styleMock.js"
    }
}
```

* Now type the following command
```
  type nul >babel.config.js
```

* Open the file and cop and paste the following and then save and close the file
```
  export default {
    presets: ['@babel/preset-env', ['@babel/preset-react', { "runtime": "automatic" }]]
  }
```

* Go into the __mocks__ folder from the project root
```
  cd __mocks__
```

* Type in the following to create the neccessary files
```
  type nul >fileMock.js
  type nul >styleMock.js
```

* Open fileMock.js and copy and paste the folloing text and then save and close the file
```
  export {};
  export default "file-stub";
```

* Now open styleMock.js, copy and paste the following text and then once again save and close the file
```
  export default {};
```

* Go back to the project root for the next steps
```
  cd ..
```

### NPM package scripts
* Open the package.json file in your favourite editor
* Modify the scripts section to read as below
```
"scripts": {
    "start": "parcel --open",
    "build": "parcel build",
    "test": "jest --passWithNoTests"
  },
```

### Create the entry point files
* Go into the public folder
* Create the main html and css files
```
type nul >index.css
type nul >index.html
```

* Open the index.css file and copy the following text into it. Remember to save and close the file afterwards
```
* {
    box-sizing: border-box;
    padding: 0;
    margin: 0;
}
```

* Now open the index.html file and copy the below text inot once again saving and closing the file afterwards
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="./index.css" rel="stylesheet">
    <title>React with Parcel</title>
</head>
<body>
    <div id="root"></div>
    <script src="../src/main.js" type="module"></script>
</body>
</html>
```

* Go back to the project root
```
  cd ..
```

* Go into the src folder
```
 cd src
```

* Create the main.js file
```
  type nul >main.js
```

* Copy and paste the following into the main.js file and save close athe file
```
import { createRoot } from 'react-dom/client'

import App from '~src/App';

createRoot(document.getElementById("root")).render(
    <App />
)
```

* Edit the App.js file and copy and paste the following remembering to save and close the file
```
const App = () => {
    return (
        <h1>React and Parcel</h1>
    )
};

export default App;
``

### Git config

* Initialise the git repo
```
  git init
```

* Create the .gitignore file
```
  type nul >.gitignore
```

* Add the initial .gitgnore content by copy and pasting the below. Remember to save and close the file afterwards
```
# NODEJS
node_modules
.env
.env.local
.env.development
.env.production

# Parcel
.parcel-cache
dist
```

* Rename the initial branch
```
  git branch -M main
```

* Add the remote repo to push changes to
```
  git remote add origin <remote_repo>
```

* Add the project files and produce the first commit and push it to the remote repo
```
  git add .
  git commit -m "Initial project setup"
  git push -U origin main
```

## TODO

* Create a repo that has the above already setup that we can clone easily for new projects
* Add steps for docker config as well
