# WillianHillAutomation
Scenario_01

*************************************************
Web Applicatoin Testing with Selenium WebDriver
*************************************************
Before I start with my test automation, I need to prepare a set of test cases for the features 
that are active in the Web application https://sports.williamhill.com/betting/en-gb. 

These can be cases meant for acceptance criteria or part of the functional testing.

Then, for automation, I will need an automation framework that could facilitate test management capabilities like 
creating data-driven tests, setting up test preconditions and postconditions, check the expected and actual output. 
And most importantly, it should provide report generation capability.

Since not all these features are available in Selenium WebDriver, so I will utilize the Python’s unit testing framework and use 
its features along with Selenium Webdriver.

*************************************************

First I would like to explain how Python Unittest Framework works.
Python Unittest library inherits its root from a third-party module known as PyUnit.

Like the JUnit, Python Unittest module splits up its functionality among five key components. All five elements work in tandem to 
support automation testing. 

We have five Components Of Python Unittest Framework

1. Test Loader – It’s a Python class which loads test cases and suites created locally or from an 
external data source like a file. 

2. Test Case – The TestCase class holds the test handlers and provides hooks for preparing each 
handler and for cleaning up after execution.

3. Test Suite – It acts as a container for grouping test cases.
Test Runner – It provides a runnable interface for the execution of tests and delivers the results to the user. 
It can use channels like a GUI.

4. Test Report – This component organizes test results, display pass/fail 
status of the executed test cases.


I can create one or more tests by inheriting the TestCase class 
available in the unittest module. To add a case, I also need 
to provide a corresponding test method (a handler) to the derived class. 
To finalize a test case, I can use assert or any of its 
variations for reporting test status.


********************************************************
Installing Python, Selenium and ChromeDriver on Windows.
********************************************************
1. Installing Python
On Linux Distributions, MAC OS X, and Unix machines; Python is by default installed.

2. If you have pip on your system, you can simply install or upgrade the Python bindings:
pip install -U selenium

3. Drivers
Selenium requires a driver to interface with the chosen browser. Firefox, for example, requires geckodriver, which needs to be 
installed before the below examples can be run. 
Make sure it’s in your PATH, e. g., place it in /usr/bin or /usr/local/bin.
Failure to observe this step will give you an error selenium.common.exceptions.WebDriverException: Message: ‘geckodriver’ executable 
needs to be in PATH.
https://github.com/mozilla/geckodriver/releases

 
In addition to the test handler, we can also add routines like setup() and tearDown() to manage the creation and disposition of any objects 
or conditions that are mandatory for a test.

****************************************************************************************

I using SetUp() Method To Manage Test Pre-Requisites
These are pre-requisites which may include the following test setup preparation tasks.

1. Create an instance of a browser driver.
2. Navigate to a base URL.
3. Load tests data for execution.
4. Open log files for recording inputs, statuses, and errors.

This method takes no arguments and doesn’t return anything. If a script has the setUp() method defined, then the runner 
will call it first before running any of the test handlers.

I using the setup() method to create an instance of Firefox, set up the properties, and navigate to 
the main page of the application before executing the actual test.

After creating a setup() method, I can now write some tests to verify
the application’s functionality. 

In this testing, I will verify the whether the Pop appear.

Similar to the <setup()> method, test methods get implemented in the 
TestCase class. 

While adding these methods, it’s a good practice to prefix their names
with the word test. Having such a name helps Test Runner distinguish between a 
test and other methods. 

Than I to define cleanup stratefy to free post test execution.

Once the test execution finishes, the pre-requisites specified in 
the setup() method have to be cleaned up.

So to achieve this, the base TestCase class provides another method i.e. 
tearDown() which the runner calls after test execution. 
It lets us clean the values initialized at the 
beginning of the test via setup() method.



******************************************************************************

Command-Line Interface
The unittest module can be used from the command line to run tests from modules, classes or even individual test methods:

python -m unittest test_module1 test_module2
python -m unittest test_module.TestClass
python -m unittest test_module.TestClass.test_method
You can pass in a list with any combination of module names, and fully qualified class or method names.

Test modules can be specified by file path as well:
python -m unittest tests/test_something.py

This allows you to use the shell filename completion to specify the test module. The file specified must still be importable as a module. The path is converted to a module name by removing the ‘.py’ and converting path separators into ‘.’. If you want to execute a test file that isn’t importable as a module you should execute the file directly instead.











































