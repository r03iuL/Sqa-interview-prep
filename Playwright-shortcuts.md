Here's a solid list of basic Playwright commands, organized by what they do — useful for your interview prep alongside the JD and README you've got in this project.

## Setup & Project Init
```bash
npm init playwright@latest       # Initialize a new Playwright project
npx playwright install           # Install browser binaries (Chromium, Firefox, WebKit)
npx playwright test              # Run all tests
npx playwright test --headed     # Run tests with browser UI visible
npx playwright test --debug      # Run in debug mode (step through)
npx playwright show-report       # Open the HTML report after a run
npx playwright codegen <url>     # Record actions and auto-generate test code
```

## Navigation
```javascript
await page.goto('https://example.com');
await page.goBack();
await page.goForward();
await page.reload();
```

## Locators (user-first, preferred over XPath/CSS)
```javascript
page.getByRole('button', { name: 'Submit' });
page.getByText('Welcome back');
page.getByLabel('Email');
page.getByPlaceholder('Enter your password');
page.getByTestId('login-btn');
page.locator('#some-id');          // CSS fallback
```

## Actions
```javascript
await page.click('selector');
await page.fill('selector', 'text');       // clears + types
await page.type('selector', 'text');       // types char by char
await page.check('selector');              // checkbox/radio
await page.uncheck('selector');
await page.selectOption('selector', 'value');
await page.hover('selector');
await page.press('selector', 'Enter');
await page.setInputFiles('input[type=file]', 'path/to/file.png');
await page.dragAndDrop('source', 'target');
```

## Assertions (web-first, auto-retrying)
```javascript
await expect(page).toHaveURL('/dashboard');
await expect(page).toHaveTitle('My App');
await expect(page.locator('.cart')).toHaveText('3');
await expect(page.locator('button')).toBeVisible();
await expect(page.locator('button')).toBeEnabled();
await expect(page.locator('input')).toBeDisabled();
await expect(page.locator('.error')).toBeHidden();
await expect(page.locator('input')).toHaveValue('test@email.com');
```

## Waiting (rarely needed manually — auto-waiting handles most cases)
```javascript
await page.waitForSelector('selector');
await page.waitForURL('/dashboard');
await page.waitForLoadState('networkidle');
```

## Screenshots & Debugging
```javascript
await page.screenshot({ path: 'screenshot.png' });
await page.pause();               # Opens Playwright Inspector mid-test
```

## Test Structure
```javascript
import { test, expect } from '@playwright/test';

test('user can log in', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill('test@email.com');
  await page.getByLabel('Password').fill('Password123');
  await page.getByRole('button', { name: 'Login' }).click();
  await expect(page).toHaveURL('/dashboard');
});
```

A tip from your own README doc worth reinforcing: always use `await` before every Playwright command, never use `page.waitForTimeout()`, and prefer `getByRole`/`getByTestId` over raw CSS or XPath selectors — these three habits come up constantly in SQA interviews.

Want me to also put together a quick-reference cheat sheet as a downloadable file, or drill you with practice questions on any of these commands?