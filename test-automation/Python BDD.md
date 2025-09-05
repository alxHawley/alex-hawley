---
layout: default
title: Python & BDD
permalink: /test-automation/python-bdd
---

# Python BDD: Test Automation with Behave Framework, Requests, and Selenium
[GitHub Repository](https://github.com/alxHawley/python-behave)

This project demonstrates comprehensive Python-based behavior-driven development (BDD) test automation using the Behave framework, combining API testing with Requests and UI testing with Selenium. As my foundational automation framework, this project reflects professional testing practices and serves as the basis for my [JavaScript BDD implementation](https://github.com/alxHawley/js-cypress-cucumber).

## Project Overview

This Python BDD framework provides a robust foundation for both API and UI test automation, implementing industry-standard testing patterns with comprehensive CRUD operations, data validation, and cross-browser compatibility. The framework demonstrates scalable architecture suitable for enterprise-level testing initiatives.

*Special acknowledgment to [Aziz Ettobi](https://www.linkedin.com/in/azizettobi/) ([GitHub](https://github.com/aettobi)) for introducing me to BDD principles and providing mentorship that shaped my automation engineering approach.*

## Technical Stack

### Core Technologies
- **Behave**: Python BDD framework supporting Gherkin syntax for readable test scenarios
- **Requests**: HTTP library for comprehensive API testing and validation
- **Selenium WebDriver**: Cross-browser automation for UI testing
- **Python 3.x**: Modern Python features with context management and f-string formatting

### Key Features
- **Full CRUD API Testing**: Complete Create, Read, Update, Delete operation validation
- **Multi-browser Support**: Cross-browser compatibility testing with Chrome, Firefox, Edge
- **Data-driven Testing**: Parameterized scenarios with external data sources
- **Robust Assertions**: Comprehensive response validation and error handling

## BDD Framework Architecture

The framework separates business logic (feature files) from technical implementation (step definitions), enabling collaboration between technical and non-technical stakeholders through human-readable Gherkin syntax.

### Feature Definition
Example from `features/api.feature` demonstrating CRUD operations:

```gherkin
Feature: API CRUD operations  
  As an API client
  I can create, read, update, and delete hotel bookings
  So that I can manage booking data effectively

  Background:
    Given the API is available and responsive

  Scenario: Create and retrieve booking record
    When I create a new booking with valid data
    Then the booking is created successfully
    And the booking ID is returned
    When a GET request is made with the booking ID
    Then the API response returns the correct record details
```

### Step Implementation
Corresponding Python step definitions from `features/steps/api_steps.py`:

```python
@when("a GET request is made with the booking ID")
def step_get_booking(context):
    """Get the booking details using the booking ID"""
    url = f"{BASE_URL}{BOOKING_ENDPOINT}/{context.bookingid}"
    context.response = requests.get(url, headers=headers, timeout=5)
    context.booking = context.response.json()

@then("the API response returns the correct record details")
def step_verify_booking_details(context):
    """Verify that the booking details match expected data"""
    assert context.response.status_code == 200
    
    response_json = context.response.json()
    # Comprehensive data validation
    assert response_json["firstname"] == context.booking["firstname"]
    assert response_json["lastname"] == context.booking["lastname"]
    assert response_json["totalprice"] == context.booking["totalprice"]
    assert response_json["depositpaid"] == context.booking["depositpaid"]
    assert (
        response_json["bookingdates"]["checkin"]
        == context.booking["bookingdates"]["checkin"]
    )
    assert (
        response_json["bookingdates"]["checkout"]
        == context.booking["bookingdates"]["checkout"]
    )
```

## Advanced Testing Capabilities

### API Testing Features
- **RESTful Service Validation**: Complete HTTP method coverage (GET, POST, PUT, DELETE)
- **Response Schema Validation**: JSON structure and data type verification
- **Authentication Testing**: Token-based and session-based authentication flows
- **Error Handling**: Comprehensive negative testing with invalid data scenarios

### UI Testing Integration
```python
# Selenium WebDriver integration for cross-browser testing
@given("I am on the booking management page")
def step_navigate_to_booking_page(context):
    context.driver.get(f"{BASE_URL}/booking")
    assert "Booking Management" in context.driver.title

@when("I fill out the booking form with valid data")
def step_fill_booking_form(context):
    context.driver.find_element(By.ID, "firstname").send_keys("John")
    context.driver.find_element(By.ID, "lastname").send_keys("Doe")
    context.driver.find_element(By.ID, "totalprice").send_keys("150")
```

## Project Structure

```
python-behave/
├── features/
│   ├── api.feature              # API testing scenarios
│   ├── ui.feature               # UI testing scenarios
│   ├── steps/
│   │   ├── api_steps.py         # API step implementations
│   │   ├── ui_steps.py          # UI step implementations
│   │   └── common_steps.py      # Shared step definitions
│   └── environment.py           # Test environment configuration
├── utils/
│   ├── api_helpers.py           # API utility functions
│   ├── data_generators.py       # Test data generation
│   └── browser_factory.py       # WebDriver management
├── config/
│   └── settings.py              # Environment configurations
└── requirements.txt             # Python dependencies
```

## Development Methodology

This project demonstrates expertise in:
- **Enterprise testing patterns** - Scalable framework architecture with separation of concerns
- **API testing proficiency** - Complete RESTful service validation and data integrity testing
- **Cross-browser automation** - Multi-browser compatibility testing with Selenium WebDriver
- **BDD collaboration** - Business-readable test scenarios facilitating stakeholder communication
- **Python best practices** - Context managers, exception handling, and modern Python idioms

## Testing Coverage

### API Test Scenarios
- **CRUD Operations**: Complete lifecycle testing of booking resources
- **Data Validation**: Schema validation and boundary testing
- **Authentication**: Secure access control and session management
- **Error Handling**: Comprehensive negative testing scenarios

### UI Test Scenarios
- **Form Validation**: Input field validation and user interaction flows
- **Navigation Testing**: Page routing and user journey validation
- **Responsive Design**: Cross-device compatibility testing

## Future Enhancements

**Performance Testing Integration**: Expanding the framework to include load testing capabilities using pytest-benchmark for API performance validation.

**CI/CD Pipeline Integration**: Implementation of continuous testing with GitHub Actions, including parallel test execution and comprehensive reporting.

---

*This project establishes the foundation for my test automation expertise, demonstrating proficiency in Python-based testing frameworks and serving as the architectural blueprint for cross-language automation implementations.*