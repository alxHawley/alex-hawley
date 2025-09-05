---
layout: default
title: JavaScript & BDD
permalink: /test-automation/js-bdd
---

# JavaScript BDD: Test Automation with Cypress and Cucumber
[GitHub Repository](https://github.com/alxHawley/js-cypress-cucumber)

This project demonstrates behavior-driven development (BDD) test automation using JavaScript, Cypress, and the Cucumber framework. Building on the foundation established in my Python BDD work, this implementation showcases cross-language automation proficiency and modern JavaScript testing practices.

## Project Overview

This JavaScript BDD framework replicates and extends the testing patterns from my [Python Behave project](https://github.com/alxHawley/python-behave), providing a comparative study of BDD implementation across different technology stacks. The project focuses on end-to-end web application testing with robust API testing capabilities.

## Technical Stack

### Core Technologies
- **Cypress**: Modern end-to-end testing framework with real browser automation
- **Cucumber**: BDD framework supporting Gherkin syntax for human-readable test scenarios  
- **JavaScript ES6+**: Modern JavaScript features and async/await patterns
- **Node.js**: Runtime environment with npm package management

### Key Features
- **Cross-browser Testing**: Comprehensive browser compatibility validation
- **API Integration**: HTTP request testing and response validation
- **DOM Interaction**: Advanced element manipulation and assertion capabilities
- **Real-time Debugging**: Cypress Test Runner with time-travel debugging

## BDD Framework Architecture

The framework follows standard BDD patterns with Gherkin feature files paired to JavaScript step definitions, promoting collaboration between technical and non-technical stakeholders.

### Feature Definition
Example from `cypress/integration/features/login.feature`:

```gherkin
Feature: User Authentication
  As a user of the application
  I want to authenticate with valid credentials
  So that I can access protected features

  Scenario: Successful login with valid credentials
    Given I am on the login page
    When I enter valid username and password
    And I click the login button
    Then I should be redirected to the dashboard
    And I should see a welcome message
```

### Step Implementation
Corresponding JavaScript step definitions in `cypress/support/step_definitions/`:

```javascript
import { Given, When, Then } from "@badeball/cypress-cucumber-preprocessor";

Given("I am on the login page", () => {
  cy.visit("/login");
  cy.url().should("include", "/login");
});

When("I enter valid username and password", () => {
  cy.get('[data-cy="username"]').type("testuser");
  cy.get('[data-cy="password"]').type("password123");
});

When("I click the login button", () => {
  cy.get('[data-cy="login-btn"]').click();
});

Then("I should be redirected to the dashboard", () => {
  cy.url().should("include", "/dashboard");
  cy.get('[data-cy="dashboard"]').should("be.visible");
});
```

## Cypress Integration Benefits

### Advanced Testing Capabilities
- **Time-travel Debugging**: Visual command log with DOM snapshots
- **Automatic Waiting**: Intelligent retry logic eliminates flaky tests
- **Network Stubbing**: Request/response mocking for isolated testing
- **Real Browser Environment**: Native browser interaction without WebDriver

### Modern JavaScript Features
```javascript
// Async/await pattern for complex workflows
const performLogin = async (credentials) => {
  await cy.get('[data-cy="username"]').type(credentials.username);
  await cy.get('[data-cy="password"]').type(credentials.password);
  await cy.get('[data-cy="login-btn"]').click();
};

// Custom commands for reusable functionality
Cypress.Commands.add('login', (username, password) => {
  cy.session([username, password], () => {
    cy.visit('/login');
    cy.get('[data-cy="username"]').type(username);
    cy.get('[data-cy="password"]').type(password);
    cy.get('[data-cy="login-btn"]').click();
    cy.url().should('include', '/dashboard');
  });
});
```

## Project Structure

```
js-cypress-cucumber/
├── cypress/
│   ├── integration/
│   │   └── features/          # Gherkin feature files
│   ├── support/
│   │   ├── step_definitions/  # JavaScript step implementations
│   │   ├── commands.js        # Custom Cypress commands
│   │   └── index.js          # Global configuration
│   ├── fixtures/             # Test data and mock responses
│   └── plugins/              # Cypress plugins and preprocessors
├── cypress.config.js         # Cypress configuration
└── package.json             # Dependencies and scripts
```

## Development Methodology

This project demonstrates proficiency in:
- **Cross-language automation expertise** - Implementing identical test scenarios across Python and JavaScript
- **Modern testing practices** - Leveraging latest Cypress features and JavaScript ES6+ syntax
- **Framework architecture** - Designing maintainable, scalable test automation solutions
- **BDD collaboration** - Facilitating communication between technical and business stakeholders

## Future Enhancements

**API Testing Integration**: Expanding the framework to include comprehensive API testing capabilities similar to the Python implementation, utilizing Cypress's native HTTP request functionality.

**Cross-browser CI/CD**: Implementation of continuous integration with multiple browser configurations for comprehensive compatibility testing.

---

*This project showcases modern JavaScript test automation practices and demonstrates the versatility of BDD frameworks across different programming languages and testing tools.*