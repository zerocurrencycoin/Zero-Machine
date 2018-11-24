## Ƶero Machine

Ƶero Blockchain Visualizer using Three.js, Cannon.js and Insight API. www.zeromachine.io

![alt text](https://github.com/zerocurrencycoin/Zero-Machine/blob/master/zeromachine.png)

### Build

Clone the repo:

```
git clone https://github.com/zerocurrencycoin/Zero-Machine.git
```

Install npm packages:
```
cd Zero-Machine && npm install
```

Build:
```
npm run build
```

To run the visualizer after build, copy the whole folder to a local or remote webserver and open index.html from a browser.

Keys: you can press 'q' to manually cycle through quality settings.

### Overview

When the page loads, the ƵeroMachine object in  [/src/dash-machine.js](https://github.com/zerocurrencycoin/dash-machine/blob/master/src/dash-machine.js)  is created which runs the visualization in an HTML5 canvas passed into the constructor.

A benchmark is run for 3 seconds behind a loading screen at start to determine the device FPS (and later tune the graphics and physics settings appropriately), preload all the assets and get the best block and its tx from Insight.

After preload the best block and tx are added to the scene as a starting point and then we listen for new unconfirmed tx and confirmed blocks from a websocket hooked into Insight using socket.io, defaulted to https://insight.zerocurrency.io/insight/

When a new block hash is seen we re-query Insight to get the block height and a cube is added with the height written on each face and to the html HUD including a timer that just increments since the last block was received.

When new unconfirmed tx are seen we add a sphere for each output, with the size roughly calculated using the log of the spherical volume with r = txo-amount, and different textures for amounts falling between the limits <1, <10, <100, <1000.  

During visualization if no network activity is detected within any (default) 30 second period, Insight is pinged for the best block hash to check if it's still alive - if not, the loader screen is shown again until a connection is re-established.

If your browser have trouble loading the images and textures, please see this link about an issue with CORS:

https://www.thepolyglotdeveloper.com/2014/08/bypass-cors-errors-testing-apis-locally/

Based on dash-machine by Andy Freer (https://github.com/andyfreer/dash-machine).


### TX Key:

<1, = less than 1 Zer = `Smallest Ball - [Complete Yellow]`

<10 = less than 10 Zer = `Small Ball - [Black Band]`

<100 = less than 100 Zer = `Medium Ball - [Yellow Band]`

<1000 = less than 1000 Zer = `Largest Ball - [Yellow Band]`
