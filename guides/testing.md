# Testing

**Scenario: Building a Calculator Application**

Let's assume we're building a simple calculator application that provides add, subtract, multiply, and divide functions.

<mark style="background-color:orange;">**Unit Testing**</mark>

* **What:** Tests individual units (functions, classes) in complete isolation from other parts of the code.
* **Focus:** Ensuring that each part of your code works as intended in isolation.
* **Python Example (using the `unittest` framework)**

Python

```
import unittest

def add(x, y):
    return x + y

class CalculatorTests(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(2, 3), 5)
        self.assertEqual(add(-1, 1), 0)

if __name__ == '__main__':
    unittest.main()
```

<mark style="background-color:orange;">**Integration Testing**</mark>

* **What:** Tests how multiple units of code work together.
* **Focus:** Checking that different components of your application interact and communicate correctly.
* **Python Example (Assume existence of `add`, `subtract`, etc., functions)**

Python

```
import unittest

class CalculatorIntegrationTests(unittest.TestCase):
    def test_multiple_operations(self):
        result = add(5, 3)
        result = subtract(result, 2)
        self.assertEqual(result, 6) 

if __name__ == '__main__':
    unittest.main()
```

<mark style="background-color:orange;">**End-to-End (E2E) Testing**</mark>

* **What:** Tests the entire application from the user's perspective.
* **Focus:** Verifying complete user flows, including the UI (often involving a testing framework like Selenium).
* **Python Example (using Selenium for UI interaction)**

Python

```
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()

# Simulate user interactions
driver.get("http://my-calculator-app.com/")
driver.find_element(By.ID, "num1").send_keys("10")
driver.find_element(By.ID, "num2").send_keys("5")
driver.find_element(By.ID, "divide_button").click()
result = driver.find_element(By.ID, "result").text

driver.quit() 
self.assertEqual(result, "2") 
```

**Key Points**

* **Testing Pyramid:** Ideally, you should have a lot of unit tests providing a strong foundation, fewer integration tests, and even fewer E2E tests (due to complexity and execution time).
* **Mocking:** Integration tests often involve mocking external parts of the system to keep tests focused on specific interactions.

Let me know if you'd like to explore other scenarios or have questions about testing tools!
