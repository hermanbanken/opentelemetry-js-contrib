# OpenTelemetry instrumentation for pino

[![NPM Published Version][npm-img]][npm-url]
[![dependencies][dependencies-image]][dependencies-url]
[![devDependencies][devDependencies-image]][devDependencies-url]
[![Apache License][license-image]][license-image]

This module provides automatic instrumentation for injection of trace context to [`pino`](https://www.npmjs.com/package/pino).

## Installation

```bash
npm install --save @opentelemetry/instrumentation-pino
```

## Usage

```js
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { PinoInstrumentation } = require('@opentelemetry/instrumentation-pino');
const { registerInstrumentations } = require('@opentelemetry/instrumentation');

const provider = new NodeTracerProvider();
provider.register();

registerInstrumentations({
  instrumentations: [
    new PinoInstrumentation({
      // Optional hook to insert additional context to log object.
      logHook: (span, record) => {
        record['resource.service.name'] = provider.resource.attributes['service.name'];
      },
    }),
    // other instrumentations
  ],
});

const pino = require('pino');
const logger = pino();
logger.info('foobar');
// {"msg":"foobar","trace_id":"fc30029f30df383a4090d3189fe0ffdf","span_id":"625fa861d19d1056","trace_flags":"01", ...}
```

### Fields added to pino log objects

For the current active span, the following fields are injected:

* `trace_id`
* `span_id`
* `trace_flags`

When no span context is active or the span context is invalid, injection is skipped.

### Supported versions

`>5.14.0` and `6.x`

## Useful links

* For more information on OpenTelemetry, visit: <https://opentelemetry.io/>
* For more about OpenTelemetry JavaScript: <https://github.com/open-telemetry/opentelemetry-js>
* For help or feedback on this project, join us in [GitHub Discussions][discussions-url]

## License

Apache 2.0 - See [LICENSE][license-url] for more information.

[discussions-url]: https://github.com/open-telemetry/opentelemetry-js/discussions
[license-url]: https://github.com/open-telemetry/opentelemetry-js-contrib/blob/main/LICENSE
[license-image]: https://img.shields.io/badge/license-Apache_2.0-green.svg?style=flat
[dependencies-image]: https://status.david-dm.org/gh/open-telemetry/opentelemetry-js-contrib.svg?path=plugins%2Fnode%2Fopentelemetry-instrumentation-pino
[dependencies-url]: https://david-dm.org/open-telemetry/opentelemetry-js-contrib?path=plugins%2Fnode%2Fopentelemetry-instrumentation-pino
[devDependencies-image]: https://status.david-dm.org/gh/open-telemetry/opentelemetry-js-contrib.svg?path=plugins%2Fnode%2Fopentelemetry-instrumentation-pino&type=dev
[devDependencies-url]: https://david-dm.org/open-telemetry/opentelemetry-js-contrib?path=plugins%2Fnode%2Fopentelemetry-instrumentation-pino&type=dev
[npm-url]: https://www.npmjs.com/package/@opentelemetry/instrumentation-pino
[npm-img]: https://badge.fury.io/js/%40opentelemetry%2Finstrumentation-pino.svg
