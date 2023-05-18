# Writing a canary script<a name="CloudWatch_Synthetics_Canaries_WritingCanary"></a>

The following sections explain how to write a canary script and how to integrate a canary with other AWS Services\.

**Topics**
+ [Writing a Node\.js canary script](CloudWatch_Synthetics_Canaries_WritingCanary_Nodejs.md)
+ [Writing a Python canary script](CloudWatch_Synthetics_Canaries_WritingCanary_Python.md)
+ [Changing an existing Selenium script to use a Synthetics canary](#CloudWatch_Synthetics_Canaries_WritingCanary_Python_Selenium)

## Changing an existing Selenium script to use a Synthetics canary<a name="CloudWatch_Synthetics_Canaries_WritingCanary_Python_Selenium"></a>

You can quickly modify an existing script for Python and Selenium to be used as a canary\. For more information about Selenium, see [www\.selenium\.dev/](https://www.selenium.dev/)\.

For this example, we'll start with the following Selenium script:

```
from selenium import webdriver

def basic_selenium_script():
    browser = webdriver.Chrome()
    browser.get('https://example.com')
    browser.save_screenshot('loaded.png')

basic_selenium_script()
```

The conversion steps are as follows\.

**To convert a Selenium script to be used as a canary**

1. Change the `import` statement to use Selenium from the `aws_synthetics` module:

   ```
   from aws_synthetics.selenium import synthetics_webdriver as webdriver
   ```

   The Selenium module from `aws_synthetics` ensures that the canary can emit metrics and logs, generate a HAR file, and work with other CloudWatch Synthetics features\.

1. Create a handler function and call your Selenium method\. The handler is the entry point function for the script\.

   If you are using `syn-python-selenium-1.0`, the handler function must be named `handler`\. If you are using `syn-python-selenium-1.1` or later, the function can have any name, but it must be the same name that is used in the script\. Also, if you are using `syn-python-selenium-1.1` or later, you can store your scripts under any folder and specify that folder as part of the handler name\.

   ```
   def handler(event, context):
       basic_selenium_script()
   ```

The script is now updated to be a CloudWatch Synthetics canary\. Here is the updated script:

```
from aws_synthetics.selenium import synthetics_webdriver as webdriver

def basic_selenium_script():
    browser = webdriver.Chrome()
    browser.get('https://example.com')
    browser.save_screenshot('loaded.png')

def handler(event, context):
    basic_selenium_script()
```