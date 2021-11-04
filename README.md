# Converting Web-to-desktop ApiRTC-angular application with Electron

Tutorial for building an ApiRTC/WebRTC Desktop app from web app available at https://github.com/ApiRTC/ApiRTC-angular.

# Clone ApiRTC-angular app

Clone with renaming with electron suffix if you already have cloned ApiRTC-angular :

`git clone https://github.com/ApiRTC/ApiRTC-angular.git ApiRTC-angular-electron`

`cd ApiRTC-angular-electron`

`npm install`

Install Electron :

`npm install electron --save-dev`

## Electron-ize the app

Edit `src/index.html` to modify `<base href`:

    <base href="./">

Create `main.js` in the project root (not in src):

    const { app, BrowserWindow } = require('electron')
    const path = require('path')
    const url = require('url')
    let win
    function createWindow() {
      win = new BrowserWindow({ width: 1024, height: 720 })
      // load the dist folder from Angular 
      win.loadURL(url.format({ pathname: path.join(__dirname, 'dist/ApiRTC-angular/index.html'), protocol: 'file:', slashes: true }))
      // Open the DevTools optionally: 
      // win.webContents.openDevTools() 
      win.on('closed', () => { win = null })
    }
    app.on('ready', createWindow)
    app.on('window-all-closed', () => { if (process.platform !== 'darwin') { app.quit() } })
    app.on('activate', () => { if (win === null) { createWindow() } })

In `package.json`, add:

    "main":"main.js",
    "scripts": {
      ...
      "electron":"electron .",
      "electron-build":"ng build --configuration production && electron ."
    }

## Build and run

`npm run electron-build`



