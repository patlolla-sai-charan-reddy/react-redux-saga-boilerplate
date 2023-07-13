
# React JS with Redux and Saga Project Structure
[![Author](http://img.shields.io/badge/author-@maitraysuthar-blue.svg)](https://www.linkedin.com/in/maitray-suthar/) [![GitHub license](https://img.shields.io/github/license/maitraysuthar/rest-api-nodejs-mongodb.svg)](https://github.com/maitraysuthar/react-redux-saga-boilerplate/blob/master/LICENSE)  ![GitHub repo size](https://img.shields.io/github/repo-size/maitraysuthar/react-redux-saga-boilerplate)

A ready-to-use boilerplate for React JS with Redux and Saga.

## Project Overview

This is a basic project structure with repeatative use cases. Added some essential feature for every projects. It is very useful to build mid to complex level project. This project structure is based on **NodeJs api boilerplate app:** https://github.com/maitraysuthar/rest-api-nodejs-mongodb

I had tried to maintain the code structure easy as any beginner can also adopt the flow and start building a great app. Project is open for suggestions, Bug reports and pull requests.

## Is this project deserves a small treat?

If you consider my project as helpful stuff, You can appreciate me or my hard work and time spent to create this helpful structure with buying a coffee for me.

<a href='https://ko-fi.com/U6U617IA8' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi2.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a> &nbsp;&nbsp; <a href="https://www.buymeacoffee.com/36GgOoQ2f" target="_blank"><img src="https://bmc-cdn.nyc3.digitaloceanspaces.com/BMC-button-images/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>

## Features

|Feature|Details  |
|--|--|
|  Structure|  Project is build with extenensible and flexible **Moduler** pattern|
|  Authentication|  Basic Authentication (Register/Login)|
|  Confirm Account|  Account confirmation with OTP verification|
|  Route Protection|  Route protection with middleware and localstorage|
|  Lazy Loading|  Added **Lazy Loading** of components to fasten the execution process of application|
|  App State Management|  Application level state management with **Redux**|
|  Async Call|  Managed async calls with **Saga** middleware|
|  Forms|  Managed apllication forms & validations with **Formik** and **Yup**|

## Software Requirements

-   Node.js **8+**

## How to install

### Using Git (recommended)

1.  Clone the project from github. Change "myproject" to your project name.

```bash
git clone https://github.com/maitraysuthar/react-redux-saga-boilerplate.git ./myproject
```

### Using manual download ZIP

1.  Download repository
2.  Uncompress to your desired directory

### Install npm dependencies after installing (Git or manual download)

```bash
cd myproject
npm install
```

### Setting up environments

1.  You will find a file named `.env.example` on root directory of project.
2.  Create a new file by copying and pasting the file and then renaming it to just `.env`
    ```bash
    cp .env.example .env
    ```
3.  The file `.env` is already ignored, so you never commit your credentials.
4.  Change the values of the file to your environment. Helpful comments added to `.env.example` file to understand the constants.

## How to run

```bash
npm start
```

## New Module

All the modules of the project will be in `/src/modules/` folder, If you need to add more modules to the project just create a new folder in the same folder.

##### Every folder contains following files:
- Component file (`index.jsx`)
- Actions file (`actions.js`)
- Reducer file (`reducer.js`)
- Saga file (`saga.js`)
- Style file (`[module].css`)
- Sub module folder, if any.

##### Root module:
Module's root module folder is `/src/modules/app/` it contains main **Routes file (`routes.js`)**, **Reducer file (`mainReducer.js`)** and **Saga file (`mainSaga.js`)**. You will need to add your every component,reducer & saga to make your module work.

## Found any bug? Need any feature?

Every project needs improvements, Feel free to report any bugs or improvements. Pull requests are always welcome.

## License

This project is open-sourced software licensed under the MIT License. See the LICENSE file for more information.

const express = require('express');
const axios = require('axios');

const app = express();
const port = 3000;

// BFF route handling
app.get('/bff/data', async (req, res) => {
  try {
    // Make requests to backend services
    const service1Response = await axios.get('http://backend-service1/api/data');
    const service2Response = await axios.get('http://backend-service2/api/data');

    // Process and aggregate the data
    const responseData = {
      service1Data: service1Response.data,
      service2Data: service2Response.data
    };

    // Send the aggregated data as the response
    res.json(responseData);
  } catch (error) {
    console.error('Error fetching data from backend services:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
});

// Start the server
app.listen(port, () => {
  console.log(`BFF layer listening at http://localhost:${port}`);
});

const express = require('express');
const path = require('path');
const fs = require('fs');
const React = require('react');
const ReactDOMServer = require('react-dom/server');
const App = require('./src/App'); // Import your main React component

const app = express();
const port = 5000; // You can choose any available port number

// Serve the static files from the React app build directory
app.use(express.static(path.join(__dirname, 'build')));

// Define your BFF API routes here
app.get('/api/data', (req, res) => {
  // Handle the BFF API request and send a response
  res.json({ message: 'Hello from the BFF layer!' });
});

// For all other routes, serve the React app
app.get('*', (req, res) => {
  // Read the HTML file generated by the React app build
  const filePath = path.join(__dirname, 'build', 'index.html');
  fs.readFile(filePath, 'utf8', (err, htmlData) => {
    if (err) {
      console.error('Error reading HTML file', err);
      return res.status(500).end();
    }

    // Render the React app on the server
    const appHtml = ReactDOMServer.renderToString(<App />);

    // Inject the server-rendered React app into the HTML
    const hydratedHtml = htmlData.replace(
      '<div id="root"></div>',
      `<div id="root">${appHtml}</div>`
    );

    // Send the HTML response to the client
    res.send(hydratedHtml);
  });
});

// Start the server
app.listen(port, () => {
  console.log(`BFF layer is running on http://localhost:${port}`);
});


npm install @babel/register @babel/preset-env @babel/preset-react --save-dev
