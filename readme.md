# next-workbox-webpack-plugin [![Build Status](https://travis-ci.org/ragingwind/next-workbox-webpack-plugin.svg?branch=master)](https://travis-ci.org/ragingwind/next-workbox-webpack-plugin)

> Webpack plugin with workbox limited in Next.js to build Progressive Web App. 

<img width="680" src="https://user-images.githubusercontent.com/124117/36341030-4b040398-142b-11e8-9de7-41d3dbe55427.png">

The webpack plugin help you build a Progressive Web App powered by Next.js. Service worker scripts and precache manifest of Next.js's pages and chunks will be generated by workbox. 

## Install

```
$ npm install @pwa/next-workbox-webpack-plugin
```

## Usage

```js
const nextWorkboxWebpackPlugin = require('@pwa/next-workbox-webpack-plugin');

nextWorkboxWebpackPlugin({
  // Optional, you can use workbox-build options. except swDest because of output location is fixed in 'static/workbox'
  ...WorkboxBuildOptions
  // Optional, next.js dist path as compiling. most of cases you don't need to fix it.
  distDir: '.next',
  // Optional, which version of workbox will be used in between 'local' or 'cdn'. 'local'
  // option will help you use copy of workbox libs in localhost.
  importWorkboxFrom: 'local',
  // Optional ,whether make a precache manifest of pages and chunks of Next.js app or not.
  precacheManifest: true,
  // Optional, whether delete workbox path generated by the plugin.
  removeDir: true,
  // Must, you can get it while Next.js app compiling time.
  buildId: '6c071eb6-ab01-47bc-8074-71057afc1f64'
});
```

## Usage in next.config.js

```
const nextWorkboxWebpackPlugin = require('@pwa/next-workbox-webpack-plugin');

module.exports = {
  webpack: (config, {isServer, buildId, config: {distDir}}) => {
    if (!isServer) {
      config.plugins.push(new NextWorkboxWebpackPlugin({
        distDir,
        buildId
      }))
    }

    return config
  }
}
```

## How it works

For Next.js, It contains some of restrictions:

- Only works in not dev mode and on custom server.
- You sould use custom version of server for serving service worker. For your convenience, test server is included with this package.
- All of files will be generated in `static/workbox` because of exporting. It might need to add the path to gitignore.
```
static/workbox
├── next-precache-manifest-d42167a04499e1887dad3156b93e064d.js
├── sw.js
└── workbox-v3.0.0-beta.0
    ├── workbox-background-sync.dev.js
    ├── ...
    ├── workbox-sw.js
```
- You need to make sure that add scripts to register service worker in your application.
- For more information, please refer to test and [Get Started With Workbox For Webpack](https://goo.gl/BFQxuo)

## License

MIT © [Jimmy Moon](https://ragingwind.me)
