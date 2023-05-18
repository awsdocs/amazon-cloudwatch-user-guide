# Runtime versions using Python and Selenium Webdriver<a name="CloudWatch_Synthetics_Library_python_selenium"></a>

The following sections contain information about the CloudWatch Synthetics runtime versions for Python and Selenium Webdriver\. Selenium is an open\-source browser automation tool\. For more information about Selenium, see [www\.selenium\.dev/](https://www.selenium.dev)

The naming convention for these runtime versions is `syn-language-framework-majorversion.minorversion`\.

## syn\-python\-selenium\-1\.3<a name="CloudWatch_Synthetics_runtimeversion-syn-python-selenium-1.3"></a>

**Major dependencies**:
+ Python 3\.8
+ Selenium 3\.141\.0
+ Chromium version 92\.0\.4512\.0

**New features in syn\-python\-selenium\-1\.3**:
+ **More precise timestamps**— The start time and stop time of canary runs are now precise to the millisecond\.

## syn\-python\-selenium\-1\.2<a name="CloudWatch_Synthetics_runtimeversion-syn-python-selenium-1.2"></a>

**Major dependencies**:
+ Python 3\.8
+ Selenium 3\.141\.0
+ Chromium version 92\.0\.4512\.0
+ **Updated dependencies**— The only new features in this runtime are the updated dependencies\.

## syn\-python\-selenium\-1\.1<a name="CloudWatch_Synthetics_runtimeversion-syn-python-selenium-1.1"></a>

**Major dependencies**:
+ Python 3\.8
+ Selenium 3\.141\.0
+ Chromium version 83\.0\.4103\.0

**Features**:
+ **Custom handler function**— You can now use a custom handler function for your canary scripts\. Previous runtimes required the script entry point to include `.handler`\. 

  You can also put canary scripts in any folder and pass the folder name as part of the handler\. For example, `MyFolder/MyScriptFile.functionname` can be used as an entry point\.
+ **Configuration options for adding metrics and step failure configurations**— These options were already available in runtimes for Node\.js canaries\. For more information, see [SyntheticsConfiguration class](CloudWatch_Synthetics_Canaries_Library_Python.md#CloudWatch_Synthetics_Library_SyntheticsConfiguration_Python)\.
+ **Custom arguments in Chrome **— You can now open a browser in incognito mode or pass in proxy server configuration\. For more information, see [ Chrome\(\)](CloudWatch_Synthetics_Canaries_Library_Python.md#CloudWatch_Synthetics_Library_Python_Chrome)\.
+ **Cross\-Region artifact buckets**— A canary can store its artifacts in an Amazon S3 bucket in a different Region\.
+ **Bug fixes, including a fix for the `index.py` issue**— With previous runtimes, a canary file named `index.py` caused exceptions because it conflicted with the name of the library file\. This issue is now fixed\.

## syn\-python\-selenium\-1\.0<a name="CloudWatch_Synthetics_runtimeversion-syn-python-selenium-1.0"></a>

**Major dependencies**:
+ Python 3\.8
+ Selenium 3\.141\.0
+ Chromium version 83\.0\.4103\.0

**Features**:
+ **Selenium support**— You can write canary scripts using the Selenium test framework\. You can bring your Selenium scripts from elsewhere into CloudWatch Synthetics with minimal changes, and they will work with AWS services\.