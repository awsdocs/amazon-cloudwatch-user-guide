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

## Packaging your canary files<a name="CloudWatch_Synthetics_Canaries_WritingCanary_Python_package"></a>

If you have more than one \.py file or your script has a dependency, you can bundle them all into a single ZIP file\. If you use the `syn-python-selenium-1.1` runtime, the ZIP file must contain your main canary \.py file within a `python` folder, such as `python/my_canary_filename.py`\. If you use `syn-python-selenium-1.1` or later, you can optionally use a different folder , such as `python/myFolder/my_canary_filename.py`\.

This ZIP file should contain all necessary folders and files, but the other files do not need to be in the `python` folder\.

Be sure to set your canary’s script entry point as `my_canary_filename.functionName` to match the file name and function name of your script’s entry point\. If you are using the `syn-nodejs-selenium-1.0` runtime, then `functionName` must be `handler`\. If you are using `syn-nodejs-selenium-1.1` or later, this handler name restriction doesn't apply, and you can also optionally store the canary in a separate folder such as `python/myFolder/my_canary_filename.py`\. If you store it in a separate folder, specify that path in your script entry point, such as `myFolder/my_canary_filename.functionName`\. 