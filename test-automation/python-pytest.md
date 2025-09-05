---
layout: default
title: Python & Pytest
permalink: /test-automation/python-pytest
---

# Python API & UI Testing with Pytest
[GitHub Repository](https://github.com/alxHawley/python-pytest)

A comprehensive, fast pytest-based test suite covering both API testing for the [Restful Booker API](https://restful-booker.herokuapp.com/apidoc/index.html) and UI testing for the [Sauce Demo](https://www.saucedemo.com/) application. This project demonstrates a streamlined approach to full-stack testing without the overhead of BDD frameworks, achieving **88% test coverage**.

## Project Overview

This pytest-based testing suite provides a professional implementation of both API and UI testing fundamentals, showcasing modern Python testing practices. The project serves as a comprehensive example of efficient testing approaches, complementing the [BDD framework](./python-bdd) with direct pytest implementation patterns for maximum speed and clarity.

## Technical Stack

### Core Technologies
- **Pytest**: Modern Python testing framework with fixture support and 88% coverage
- **Requests**: HTTP library for API interaction and validation
- **Selenium**: Web browser automation with webdriver-manager for automatic driver management
- **JSON Schema**: Response structure validation and data integrity testing
- **Python 3.8+**: Modern Python practices with type hints and comprehensive error handling

### Testing Features
- **Fast & Lightweight**: Pure pytest with no BDD overhead
- **Comprehensive Coverage**: Tests all major API endpoints and UI workflows
- **Schema Validation**: JSON schema validation using jsonschema
- **Cross-browser Ready**: Chrome with headless mode for CI/CD
- **Environment Configurable**: Easy switching between local Docker and production environments

## Testing Implementation

### API Testing - CRUD Operations
Comprehensive API testing patterns demonstrating essential validation techniques:

```python
import pytest
import requests
from schemas.booking_schema import booking_schema
from src.api_client import APIClient

class TestAPI:
    def test_create_booking(self, api_client):
        """Test creating a new booking record"""
        booking_data = {
            "firstname": "John",
            "lastname": "Doe", 
            "totalprice": 150,
            "depositpaid": True,
            "bookingdates": {
                "checkin": "2024-01-01",
                "checkout": "2024-01-07"
            }
        }
        
        response = api_client.create_booking(booking_data)
        assert response.status_code == 200
        assert "bookingid" in response.json()
    
    def test_get_booking(self, api_client, booking_id):
        """Test retrieving booking by ID with schema validation"""
        response = api_client.get_booking(booking_id)
        assert response.status_code == 200
        
        booking = response.json()
        # Validate against JSON schema
        api_client.validate_booking_schema(booking)
```

### UI Testing - Selenium Automation
Modern Selenium implementation with reliable selectors and proper test isolation:

```python
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

class TestLogin:
    def test_valid_login(self, driver):
        """Test successful login with standard user credentials"""
        driver.get("https://www.saucedemo.com/")
        
        # Use data-test attributes for stable selectors
        driver.find_element(By.CSS_SELECTOR, "[data-test='username']").send_keys("standard_user")
        driver.find_element(By.CSS_SELECTOR, "[data-test='password']").send_keys("secret_sauce")
        driver.find_element(By.CSS_SELECTOR, "[data-test='login-button']").click()
        
        # Verify successful login
        assert "inventory.html" in driver.current_url
        assert driver.find_element(By.CLASS_NAME, "title").text == "Products"
```

## Project Structure

```
python-pytest/
├── src/
│   ├── __init__.py          # Package initialization
│   └── api_client.py        # API client library
├── tests/
│   ├── test_api.py          # Main API test suite (10 tests)
│   ├── test_api_client.py   # API client tests (6 tests)
│   ├── test_ui.py           # UI/Selenium test suite (6 tests)
│   └── conftest.py          # Pytest configuration and fixtures
├── schemas/
│   ├── booking_schema.json  # JSON schema for booking validation
│   └── booking_id_schema.json
├── requirements.txt          # Python dependencies
├── pyproject.toml           # Project configuration
└── README.md               # Comprehensive documentation
```

## Test Coverage

### API Testing Coverage (16 Tests)
- **Health Check** (`/ping`) - API status verification
- **Create Booking** (`POST /booking`) - New booking creation with validation
- **Get All Bookings** (`GET /booking`) - List all booking IDs
- **Get Specific Booking** (`GET /booking/{id}`) - Retrieve booking details
- **Filtered Search** (`GET /booking?firstname=X&lastname=Y`) - Parameter-based searching
- **Update Booking** (`PUT /booking/{id}`) - Full booking updates with authentication
- **Partial Update** (`PATCH /booking/{id}`) - Partial field updates
- **Delete Booking** (`DELETE /booking/{id}`) - Booking removal
- **Authentication** (`POST /auth`) - Token generation and validation
- **Error Scenarios** - Invalid IDs, authentication failures, edge cases

### UI Testing Coverage (6 Tests)
- **Valid Login** - Successful authentication with standard credentials
- **Invalid Login** - Error handling for incorrect credentials  
- **Locked User** - Handling of locked-out user accounts
- **User Logout** - Complete logout flow and session cleanup
- **Performance Testing** - Page load performance monitoring
- **Inventory Display** - Product page functionality and element verification

## Testing Approach

This implementation demonstrates:
- **Dual Testing Strategy**: Comprehensive API and UI coverage in one suite
- **Modern Selenium Practices**: Webdriver-manager, stable selectors, proper waits
- **Professional Test Organization**: Class-based structure with function-scoped fixtures
- **Schema Validation**: Automated response structure verification
- **Environment Flexibility**: Docker/production switching with environment variables
- **CI/CD Ready**: Headless browser support and comprehensive reporting

## Performance & Quality Metrics

- **Test Execution Time**: ~58 seconds for full suite (22 tests)
- **Coverage**: 88% code coverage with detailed HTML reports
- **Success Rate**: 21 passed, 1 expected failure (performance test)
- **Browser Compatibility**: Chrome (headless for CI), extensible to other browsers
- **API Response Time**: Built-in timeout handling and performance monitoring

## Comparison with BDD Approach

**Pytest Benefits:**
- **Speed**: Significantly faster test development and execution
- **Directness**: No step definition overhead or abstraction layers
- **Advanced Features**: Excellent fixtures, parametrization, and parallel execution
- **Professional Reporting**: Comprehensive test results with coverage metrics
- **Flexibility**: Easy integration with CI/CD and multiple test types

**BDD Benefits:**
- **Stakeholder Communication**: Human-readable scenarios for business collaboration
- **Documentation**: Living documentation of business requirements
- **Reusability**: Step definitions can be shared across multiple scenarios
- **Cross-functional**: Better communication between technical and non-technical teams

## Quick Start

```bash
# Clone and setup
git clone https://github.com/alxHawley/python-pytest
cd python-pytest
python -m venv venv
source venv/bin/activate  # or .\venv\Scripts\Activate.ps1 on Windows
pip install -r requirements.txt

# Run all tests
python -m pytest tests/ -v

# Run with coverage
python -m pytest tests/ --cov=tests --cov-report=html
```

---

*This project provides a comprehensive, production-ready testing approach that balances speed, maintainability, and thorough coverage across both API and UI testing domains.*
