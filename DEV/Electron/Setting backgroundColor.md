For a complex app, the `ready-to-show` event could be emitted too late, making the app feel slow. In this case, it is recommended to show the window immediately, and use a `backgroundColor` close to your app's background:

```JavaScript
const { BrowserWindow } = require('electron')const win = new BrowserWindow({ backgroundColor: '#2e2c29' })win.loadURL('https://github.com')
```

Note that even for apps that use `ready-to-show` event, it is still recommended to set `backgroundColor` to make the app feel more native.

Some examples of valid `backgroundColor` values include:

```JavaScript
const win = new BrowserWindow()win.setBackgroundColor('hsl(230, 100%, 50%)')win.setBackgroundColor('rgb(255, 145, 145)')win.setBackgroundColor('#ff00a3')win.setBackgroundColor('blueviolet')
```