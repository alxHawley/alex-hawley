---
layout: default
title: TypeScript & Playwright
permalink: /test-automation/typescript-playwright
---

# TypeScript Test Automation with Playwright Framework
[GitHub Repository](https://github.com/alxHawley/typescript-playwright)

This project demonstrates modern test automation using TypeScript and Playwright, combining end-to-end browser testing with comprehensive API validation. Built with enterprise-grade practices, this framework showcases advanced testing capabilities including parallel execution, cross-browser compatibility, and CI/CD integration with GitHub Actions.

## Project Overview

This TypeScript-based automation framework provides a robust testing solution for both web applications and APIs, implementing modern testing patterns with type safety and comprehensive validation. The project demonstrates scalable architecture suitable for complex testing scenarios and continuous integration pipelines.

## Technical Stack

### Core Technologies
- **Playwright**: Modern browser automation framework with multi-browser support
- **TypeScript**: Strongly typed JavaScript for enhanced code quality and maintainability
- **GitHub Actions**: Automated CI/CD pipeline with containerized testing environment
- **Docker Integration**: Containerized API testing with restful-booker service

### Key Features
- **Cross-browser Testing**: Chrome, Firefox, Safari, and Edge compatibility testing
- **Parallel Test Execution**: Concurrent test running for optimal performance
- **API & E2E Integration**: Comprehensive testing coverage across all application layers
- **Type Safety**: Full TypeScript implementation with schema validation
- **CI/CD Ready**: GitHub Actions workflow with automated test reporting

## Framework Architecture

The framework separates concerns with dedicated directories for API testing, E2E testing, and shared utilities, enabling maintainable and scalable test development.

### End-to-End Testing
Example from `src/tests/e2e/auth.spec.ts` demonstrating user authentication flows:

```typescript
import { test, expect } from '@playwright/test';
import { users } from '../../utils/sauce.users';

test.describe('Authentication Tests', () => {
  test('successful login with valid credentials', async ({ page }) => {
    await page.goto('/');
    
    // Login with standard user credentials
    await page.fill('[data-test="username"]', users.standard.username);
    await page.fill('[data-test="password"]', users.standard.password);
    await page.click('[data-test="login-button"]');
    
    // Verify successful login
    await expect(page).toHaveURL('/inventory.html');
    await expect(page.locator('.title')).toHaveText('Products');
  });

  test('login failure with invalid credentials', async ({ page }) => {
    await page.goto('/');
    
    // Attempt login with invalid credentials
    await page.fill('[data-test="username"]', 'invalid_user');
    await page.fill('[data-test="password"]', 'invalid_password');
    await page.click('[data-test="login-button"]');
    
    // Verify error message
    await expect(page.locator('[data-test="error"]'))
      .toHaveText('Epic sadface: Username and password do not match any user in this service');
  });
});
```

### API Testing Implementation
Comprehensive API testing from `src/tests/api/restfulbooker.spec.ts`:

```typescript
import { test, expect } from '@playwright/test';
import { apiConfig } from '../../utils/api.config';
import { bookingSchema } from '../../utils/schemas/booking.schema';

test.describe('RESTful Booker API Tests', () => {
  let bookingId: number;
  let authToken: string;

  test.beforeAll(async ({ request }) => {
    // Authenticate and get token
    const authResponse = await request.post(`${apiConfig.baseURL}/auth`, {
      data: {
        username: apiConfig.credentials.username,
        password: apiConfig.credentials.password
      }
    });
    const authData = await authResponse.json();
    authToken = authData.token;
  });

  test('create new booking', async ({ request }) => {
    const bookingData = {
      firstname: 'John',
      lastname: 'Doe',
      totalprice: 150,
      depositpaid: true,
      bookingdates: {
        checkin: '2024-01-01',
        checkout: '2024-01-07'
      },
      additionalneeds: 'Breakfast'
    };

    const response = await request.post(`${apiConfig.baseURL}/booking`, {
      data: bookingData
    });

    expect(response.status()).toBe(200);
    const responseBody = await response.json();
    
    // Validate response structure
    expect(responseBody).toHaveProperty('bookingid');
    expect(responseBody.booking).toMatchObject(bookingData);
    
    bookingId = responseBody.bookingid;
  });

  test('retrieve booking by ID', async ({ request }) => {
    const response = await request.get(`${apiConfig.baseURL}/booking/${bookingId}`);
    
    expect(response.status()).toBe(200);
    const booking = await response.json();
    
    // Schema validation
    expect(booking).toMatchSchema(bookingSchema);
    expect(booking.firstname).toBe('John');
    expect(booking.lastname).toBe('Doe');
  });
});
```

## Advanced Testing Capabilities

### TypeScript Configuration
Comprehensive type safety with `tsconfig.json` and schema validation:

```typescript
// utils/schemas/booking.schema.ts
export const bookingSchema = {
  type: 'object',
  required: ['firstname', 'lastname', 'totalprice', 'depositpaid', 'bookingdates'],
  properties: {
    firstname: { type: 'string' },
    lastname: { type: 'string' },
    totalprice: { type: 'number' },
    depositpaid: { type: 'boolean' },
    bookingdates: {
      type: 'object',
      required: ['checkin', 'checkout'],
      properties: {
        checkin: { type: 'string', format: 'date' },
        checkout: { type: 'string', format: 'date' }
      }
    }
  }
};
```

### Configuration Management
Centralized configuration with environment-specific settings:

```typescript
// utils/api.config.ts
export const apiConfig = {
  baseURL: process.env.API_BASE_URL || 'http://localhost:3001',
  credentials: {
    username: 'admin',
    password: 'password123'
  },
  timeout: 30000,
  retries: 3
};
```

## Project Structure

```
typescript-playwright/
├── .github/workflows/
│   └── playwright.yml           # GitHub Actions CI/CD pipeline
├── src/
│   ├── tests/
│   │   ├── api/
│   │   │   └── restfulbooker.spec.ts  # API test suite
│   │   └── e2e/
│   │       ├── auth.spec.ts           # Authentication tests
│   │       └── purchase.spec.ts       # E2E purchase flow
│   └── utils/
│       ├── api.config.ts              # API configuration
│       ├── sauce.users.ts             # Test user data
│       └── schemas/
│           └── booking.schema.ts      # Response schema validation
├── playwright.config.ts               # Playwright configuration
├── tsconfig.json                     # TypeScript configuration
└── package.json                      # Dependencies and scripts
```

## CI/CD Integration

### GitHub Actions Workflow
Automated testing with containerized environment from `.github/workflows/playwright.yml`:

- **Docker Integration**: RESTful Booker API container for consistent testing
- **Multi-browser Execution**: Parallel testing across Chrome, Firefox, and Safari
- **Test Reporting**: Automated test results and artifact generation
- **Environment Flexibility**: Configurable API endpoints for different environments

### Local Development Options
```bash
# Option 1: Local Docker container
docker run -p 3001:3001 mwinteringham/restfulbooker

# Option 2: Remote API endpoint
# Update api.config.ts baseURL to 'https://restful-booker.herokuapp.com/'
```

## Development Methodology

This project demonstrates expertise in:
- **Modern TypeScript practices** - Strong typing, interfaces, and advanced language features
- **Enterprise testing patterns** - Scalable architecture with separation of concerns
- **Cross-platform automation** - Multi-browser and multi-environment compatibility
- **API testing proficiency** - RESTful service validation with schema verification
- **DevOps integration** - CI/CD pipeline implementation with containerized testing

## Testing Coverage

### E2E Test Scenarios
- **User Authentication**: Login/logout flows with validation
- **E-commerce Workflows**: Complete purchase journey testing
- **UI Interaction**: Form validation and user experience testing
- **Cross-browser Compatibility**: Consistent behavior validation

### API Test Scenarios
- **CRUD Operations**: Complete booking lifecycle management
- **Authentication**: Token-based security testing
- **Data Validation**: Schema verification and boundary testing
- **Error Handling**: Comprehensive negative testing scenarios

## Performance & Reliability Features

- **Parallel Execution**: Concurrent test running for faster feedback
- **Auto-retry Logic**: Configurable retry mechanisms for flaky tests
- **Screenshot Capture**: Automatic failure documentation
- **Test Isolation**: Independent test execution preventing cross-test dependencies

## Future Enhancements

**Visual Regression Testing**: Integration of Playwright's visual comparison capabilities for UI consistency validation across releases.

**Performance Monitoring**: Implementation of web vitals and performance metrics collection during E2E test execution.

---

*This project showcases modern TypeScript-based test automation practices, demonstrating proficiency in contemporary testing frameworks and enterprise-grade CI/CD integration.*
