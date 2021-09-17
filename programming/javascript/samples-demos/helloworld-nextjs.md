---
layout: default-layout
title: Dynamsoft Barcode Reader for JavaScript - ReactJS Integration Sample
description: Dynamsoft Barcode Reader SDK for JavaScript - ReactJS Integration
keywords: javascript, js, barcode, reactjs
noTitleIndex: true
breadcrumbText: React
---

# JavaScript Hello World Sample - Next.js <svg width="82" height="40" viewBox="0 0 148 90" version="1.1" xmlns:xlink="http://www.w3.org/1999/xlink" style="transform:translateX(4%);shape-rendering:auto"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="#000" fill-rule="nonzero"></path></svg>

[Next.js](https://nextjs.org/) is an open-source development framework built on top of Node.js enabling [React](https://reactjs.org/) based web applications functionalities such as server-side rendering and generating static websites. Follow this guide to learn how to implement Dynamsoft Barcode Reader JavaScript SDK (hereafter called "the library") into a Next.js application.

## Official Sample

* <a target = "_blank" href="https://demo.dynamsoft.com/Samples/DBR/JS/1.hello-world/">Hello World in Next.js - Demo</a>
* <a target = "_blank" href="https://github.com/Dynamsoft/dbr-browser-samples/tree/master/1.hello-world/">Hello World in Next.js - Source Code</a>

## Preparation

Make sure you have [node](https://nodejs.org/) and [yarn](https://yarnpkg.com/cli/install) installed. `node 14.16.0` and `yarn 1.22.10` are used in the example below.

## Create the sample project

### Create a Bootstrapped Raw Next.js Application

```cmd
npx create-next-app read-video-nextjs
```

### **CD** to the root directory of the application and install the dependencies

```cmd
yarn add dynamsoft-javascript-barcode
```

## Start to implement

### Add a file dbr.js at the root of the application to configure the library

```jsx
import DBR from "dynamsoft-javascript-barcode";
DBR.engineResourcePath = "https://cdn.jsdelivr.net/npm/dynamsoft-javascript-barcode@8.6.1/dist/";
export default DBR;
```

> Note:
> * There are multiple settings available for the configuration, here we only set the `engineResourcePath` which is essential for the library to get the necessary resources at runtime.

### Create a directory "components" and create the following files inside it to represent two components

* BarcodeScanner.js
* HelloWorld.js

### Edit the BarcodeScanner component

* In BarcodeScanner.js, add code for initializing and destroying the library.

```jsx
import DBR from "../dbr";
import React from 'react';

class BarcodeScanner extends React.Component {
    constructor(props) {
        super(props);
        this.bDestroyed = false;
        this.pScanner = null;
        this.elRef = React.createRef();
    }
    async componentDidMount() {
        try {
            let scanner = await (this.pScanner = this.pScanner || DBR.BarcodeScanner.createInstance());
            if (this.bDestroyed) {
                scanner.destroy();
                return;
            }
            this.elRef.current.appendChild(scanner.getUIElement());
            await scanner.open();
        } catch (ex) {
            console.error(ex);
        }
    }
    async componentWillUnmount() {
        this.bDestroyed = true;
        if (this.pScanner) {
            (await this.pScanner).destroy();
        }
    }
    shouldComponentUpdate() {
        // Never update UI after mount, dbrjs sdk use native way to bind event, update will remove it.
        return false;
    }
    render() {
        return (
            <div style={{ width: "100%", height: "100%" }} ref={this.elRef}>
            </div>
        );
    }
}

export default BarcodeScanner;
```

> Note:
> * The html code in `render()` and the following code builds the UI for the library.
> 
>   ```jsx
>   this.elRef.current.appendChild(scanner.getUIElement());
>   ```
> 
> * To release resources timely, the `BarcodeScanner` instance is destroyed with the component in the callback `componentWillUnmount`.
> * The component should never update (check the code for `shouldComponentUpdate()`) so that events bound to the UI stay valid.

### Edit the HelloWorld component

* Add the BarcodeScanner component in HelloWorld.js

```jsx
import React from 'react';
import BarcodeScanner from './BarcodeScanner';

class HelloWorld extends React.Component {
    constructor(props) {
        super(props);
    }
    render() {
        return (
            <BarcodeScanner></BarcodeScanner>
        );
    }
}
export default HelloWorld;
```

### Add the HelloWorld component to /pages/index.js

In index.js, import the `HelloWorld` component:

```jsx
import HelloWorld from '../components/HelloWorld'
```

Change the &lt;main&gt; tag like this:

```jsx
<main className={styles.main}>
<div className={styles.UIElement}>
    <HelloWorld title="Welcome to DBRJS Nextjs sample" />
</div>
</main>
```

* Define the style of the "App" element in styles/Home.module.css

```css
.UIElement {
  margin: 2vmin auto;
  text-align: center;
  font-size: medium;
  height: 40vh;
  width: 80vw;
}
```

* Try running the project.

```cmd
yarn dev
```

If you followed all the steps correctly, you will have a working page that turns one of the cameras hooked to or built in your computer or mobile device into a barcode scanner. However, the found barcodes are not displayed anywhere yet. At the same time, there is a short delay for the initialization of the library during which nothing happens and is not user-friendly. The following takes care of these two issues.

### Update `HelloWorld.js`

* Add state values

```jsx
constructor(props) {
    super(props);
    this.state = {
        libLoaded: false,
        resultValue: "",
        bShowScanner: false
    };
}
```

* Add a few functions

```jsx
import DBR from "../dbr";
```

```jsx
async componentDidMount() {
    try {
        //Load the library on page load to speed things up.
        await DBR.BarcodeScanner.loadWasm();
        this.setState(state => {
            state.libLoaded = true;
            return state;
        }, () => {
            this.showScanner();
        });
    } catch (ex) {
        alert(ex.message);
        throw ex;
    }
}    
showScanner = () => {
    this.setState({
        bShowScanner: true
    });
}
appendMessage = (message) => {
    switch (message.type) {
        case "result":
            this.setState(prevState => {
                prevState.resultValue = message.format + ": " + message.text;
                return prevState;
            });
            break;
        case "error":
            this.setState(prevState => {
                prevState.resultValue = message.msg;
                return prevState;
            });
            break;
        default: break;
    }
}
```

> NOTE :
> * The method `loadWasm()` in the function `componentDidMount()` initializes the library in the background. The scanner UI is only shown when the initialization finishes.
> * The method `appendMessage()` is used to show the result text on the page.

* Change the UI

```jsx
render() {
    return (
        <div className="helloWorld" style={{ height: "100%", width: "100%" }}>
            {!this.state.libLoaded ? (<span style={{ fontSize: "x-large" }}>Loading Library...</span>) : ""}
            {this.state.bShowScanner ? (<BarcodeScanner appendMessage={this.appendMessage}></BarcodeScanner>) : ""}
            {this.state.bShowScanner ? (<input type="text" style={{width: "80vw",border: "none",fontSize: "1rem",textAlign: "center"}} value={this.state.resultValue} readOnly={true} id="resultText" />) : ""}
        </div>
    );
}
```

* In `BarcodeScanner.js`, use the event `onFrameRead` and the parent method `appendMessage()` to return the results.

```jsx    
async componentDidMount() {
  try {
      //Omitted code...
      this.elRef.current.appendChild(scanner.getUIElement());
      scanner.onFrameRead = results => {
          for (let result of results) {
              this.props.appendMessage({ format: result.barcodeFormatString, text: result.barcodeText, type: "result" });
              if (result.barcodeText.indexOf("Attention(exceptionCode") !== -1) {
                  this.props.appendMessage({ msg: result.exception.message, type: "error" });
              }
          }
      };
      await scanner.open();
  } catch (ex) {
      this.props.appendMessage({ msg: ex.message, type: "error" });
      console.error(ex);
  }
}
```

> NOTE :
> * The event `onFrameRead` is triggered upon reading of each frame. If barcodes are found on that frame, the results will be returned and shown on the page.

After the above changes, the application is made more user-friendly and the barcode text is displayed on the page right away. You can start implementing your own business workflow and make the application useful.