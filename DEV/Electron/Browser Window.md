Create and control browser windows

Process: Main

This module cannot be used until the ready event of the app module is emitted.

```JavaScript 
const { BrowserWindow } = require('electron')

const win = new BrowserWindow({ width : 800, height : 600 })

//Load a remote URL
win.loadURL('https://github.com')

//Or load a local HTML file
win.loadFile('index.html')
```

## Window Customization
The BrowserWindow class exposes various ways to modify the look and behavior of your app's windows.  For more details, see the [[Window Customization]] page.

## Showing the window gracefully
When loading a page in the window directly, users may see the page load incrementally, which is not a good experience for a native app. To make the window display without a visual flash, there are two solutions for different situations.
### Using the `ready-to-show` event
While loading the page, the `ready-to-show` event will be emitted when the renderer process has rendered the page for the first time if the window has not been shown yet. Showing the window after this event will have no visual flash:
```JavaScript 
const { BrowserWindow } = require('electron')
const win = new BrowserWindow({ show: false })
win.once('ready-to-show', () => {
	win.show()
})
```
This event is usually emitted after the `did-finish-load` event, but for pages with may remote resources, it may be emitted before the `did-finish-load` event
Please note that using this event implies that the renderer will be considered "visible" and paint event through `show` is false. This event will never fire if you use `paintWhenInitiallyHidden: false`

## Setting the `backgroundColor` properly
For a complex app, the `ready-to-show` event could be emitted too late, making the app feel slow. In this case, it is recommended to show the window immediately, and use a `backgroundColor` close to your app's background:
```JavaScript 
const { BrowserWindow } = require('electron')
const win = new BrowserWindow({ backgroundColor: #2e2c29 })
wind.loadURL('https://github.com')
```
Note that even for apps that use `ready-to-show` event, it is still recommended to set `backgroundColor` to make the app feel more native
Some Example of valid `backgroundColor` values include:
```JavaScript
const win = new BrowserWindow()
win.setBackgroundColor('hsl(230, 100%, 50%)')
win.setBackgroundColor('rgb(255, 145, 145)')
win.setBackgroundColor('#ff00a3')
win.setBackgroundColor('blueviolet')
```
For more information about these color types see valid options in [[win.setBackgroundColor]].
## Parent and child windows
By using `parent` option, you can create child windows:
```JavaScript
const { BrowserWindow } = require('electron')
const top = new BrowserWindow()
const child = new BrowserWindo({ parent : top })
child.show()
top.show()
```
The `child` window will always shown on top of the `top` window
## Modal Windows
A modal window is a child window that disables parent window. To create a modal window, you have to set both `parent` and `modal` options:
```JavaScript
const { BrowserWindow } = require('electron')
const top = new BrowserWindow()
const child = new BrowserWindow({ parent: top, modal: true, show: false })
child.loadURL('https://github.com)
child.once('ready-to-show', () => {
	child.show()
})
```
## Page Visibility
https://www.electronjs.org/docs/latest/api/browser-window

continue from here
