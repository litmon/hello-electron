
Chapter 1.で完成したところまでのリポジトリの様子はこちら

https://github.com/litmon/hello-electron/tree/chapter-1

# Chapter 1. Electronの導入・起動まで

適当な作業用フォルダを作成

```
$ mkdir hello-electron
$ cd hello-electron
```

`package.json` を作成

```
$ npm init
# いろいろ質問があるので答えていく
# そうすると最小構成でpackage.jsonが作成される
```

`electron` をインストールする
`npm install` 時に `--save-dev` オプションを付けると、 `package.json` にいい感じに追加してくれる

```
$ npm install --save-dev electron
```

`package.json` にelectronを起動させるコマンドを追記する

```
{
  "name": "hello-electron",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "electron ." // <= ここ
  },
  "author": "",
  "license": "MIT",
  "devDependencies": {
    "electron": "^1.4.10"
  }
}
```

`index.js` を作成([electronのチュートリアル](http://electron.atom.io/docs/tutorial/quick-start/) をそのままコピペでOK)
`index.js` は、 `package.json` に記載してある `main` という項目にあるファイル名を参照するため、 `main` の部分が違うのなら同じになるようにする
これがelectronのエントリポイントになる

```
const {app, BrowserWindow} = require('electron')
const path = require('path')
const url = require('url')

// Keep a global reference of the window object, if you don't, the window will
// be closed automatically when the JavaScript object is garbage collected.
let win

function createWindow () {
  // Create the browser window.
  win = new BrowserWindow({width: 800, height: 600})

  // and load the index.html of the app.
  win.loadURL(url.format({
    pathname: path.join(__dirname, 'index.html'),
    protocol: 'file:',
    slashes: true
  }))

  // Open the DevTools.
  win.webContents.openDevTools()

  // Emitted when the window is closed.
  win.on('closed', () => {
    // Dereference the window object, usually you would store windows
    // in an array if your app supports multi windows, this is the time
    // when you should delete the corresponding element.
    win = null
  })
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.on('ready', createWindow)

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
  if (win === null) {
    createWindow()
  }
})

// In this file you can include the rest of your app's specific main process
```

`index.html` を作成
`index.js` 内で、loadURLで表示するHTMLファイルを読み込んでいる(下記部分)

```
  win.loadURL(url.format({
    pathname: path.join(__dirname, 'index.html'),
    protocol: 'file:',
    slashes: true
  }))
```

今回はチュートリアルをそのままコピペしているので `index.html` を作成

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using node <script>document.write(process.versions.node)</script>,
    Chrome <script>document.write(process.versions.chrome)</script>,
    and Electron <script>document.write(process.versions.electron)</script>.
  </body>
</html>
```

ここまで来たらすべての準備が完了したので、先ほど `package.json` に追加した `start` を使ってelectronを動かす

```
$ npm start
```

実際に動いている様子はこちら

![](/images/chapter-1-1.png)

