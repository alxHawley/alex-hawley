---
layout: default
title: Python & BDD
permalink: /test-automation/python-bdd
---

# Python BDD: Test Automation with the Behave Framework, Requests, and Selenium
[GitHub Repository](https://github.com/alxHawley/python-behave)

Rather than duplicate the README for the repository I'll tell you why I started this particular project.

For starters, I like working with Python. It is the first programming language I learned and I still gravitate to it for test automation solutions and elsewhere. Python is versitile, fast, and easy to read. Additionally, my first professional test automation contribututions were made in a similar framework. That said, I'd like to give a big shoutout to Aziz Ettobi ([Linkedin](https://www.linkedin.com/in/azizettobi/) & [GitHub](https://github.com/aettobi)) for introducing me to BDD, and for providing crucial guidance early in my automation career. Aziz was responsible for architecting the framework I contributed to, but he also mentored me into becoming a better tester and programmer.

The README for the repo goes into some detail on BDD, and links to resources, but here's high-level overview of how the components of the structure work together.

We'll start with a 'Feature' ie; [features/api.feature](https://github.com/alxHawley/python-behave/blob/main/features/api.feature), the human-language component of a BDD framework. Here the feature is defined, preconditions can be set (background), and the tests (scenarios) defined. I consider the featue analogous to the 'frontend' while the steps, the 'backend', does the work.

```gherkin
Feature: API CRUD operations  
As an API client I can create, read, update, and delete hotel bookings

Scenario: GET request with booking ID
When a GET request is made with the booking ID
Then the API response returns the correct record details
```

The other main component of a BDD framework (Behave, Cucumber, etc.) is the '_steps.py file', where the actual programming language chosen comes into play. Each statement of the .feature file is paired to a step that executes the test code.

[features/steps/api_Steps.py](https://github.com/alxHawley/python-behave/blob/main/features/steps/api_steps.py) includes the 'When' and 'Then' steps that correlate to the Gherkin syntax in the feature.

'When' step - the action or event that occurs:

```python
@when("a GET request is made with the booking ID")
def step_get_booking(context):
    """Get the booking details using the booking ID"""
    url = f"{BASE_URL}{BOOKING_ENDPOINT}/{context.bookingid}"
    context.response = requests.get(url, headers=headers, timeout=5)
    context.booking = context.response.json()
```

'Then' step - the expected outcome or result:


```bash
@then("the API response returns the correct record details")
def step_booking_details_updated(context):
    """Verify that the booking details are updated successfully"""
    assert context.response.status_code == 200

    # Log the entire JSON response for debugging
    response_json = context.response.json()
    # assert that booking data returned matches data used to update the booking
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