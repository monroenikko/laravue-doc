# Build & Deploy

## Build

When projects are completed, you can build your application only run one command:

```bash
# build for production environment
yarn run production
```

### Environmental variables

All test or production environment variables are configured under the `@/build/config` directory.

They all inject into the global context via the `webpack.DefinePlugin` plug-ins.

```js
new webpack.DefinePlugin({
  'process.env': require('../config/xxx.env')
})
```

You can simply get your configuration environment variables directly using your code such as:

```js
// So you can get the base_url configured in @/build/config
const baseURL = process.env.BASE_API,
```

### Analyze the build file size

If your build file is large, you can optimize your code by building and analyzing the size distribution of dependent modules using the `webpack-bundle-analyzer`.

```bash
yarn run report
```

After running you can see the specific size distribution at http://127.0.0.1:8888

![](https://wpimg.wallstcn.com/3fddf034-2b38-4299-b0d2-b748fb2abef0.jpg)

::: tip
It is recommended to use gzip, after using the volume will be only the original 1/3 or so. You can also use lazy loading or Code Splitting.
:::

## Publish

For publishing, you only have to publish the resulting static file after build, which is usually the static file in the `dist` folder, to your cdn or static server. Note that the `index.html` usually will be an entry page for your backend service. You may need to change the page's import path after determining static for JS and css.

::: tip
In deployment may find that the resource path is wrong, just modify the `@/config/index.js` file resource path.
:::

```js
// changes configure depending on your own path
assetsPublicPath: './'
```

### Router & Server

In vue-element-admin, the front-end routing uses `vue-router`, so you have two options:`browserHistory` and `hashHistory`.

Simply speaking, the difference between them is the deal with routing. `hashHistory` is processed by the path following `#`, front-end routing management through [HTML 5 History](https://developer.mozilla.org/en-US/docs/Web/API/History_API), and `browserHistory` is similar to our usual page access path, and with not `#`, but must through the server's configuration.

This project uses `hashHistory` by default, so if you have`#`in your url and you want to get rid of it, you need to switch to`browserHistory`.

Modify `src/router/index.js` mode。

```js
export default new Router({
  // mode: 'history' // Need backend support
})
```

::: tip
Detail see [vue-router document](https://router.vuejs.org/guide/essentials/history-mode.html)
:::
