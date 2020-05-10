# ELECTRON

## WHAT IS IT

Electronjs is an open source platform which is used to develop cross platform complex application.
it is based on a chromium engine onto which one load "html" pages. It allows for building very complex application based on js/html/js

one can download it all from : https://www.electronjs.org/

## SETUP

Electron is to be installed in the very projet you want to create the application in.

1. create your "app"
````
npm init
````
2. create npm entry point for electron
   edit the package.json from the app
   ````json
   {
  "name": "your-app",
  "version": "0.1.0",
  "main": "main.js",
  "scripts": {
    "start": "node .",
    "start-electron": "electron ."
    }
    }
   ````
3. then
   ````
   npm install --save-dev electron
   ````

## HELLO WORLD
   4. create main.js and index.html files in the root folder of the app
   
   5. content of main.js is creating the window with some definition
````js
   const { app, BrowserWindow } = require('electron')

function createWindow () {
  // Create the browser window.
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  // and load the index.html of the app.
  win.loadFile('index.html')

  // Open the DevTools.
  win.webContents.openDevTools()
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.whenReady().then(createWindow)

// Quit when all windows are closed.
app.on('window-all-closed', () => {
  // On macOS it is common for applications and their menu bar
  // to stay active until the user quits explicitly with Cmd + Q
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  // On macOS it's common to re-create a window in the app when the
  // dock icon is clicked and there are no other windows open.
  if (BrowserWindow.getAllWindows().length === 0) {
    createWindow()
  }
})

// In this file you can include the rest of your app's specific main process
// code. You can also put them in separate files and require them here. 
````

then on can easily execute the application with 
````
npm run start-electron
````



## MORE DETAILS
### WITH ANGULAR
use of angular for front end and navig


### WITH REACT

### WITH THREE.JS

#### add the three editor in it
