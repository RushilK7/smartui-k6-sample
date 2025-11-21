# SmartUI SDK Sample for K6

Welcome to the SmartUI SDK sample for K6. This repository demonstrates how to integrate SmartUI visual regression testing with K6 browser automation.

## Repository Structure

```
smartui-k6-sample/
├── k6-smartui.js              # Cloud test with single scenario
├── smartui-k6-scenarios.js    # Cloud test with multiple scenarios
└── smartui-web.json           # SmartUI config (create with npx smartui config:create)
```

## 1. Prerequisites and Environment Setup

### Prerequisites

- K6 installed (see [K6 Installation Guide](https://k6.io/docs/get-started/installation/))
- LambdaTest account credentials
- Node.js installed (for SmartUI CLI, optional)

### Install K6

**macOS:**
```bash
brew install k6
```

**Linux:**
```bash
sudo gpg -k
sudo gpg --no-default-keyring --keyring /usr/share/keyrings/k6-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69
echo "deb [signed-by=/usr/share/keyrings/k6-archive-keyring.gpg] https://dl.k6.io/deb stable main" | sudo tee /etc/apt/sources.list.d/k6.list
sudo apt-get update
sudo apt-get install k6
```

**Windows:**
Download the installer from [K6 Installation Guide](https://k6.io/docs/get-started/installation/)

**Verify installation:**
```bash
k6 version
```

### Environment Setup

```bash
export LT_USERNAME='your_username'
export LT_ACCESS_KEY='your_access_key'
```

## 2. Initial Setup and Dependencies

### Clone the Repository

```bash
git clone https://github.com/LambdaTest/smartui-k6-sample
cd smartui-k6-sample
```

### Create SmartUI Configuration (Optional)

For K6 browser automation with hooks, the SmartUI config is optional. The `smartUIProjectName` in capabilities is used instead.

```bash
npx smartui config:create smartui-web.json
```

## 3. Steps to Integrate Screenshot Commands into Codebase

The SmartUI screenshot function is already implemented in the repository using hooks-based integration.

**Test File** (`k6-smartui.js`):
```javascript
import { chromium } from 'k6/experimental/browser';

await page.goto("https://www.lambdatest.com");

// Take screenshot using SmartUI hooks
await captureSmartUIScreenshot(page, "screenshot");

async function captureSmartUIScreenshot(page, screenshotName) {
  await page.evaluate(_ => {}, `lambdatest_action: ${JSON.stringify({ 
    action: "smartui.takeScreenshot", 
    arguments: { screenshotName: screenshotName } 
  })}`);
}
```

**Note**: 
- The code is already configured and ready to use
- You can modify the URL and screenshot name if needed
- Update `smartUIProjectName` in capabilities to match your SmartUI project name
- The test uses K6 browser automation with hooks-based SmartUI integration

## 4. Execution and Commands

Execute visual regression tests on SmartUI using the following command:

```bash
K6_BROWSER_ENABLED=true k6 run k6-smartui.js
```

**Note**: 
- `K6_BROWSER_ENABLED=true` is required to enable browser automation in K6
- Navigate to the [LambdaTest dashboard](https://automation.lambdatest.com/build) to view the running test
- Visit your SmartUI project to see captured screenshots

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

The test files include capabilities configuration for LambdaTest Cloud. Update `smartUIProjectName` to match your SmartUI project name:

```javascript
const capabilities = {
  "browserName": "Chrome",
  "browserVersion": "latest",
  "LT:Options": {
    "user": __ENV.LT_USERNAME,
    "accessKey": __ENV.LT_ACCESS_KEY,
    "smartUIProjectName": "K6_Test_Sample",  // Update this to match your project
  }
};
```

## View Results

After running the tests, visit your [LambdaTest dashboard](https://automation.lambdatest.com/build) to view the running test and SmartUI project to see captured screenshots.

## More Information

For detailed onboarding instructions, see the [SmartUI K6 Onboarding Guide](https://www.lambdatest.com/support/docs/smartui-onboarding-k6/).

## Notes

- K6 browser automation requires `K6_BROWSER_ENABLED=true` environment variable
- The repository uses hooks-based SmartUI integration (not SDK-based)
- Screenshots are captured using `smartui.takeScreenshot` action via `page.evaluate()`
