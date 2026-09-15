# Playwright Testing Patterns & Recipes

## 1. Web-First Locators Hierarchy

```typescript
// 1. Role-based (Preferred)
page.getByRole('button', { name: 'Save changes' });
page.getByRole('heading', { name: 'Dashboard' });

// 2. Form labels
page.getByLabel('Password');

// 3. Placeholders
page.getByPlaceholder('Search products...');

// 4. Visible text
page.getByText('Account created successfully');

// 5. Test ID (Fallback)
page.getByTestId('custom-chart-canvas');
```

---

## 2. Waiting Recipes

```typescript
// Auto-waiting assertions (DO THIS)
await expect(page.getByRole('dialog')).toBeVisible();
await expect(page.getByRole('button', { name: 'Submit' })).toBeEnabled();

// NEVER DO THIS:
// await page.waitForTimeout(3000); // BANNED
```

---

## 3. Accessibility Testing with axe-core

```typescript
import AxeBuilder from '@axe-core/playwright';

test('Page is accessible', async ({ page }) => {
  await page.goto('/');
  const results = await new AxeBuilder({ page }).analyze();
  expect(results.violations).toEqual([]);
});
```
