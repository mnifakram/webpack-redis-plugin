# Webpack Redis Plugin

[![NPM](https://img.shields.io/npm/v/webpack-redis-plugin.svg)](https://www.npmjs.com/package/webpack-redis-plugin)
[![Build Status](https://travis-ci.org/dokmic/webpack-redis-plugin.svg?branch=master)](https://travis-ci.org/dokmic/webpack-redis-plugin)
[![Code Coverage](https://codecov.io/gh/dokmic/webpack-redis-plugin/badge.svg?branch=master)](https://codecov.io/gh/dokmic/webpack-redis-plugin?branch=master)

This webpack plugin provides an ability to save your assets in [Redis](https://redis.io/).

## Install
```bash
npm install --save-dev webpack5-redis-plugin
```

## Usage
In your `webpack.config.js`:
```javascript
const WebpackRedisPlugin = require('webpack5-redis-plugin');

module.exports = {
  entry: {
    page1: [
      './src/page1/index.js',
    ],
    page2: [
      './src/page2/index.js',
    ],
  },

  output: {
    filename: 'js/[name].js',
  },

  // ...

  plugins: [
    new WebpackRedisPlugin({
      config: {
        host: 'redis.example.com',
        password: 'password',
      },
      filter: key => key === 'js/page1.js',
      transform: (key, content) => Object({
        key: key + ':current',
        value: content+'suffix',
      }),
    }),
  ],
};
```

This config tells the plugin to filter out everything `js/page1.js` and save key value pair in redis db

## API

### `options.config`
Redis client configuration. All possible options can be found [here](https://www.npmjs.com/package/redis#options-object-properties).

### `options.filter`
The callback function filters keys/assets that will be set in Redis:

```javascript
Function(
  key: string
): boolean
```

- `key` - the destination file name relative to your output directory.

**Default:** `() => true`

### `options.transform`
The callback function transforms keys and values that will be set in Redis:

```javascript
Function(
  key: string,
  content: string
): { key: string, value: string }
```
- `key` - the destination file name relative to your output directory.
- `content` - UTF-8 file conent

**Default:** `(key, content) => { key, value: content }`
