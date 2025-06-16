# kong-js-pdk

[![Node.js CI](https://github.com/Kong/kong-js-pdk/actions/workflows/node.js.yml/badge.svg)](https://github.com/Kong/kong-js-pdk/actions/workflows/node.js.yml)
[![NPM](https://img.shields.io/npm/v/kong-pdk)](https://www.npmjs.com/package/kong-pdk)
[![codecov](https://codecov.io/gh/Kong/kong-js-pdk/branch/main/graph/badge.svg?token=OLN3HEOIVP)](https://codecov.io/gh/Kong/kong-js-pdk)

Plugin server and PDK (Plugin Development Kit) for Javascript language support in Kong.

Requires Kong >= 2.3.0.

## Documentation

This project provides the JavaScript Plugin Development Kit (PDK) for [Kong Gateway](https://developer.konghq.com/gateway/), enabling you to write custom plugins in JavaScript or TypeScript.

It includes:
- A plugin server runtime for JavaScript-based plugins
- Type definitions for TypeScript support
- A promise-based API for implementing phase handlers like `access`, `log`, and more

## Installation

Install the plugin server globally:

```
npm install -g kong-pdk
```

You can also add it to a plugin’s local dependencies:

```
npm install kong-pdk
```

## Plugin Development

A valid JavaScript plugin exports an object like this:

```javascript
module.exports = {
  Plugin: KongPlugin,
  Schema: [
    { message: { type: "string" } },
  ],
  Version: '0.1.0',
  Priority: 0,
}
```

You can implement logic in Kong Gateway phases by defining methods on the plugin class:

```javascript
class KongPlugin {
  constructor(config) {
    this.config = config
  }

  async access(kong) {
    const host = await kong.request.getHeader("host")
    // your logic here
  }
}
```

Supported phase handlers include: `certificate`, `rewrite`, `access`, `response`, `preread`, and `log`.

## TypeScript Support

The `kong-pdk` package includes type definitions. To use TypeScript:

```typescript
import kong from "kong-pdk/kong";
```

In your `package.json`:

```json
{
  "dependencies": {
    "kong-pdk": "^0.5.5"
  }
}
```

## Testing

You can test plugins using [`jest`](https://jestjs.io/). Example setup:

```
npm install --save-dev jest
```

Add this to your `package.json`:

```json
{
  "scripts": {
    "test": "jest"
  },
  "devDependencies": {
    "jest": "^26.6.3",
    "kong-pdk": "^0.3.2"
  }
}
```

Run tests with:

```
npm test
```

## Plugin Server Configuration (Kong Gateway)

Example `kong.conf` setup:

```
pluginserver_names = js

pluginserver_js_start_cmd = /usr/local/bin/kong-js-pluginserver -v --plugins-directory /usr/local/kong/js-plugins
pluginserver_js_query_cmd = /usr/local/bin/kong-js-pluginserver --plugins-directory /usr/local/kong/js-plugins --dump-all-plugins
pluginserver_js_socket = /usr/local/kong/js_pluginserver.sock
```

## More Resources

- [Kong Gateway Plugin Development Docs](https://docs.konghq.com/gateway/latest/plugin-development/)
- [JavaScript PDK Examples](https://github.com/Kong/kong-js-pdk/tree/master/examples)


## TODO

- Better API design for user land (without async/await?)
- Dedicated server per plugin
- Rewrite with typescript
