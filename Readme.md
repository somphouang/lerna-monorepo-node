## Overview

Lerna demo builds in Azure DevOps.  
Lerna tool helps for managing JS projects with multiple packages,
it runs npm scripts in each package,
hoist dependencies to root level,
and code sharing between packages

## Get Started

Structure for this demo

```
demo-versioning-lerna
  node_modules/
  packages/
    web-client/
	  ReactAppFrontEnd
	server-api/
	  NodejsAppBackEnd
  package.json
  lerna.json
```

1. Create a folder for this project
```
mkdir demo-versioning-lerna && cd demo-demo-versioning-lerna
```

2. Create a `web-client` using npx
```
npx create-react-app web-client
``` 

3. Install Lerna
```
npm i -D lerna
```

4. Initialize Lerna in the project, helps to create the `lerna.json`
```
npx lerna init
```

5. Move the folder `web-client` into `./packages/web-client` 

6. Open up `./package.json` and add `"version": "1.0.0"`

7. Open up `./lerna.json` and update `"version": "1.0.0"`

8. Go into directory `./packages` and create new folder `server-api` and initialize 
```
cd packages && mkdir server-api && cd server-api && npm init -y
```

9. In the `server-api` install package `express body-parser`
```
npm install express body-parser
```

10. Inside the `server-api` create the file and edit `index.js`
```
vi index.js

## Enter below

const express = require("express");
const port = process.env.PORT || 5000;
const app = express();

app.get("/", (re, res) => {
  res.send("I am a backend service listening");
});

app.listen(port, (err) => {
  if (err) {
    console.log('Error: ${err.message}');
  } else {
    console.log('Listening on port ${port}');
  }
});
```



11. Clean `server-api` using `lerna`, to move all the dependencies to the root `node_modules` folder in the root level
```
npx lerna clean -y
```

12. Run `lerna` bootstrap to hoist all the packages
```
npx lerna bootstrap --hoist
```


13.  Open and edit `./packages/server-api/package.json` and add some `scripts`
```
...
"scripts": {
  "start": "node index.js",
  "test": "echo testing my node server"
},
...
```

14.  Open and edit `./packages/web-client/package.json` and edit `scripts -> test` to just do `echo`
```
"scripts": {
  ...
  "test": "echo testing my front end React app",
  ...
},
```

15.  Open `./package.json` in the root level and add some `"scripts"`
```
"scripts": {
  "start": "lerna run start",
  "test": "lerna run test",
  "new-version": "lerna version --conventional-commits --yes",
  "diff": "lerna diff"
}
```

16.  Test this out from the root, using `lerna`
```
npm run test
```

17.  Run the app services from both packages
```
npm start
```
  * Open up web browser and navigate to `http://localhost:5000` for the backend api server and `http://localhost:3000` for the front end React service.

That's it, now you can add more `scripts` like `publish` and more, see https://lerna.js.org/ for more details.

## Azure DevOps Pipeline

Demo and testing out the Azure DevOps pipeline by creating pipeline referencing the `azure-pipeline.yml` and commit using the conventional commit comment message
with types, like `fix: your message ...`, `feat: new feature added ...`, `doc: update readme ...`, `build: fixed something ...`











