# SmartUI SDK Sample for K6

Welcome to the SmartUI SDK sample for K6. This repository demonstrates how to integrate SmartUI visual regression testing with K6 browser automation.

## Prerequisites

- K6 installed (see [K6 Installation Guide](https://k6.io/docs/get-started/installation/))
- LambdaTest account credentials (for Cloud tests)
- Node.js installed (for SmartUI CLI)

## Repository Structure

```
smartui-k6-sample/
├── k6-smartui.js              # Cloud test with single scenario
├── smartui-k6-scenarios.js    # Cloud test with multiple scenarios
└── smartui-web.json           # SmartUI config (create with npx smartui config:create)
```

## Quick Start

### Cloud Execution

1. **Clone the repository:**
   ```bash
   git clone https://github.com/LambdaTest/smartui-k6-sample
   cd smartui-k6-sample
   ```

2. **Set your credentials:**
   ```bash
   export LT_USERNAME='your_username'
   export LT_ACCESS_KEY='your_access_key'
   ```

3. **Create SmartUI config (optional):**
   ```bash
   npx smartui config:create smartui-web.json
   ```

4. **Run the test:**
   ```bash
   K6_BROWSER_ENABLED=true k6 run k6-smartui.js
   ```

## Test Files

### Single Scenario Test (`k6-smartui.js`)

- Connects to LambdaTest Cloud using K6 browser automation
- Reads credentials from environment variables (`LT_USERNAME`, `LT_ACCESS_KEY`)
- Takes screenshot with name: `screenshot`
- Uses `smartui.takeScreenshot` action via hooks

### Multiple Scenarios Test (`smartui-k6-scenarios.js`)

- Demonstrates running tests with multiple browser scenarios
- Tests Chrome and Microsoft Edge browsers
- Uses per-VU iterations executor

## Configuration

### Capabilities

The test files include capabilities configuration for LambdaTest Cloud:

```javascript
const capabilities = {
  "browserName": "Chrome",
  "browserVersion": "latest",
  "LT:Options": {
    "platform": "MacOS Ventura",
    "build": "K6 Build",
    "name": "K6 SmartUI test",
    "user": __ENV.LT_USERNAME,
    "accessKey": __ENV.LT_ACCESS_KEY,
    "smartUIProjectName": "K6_Test_Sample",
  }
};
```

### SmartUI Screenshot

To take a screenshot, use the `captureSmartUIScreenshot` function:

```javascript
await captureSmartUIScreenshot(page, "screenshot");
```

This function uses the `smartui.takeScreenshot` action via K6's browser automation hooks.

## View Results

After running the tests, visit your [LambdaTest dashboard](https://automation.lambdatest.com/build) to view the running test and SmartUI project to see captured screenshots.

## More Information

For detailed onboarding instructions, see the [SmartUI K6 Onboarding Guide](https://www.lambdatest.com/support/docs/smartui-onboarding-k6/).

## Notes

- K6 browser automation requires `K6_BROWSER_ENABLED=true` environment variable
- The repository uses hooks-based SmartUI integration (not SDK-based)
- Screenshots are captured using `smartui.takeScreenshot` action via `page.evaluate()`
