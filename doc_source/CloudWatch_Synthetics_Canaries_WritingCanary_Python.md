# Writing a Python canary script<a name="CloudWatch_Synthetics_Canaries_WritingCanary_Python"></a>

This script passes as a successful run, and returns a string\. To see what a failing canary looks like, change fail = False to fail = True

```
def basic_custom_script():
    # Insert your code here
    # Perform multi-step pass/fail check
    # Log decisions made and results to /tmp
    # Be sure to wait for all your code paths to complete 
    # before returning control back to Synthetics.
    # In that way, your canary will not finish and report success
    # before your code has finished executing
    fail = False
    if fail:
        raise Exception("Failed basicCanary check.")
    return "Successfully completed basicCanary checks."
def handler(event, context):
    return basic_custom_script()
```

If you have more than one \.py file or your script has a dependency, you can bundle them all into a single ZIP file\. the ZIP file must contain your main canary \.py file within a `python` folder, such as `python/my_canary_filename.py`\. This ZIP file should contain all necessary folders and files, but the other files do not need to be in the `python` folder\.

Be sure to set your canary’s script entry point as `my_canary_filename.handler` to match the file name of your script’s entry point\.

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