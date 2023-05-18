# Library functions available for Python canary scripts using Selenium<a name="CloudWatch_Synthetics_Canaries_Library_Python"></a>

This section lists the Selenium library functions available for Python canary scripts\.

**Topics**
+ [Python and Selenium library classes and functions that apply to all canaries](#CloudWatch_Synthetics_Library_allcanaries_Python)
+ [Python and Selenium library classes and functions that apply to UI canaries only](#CloudWatch_Synthetics_Library_Python_UIcanaries)

## Python and Selenium library classes and functions that apply to all canaries<a name="CloudWatch_Synthetics_Library_allcanaries_Python"></a>

The following CloudWatch Synthetics Selenium library functions for Python are useful for all canaries\.

**Topics**
+ [SyntheticsConfiguration class](#CloudWatch_Synthetics_Library_SyntheticsConfiguration_Python)
+ [SyntheticsLogger class](#CloudWatch_Synthetics_Library_SyntheticsLogger_Python)

### SyntheticsConfiguration class<a name="CloudWatch_Synthetics_Library_SyntheticsConfiguration_Python"></a>

You can use the SyntheticsConfiguration class to configure the behavior of Synthetics library functions\. For example, you can use this class to configure the `executeStep()` function to not capture screenshots\.

You can set CloudWatch Synthetics configurations at the global level\.

Function definitions:

#### set\_config\(options\)<a name="CloudWatch_Synthetics_Library_setConfig_Python"></a>

```
from aws_synthetics.common import synthetics_configuration
```

`options` is an object, which is a set of configurable options for your canary\. The following sections explain the possible fields in `options`\.
+ `screenshot_on_step_start` \(boolean\)— Whether to take a screenshot before starting a step\.
+ `screenshot_on_step_success` \(boolean\)— Whether to take a screenshot after completing a successful step\.
+ `screenshot_on_step_failure` \(boolean\)— Whether to take a screenshot after a step fails\.

**with\_screenshot\_on\_step\_start\(screenshot\_on\_step\_start\)**

Accepts a Boolean argument, which indicates whether to take a screenshot before starting a step\.

**with\_screenshot\_on\_step\_success\(screenshot\_on\_step\_success\)**

Accepts a Boolean argument, which indicates whether to take a screenshot after completing a step successfully\.

**with\_screenshot\_on\_step\_failure\(screenshot\_on\_step\_failure\)**

Accepts a Boolean argument, which indicates whether to take a screenshot after a step fails\.

**get\_screenshot\_on\_step\_start\(\)**

Returns whether to take a screenshot before starting a step\.

**get\_screenshot\_on\_step\_success\(\)**

Returns whether to take a screenshot after completing a step successfully\.

**get\_screenshot\_on\_step\_failure\(\)**

Returns whether to take a screenshot after a step fails\.

**disable\_step\_screenshots\(\)**

Disables all screenshot options \(get\_screenshot\_on\_step\_start, get\_screenshot\_on\_step\_success, and get\_screenshot\_on\_step\_failure\)\.

**enable\_step\_screenshots\(\)**

Enables all screenshot options \(get\_screenshot\_on\_step\_start, get\_screenshot\_on\_step\_success, and get\_screenshot\_on\_step\_failure\)\. By default, all these methods are enabled\.

**setConfig\(options\) regarding CloudWatch metrics**

For canaries using `syn-python-selenium-1.1` or later, the **\(options\)** for **setConfig** can include the following Boolean parameters that determine which metrics are published by the canary\. The default for each of these options is `true`\. The options that start with `aggregated` determine whether the metric is emitted without the `CanaryName` dimension\. You can use these metrics to see the aggregated results for all of your canaries\. The other options determine whether the metric is emitted with the `CanaryName` dimension\. You can use these metrics to see results for each individual canary\.

For a list of CloudWatch metrics emitted by canaries, see [CloudWatch metrics published by canaries](CloudWatch_Synthetics_Canaries_metrics.md)\.
+ `failed_canary_metric` \(boolean\)— Whether to emit the `Failed` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `failed_requests_metric` \(boolean\)— Whether to emit the `Failed requests` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `2xx_metric` \(boolean\)— Whether to emit the `2xx` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `4xx_metric` \(boolean\)— Whether to emit the `4xx` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `5xx_metric` \(boolean\)— Whether to emit the `5xx` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `step_duration_metric` \(boolean\)— Whether to emit the `Step duration` metric \(with the `CanaryName` `StepName` dimensions\) for this canary\. The default is `true`\.
+ `step_success_metric` \(boolean\)— Whether to emit the `Step success` metric \(with the `CanaryName` `StepName` dimensions\) for this canary\. The default is `true`\.
+ `aggregated_failed_canary_metric` \(boolean\)— Whether to emit the `Failed` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `aggregated_failed_requests_metric` \(boolean\)— Whether to emit the `Failed Requests` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `aggregated_2xx_metric` \(boolean\)— Whether to emit the `2xx` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `aggregated_4xx_metric` \(boolean\)— Whether to emit the `4xx` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `aggregated_5xx_metric` \(boolean\)— Whether to emit the `5xx` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.

**with\_2xx\_metric\(2xx\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `2xx` metric with the `CanaryName` dimension for this canary\.

**with\_4xx\_metric\(4xx\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `4xx` metric with the `CanaryName` dimension for this canary\.

**with\_5xx\_metric\(5xx\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `5xx` metric with the `CanaryName` dimension for this canary\.

**withAggregated2xxMetric\(aggregated2xxMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `2xx` metric with no dimension for this canary\.

**withAggregated4xxMetric\(aggregated4xxMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `4xx` metric with no dimension for this canary\.

**with\_aggregated\_5xx\_metric\(aggregated\_5xx\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `5xx` metric with no dimension for this canary\.

**with\_aggregated\_failed\_canary\_metric\(aggregated\_failed\_canary\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `Failed` metric with no dimension for this canary\.

**with\_aggregated\_failed\_requests\_metric\(aggregated\_failed\_requests\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `Failed requests` metric with no dimension for this canary\.

**with\_failed\_canary\_metric\(failed\_canary\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `Failed` metric with the `CanaryName` dimension for this canary\.

**with\_failed\_requests\_metric\(failed\_requests\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `Failed requests` metric with the `CanaryName` dimension for this canary\.

**with\_step\_duration\_metric\(step\_duration\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `Duration` metric with the `CanaryName` dimension for this canary\.

**with\_step\_success\_metric\(step\_success\_metric\)**

Accepts a Boolean argument, which specifies whether to emit a `StepSuccess` metric with the `CanaryName` dimension for this canary\.

##### Methods to enable or disable metrics<a name="CloudWatch_Synthetics_Python_setConfig_metrics"></a>

**disable\_aggregated\_request\_metrics\(\)**

Disables the canary from emitting all request metrics that are emitted with no `CanaryName` dimension\.

**disable\_request\_metrics\(\)**

Disables all request metrics, including both per\-canary metrics and metrics aggregated across all canaries\.

**disable\_step\_metrics\(\)**

Disables all step metrics, including both step success metrics and step duration metrics\.

**enable\_aggregated\_request\_metrics\(\)**

Enables the canary to emit all request metrics that are emitted with no `CanaryName` dimension\.

**enable\_request\_metrics\(\)**

Enables all request metrics, including both per\-canary metrics and metrics aggregated across all canaries\.

**enable\_step\_metrics\(\)**

Enables all step metrics, including both step success metrics and step duration metrics\.

**Usage in UI canaries**

First, import the synthetics dependency and fetch the configuration\. Then, set the configuration for each option by calling the setConfig method using one of the following options\.

```
from aws_synthetics.common import synthetics_configuration

synthetics_configuration.set_config(
     {
        "screenshot_on_step_start": False,
        "screenshot_on_step_success": False,
        "screenshot_on_step_failure": True
     }
)

or
```

Or

```
synthetics_configuration.with_screenshot_on_step_start(False).with_screenshot_on_step_success(False).with_screenshot_on_step_failure(True)
```

To disable all screenshots, use the disableStepScreenshots\(\) function as in this example\.

```
synthetics_configuration.disable_step_screenshots()
```

You can enable and disable screenshots at any point in the code\. For example, to disable screenshots only for one step, disable them before running that step and then enable them after the step\.

##### set\_config\(options\) for UI canaries<a name="CloudWatch_Synthetics_Library_Python_UI"></a>

Starting with `syn-python-selenium-1.1`, for UI canaries, `set_config` can include the following Boolean parameters:
+ `continue_on_step_failure` \(boolean\)— Whether to continue with running the canary script after a step fails \(this refers to the **executeStep** function\)\. If any steps fail, the canary run will still be marked as failed\. The default is `false`\.

### SyntheticsLogger class<a name="CloudWatch_Synthetics_Library_SyntheticsLogger_Python"></a>

`synthetics_logger` writes logs out to both the console and to a local log file at the same log level\. This log file is written to both locations only if the log level is at or below the desired logging level of the log function that was called\.

The logging statements in the local log file are prepended with "DEBUG: ", "INFO: ", and so on to match the log level of the function that was called\.

Using `synthetics_logger` is not required to create a log file that is uploaded to your Amazon S3 results location\. You could instead create a different log file in the `/tmp` folder\. Any files created under the `/tmp` folder are uploaded to the results location in the S3 bucket as artifacts\.

To use `synthetics_logger`:

```
from aws_synthetics.common import synthetics_logger
```

****Useful function definitions:

Get log level:

```
log_level = synthetics_logger.get_level()
```

Set log level:

```
synthetics_logger.set_level()
```

Log a message with a specified level\. The level can be `DEBUG`, `INFO`, `WARN`, or `ERROR`, as in the following syntax examples:

```
synthetics_logger.debug(message, *args, **kwargs)
```

```
synthetics_logger.info(message, *args, **kwargs)
```

```
synthetics_logger.log(message, *args, **kwargs)
```

```
synthetics_logger.warn(message, *args, **kwargs)
```

```
synthetics_logger.error(message, *args, **kwargs)
```

For information about debug parameters, see the standard Python documentation at [logging\.debug](https://docs.python.org/3/library/logging.html#logging.debug)

In these logging functions, the `message` is the message format string\. The `args` are the arguments that are merged into `msg` using the string formatting operator\.

There are three keyword arguments in `kwargs`:
+ `exc_info`– If not evaluated as false, adds exception information to the logging message\.
+ `stack_info`– defaults to false\. If true, adds stack information to the logging message, including the actual logging call\.
+ `extra`– The third optional keyword argument, which you can use to pass in a dictionary that is used to populate the `__dict__` of the `LogRecord` created for the logging event with user\-defined attributes\.

Examples:

Log a message with the level `DEBUG`:

```
synthetics_logger.debug('Starting step - login.')
```

Log a message with the level `INFO`\. `logger.log` is a synonym for `logger.info`:

```
synthetics_logger.info('Successfully completed step - login.')
```

or

```
synthetics_logger.log('Successfully completed step - login.')
```

Log a message with the level `WARN`:

```
synthetics_logger.warn('Warning encountered trying to publish %s', 'CloudWatch Metric')
```

Log a message with the level `ERROR`:

```
synthetics_logger.error('Error encountered trying to publish %s', 'CloudWatch Metric')
```

Log an exception:

```
synthetics_logger.exception(message, *args, **kwargs)
```

Logs a message with level `ERROR`\. Exception information is added to the logging message\. You should call this function only from an exception handler\.

For information about exception parameters, see the standard Python documentation at [logging\.exception](https://docs.python.org/3/library/logging.html#logging.exception)

The `message` is the message format string\. The `args` are the arguments, which are merged into `msg` using the string formatting operator\.

There are three keyword arguments in `kwargs`:
+ `exc_info`– If not evaluated as false, adds exception information to the logging message\.
+ `stack_info`– defaults to false\. If true, adds stack information to the logging message, including the actual logging call\.
+ `extra`– The third optional keyword argument, which you can use to pass in a dictionary that is used to populate the `__dict__` of the `LogRecord` created for the logging event with user\-defined attributes\.

Example:

```
synthetics_logger.exception('Error encountered trying to publish %s', 'CloudWatch Metric')
```

## Python and Selenium library classes and functions that apply to UI canaries only<a name="CloudWatch_Synthetics_Library_Python_UIcanaries"></a>

The following CloudWatch Synthetics Selenium library functions for Python are useful only for UI canaries\.

**Topics**
+ [SyntheticsBrowser class](#CloudWatch_Synthetics_Library_Python_SyntheticsBrowser)
+ [SyntheticsWebDriver class](#CloudWatch_Synthetics_Library_Python_SyntheticsWebDriver)

### SyntheticsBrowser class<a name="CloudWatch_Synthetics_Library_Python_SyntheticsBrowser"></a>

When you create a browser instance by calling `synthetics_webdriver.Chrome()`, the returned browser instance is of the type `SyntheticsBrowser`\. The `SyntheticsBrowser` class controls the ChromeDriver, and enables the canary script to drive the browser, allowing the Selenium WebDriver to work with Synthetics\.

In addition to the standard Selenium methods, it also provides the following methods\. 

#### set\_viewport\_size\(width, height\)<a name="CloudWatch_Synthetics_Library_set_viewport_size"></a>

Sets the viewport of the browser\. Example:

```
browser.set_viewport_size(1920, 1080)
```

#### save\_screenshot\(filename, suffix\)<a name="CloudWatch_Synthetics_Library_save_screenshot"></a>

Saves screenshots to the `/tmp` directory\. The screenshots are uploaded from there to the canary artifacts folder in the S3 bucket\.

*filename* is the file name for the screenshot, and *suffix* is an optional string to be used for naming the screenshot\.

Example:

```
browser.save_screenshot('loaded.png', 'page1')
```

### SyntheticsWebDriver class<a name="CloudWatch_Synthetics_Library_Python_SyntheticsWebDriver"></a>

To use this class, use the following in your script:

```
from aws_synthetics.selenium import synthetics_webdriver
```

#### add\_execution\_error\(errorMessage, ex\);<a name="CloudWatch_Synthetics_Library_Python_addExecutionError"></a>

`errorMessage` describes the error and `ex` is the exception that is encountered

You can use `add_execution_error` to set execution errors for your canary\. It fails the canary without interrupting the script execution\. It also doesn't impact your `successPercent` metrics\.

You should track errors as execution errors only if they are not important to indicate the success or failure of your canary script\.

An example of the use of `add_execution_error` is the following\. You are monitoring the availability of your endpoint and taking screenshots after the page has loaded\. Because the failure of taking a screenshot doesn't determine availability of the endpoint, you can catch any errors encountered while taking screenshots and add them as execution errors\. Your availability metrics will still indicate that the endpoint is up and running, but your canary status will be marked as failed\. The following sample code block catches such an error and adds it as an execution error\.

```
try:
    browser.save_screenshot("loaded.png")  
except Exception as ex:
   self.add_execution_error("Unable to take screenshot", ex)
```

#### add\_user\_agent\(user\_agent\_str\)<a name="CloudWatch_Synthetics_Library_add_user_agent"></a>

Appends the value of `user_agent_str` to the browser's user agent header\. You must assign `user_agent_str` before creating the browser instance\.

Example:

```
synthetics_webdriver.add_user_agent('MyApp-1.0')
```

#### execute\_step\(step\_name, function\_to\_execute\)<a name="CloudWatch_Synthetics_Library_Python_execute_step"></a>

Processes one function\. It also does the following:
+ Logs that the step started\.
+ Takes a screenshot named `<stepName>-starting`\.
+ Starts a timer\.
+ Executes the provided function\.
+ If the function returns normally, it counts as passing\. If the function throws, it counts as failing\.
+ Ends the timer\.
+ Logs whether the step passed or failed
+ Takes a screenshot named `<stepName>-succeeded` or `<stepName>-failed`\.
+ Emits the `stepName` `SuccessPercent` metric, 100 for pass or 0 for failure\.
+ Emits the `stepName` `Duration` metric, with a value based on the step start and end times\.
+ Finally, returns what the `functionToExecute` returned or re\-throws what `functionToExecute` threw\.

Example:

```
def custom_actions():
        #verify contains
        browser.find_element_by_xpath("//*[@id=\"id_1\"][contains(text(),'login')]")
        #click a button
        browser.find_element_by_xpath('//*[@id="submit"]/a').click()

    await synthetics_webdriver.execute_step("verify_click", custom_actions)
```

#### Chrome\(\)<a name="CloudWatch_Synthetics_Library_Python_Chrome"></a>

Launches an instance of the Chromium browser and returns the created instance of the browser\.

Example:

```
browser = synthetics_webdriver.Chrome()
browser.get("https://example.com/)
```

To launch a browser in incognito mode, use the following:

```
add_argument('——incognito')
```

To add proxy settings, use the following:

```
add_argument('--proxy-server=%s' % PROXY)
```

Example:

```
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument("——incognito")
browser = syn_webdriver.Chrome(chrome_options=chrome_options)
```