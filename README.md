# vue-qrcode
A component to draw QRcode with logo and other attributes.

this component is base on [vue-qriously](https://github.com/neocotic/qrious).

## Installation
- #### NPM / Yarn
  Run `npm install --save vue-qriously`, or `yarn add vue-qriously`

- #### With Modules

  ``` js
  // ES6
  import Vue from 'vue'
  import VueQriously from 'vue-qriously'
  Vue.use(VueQriously)

  // ES5
  var Vue = require('vue')
  Vue.use(require('vue-qriously').default)
  ```

- #### `<script>` Include

  Just include `./dist/vue-qriously.js` after Vue itself.

## Usage

``` html
import QrCode from "@c/public/QrCode.vue";

...

<qr-code class="share-qrcode" value="this is qr code content" title="Hello World" desc="this is qr code description " :logo="require('@/assets/logo.svg')" :size="200" :gap="24"/>
```


## License

[MIT](http://opensource.org/licenses/MIT)
