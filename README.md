# improved-axe-html-reporter

This package generates a detailed HTML accessibility report from Axe-core® library results, focusing on usage with Playwright. The report includes violations, passes, incomplete, and incompatible results, and can embed screenshots of specific issues.

## Features
- Creates a detailed HTML report from Axe-core results.
- Supports embedding screenshots for violations.
- Customizable with options for file name, output directory, and project key.
- Intended for Playwright integration.

## Installation

Install the package using npm:

```
npm install -D improved-axe-html-reporter
```

## Usage with Playwright

### Example

```javascript
import { test, expect } from '@playwright/test';
import path from 'path';
import fs from 'fs';
import AxeBuilder from '@axe-core/playwright';
import { createHtmlReport } from 'improved-axe-html-reporter';
import { captureScreenshotsForViolations } from '../utils/axe/screenshot';

// Playwright Test Example
test('Accessibility scan with screenshots', async ({ page }, testInfo) => {
    const url = testInfo.project.use.baseURL;
    await page.goto(url);

    // Create output folder
    const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
    const outputDir = path.resolve('a11y-reports', timestamp);
    fs.mkdirSync(outputDir, { recursive: true });

    // Run Axe accessibility scan
    const results = await new AxeBuilder({ page }).analyze();
    await captureScreenshotsForViolations(page, results, outputDir);

    // Generate HTML report
    createHtmlReport({
        results,
        options: {
            outputDir,
            reportFileName: 'a11y-report.html',
            projectKey: 'Accessibility Test'
        }
    });

    expect(results.violations.length).toBe(0);
});
```

## Options
- `outputDir`: Specifies the directory to save the HTML report.
- `reportFileName`: Sets a custom name for the report file.
- `projectKey`: Adds a project identifier to the report header.
- `customSummary`: Allows adding custom HTML content to the summary.

## License
This project is licensed under the MIT License. It is a customized version of the original axe-html-reporter by Liliia Pelypenko.
