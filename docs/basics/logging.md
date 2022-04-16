# Logging

Lesgo! is configured with structured logging.

Structured logs will appear on the console by default.

```js
import logger from "Lesgo/Utils/logger";

logger.log("info", "this is an info log");
logger.info("This is an info log");
logger.warn("This is a warning log");
logger.error("This is an error log");
logger.debug(
  "This is a debug log and will only get logged when APP_DEBUG=true"
);
```

!!! info "PRO TIP"

    This logger is built for, and works well, with both CloudWatch and DataDog. DataDog will help you monitor your logs, errors, inovations, and alot more in almost real-time with no performance impact!

You may also add additional custom metadata as such:

```js
logger.info("This is an info log with my own custom metadata", {
  customData1: "someData1",
  customData2: "someData2",
});
```
