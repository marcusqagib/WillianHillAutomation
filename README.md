# WillianHillAutomation
Scenario_01

*************************************************
Web Application Testing With Selenium WebDriver
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

 
In addition to the test handler, I can also add routines like setup() and tearDown() to manage the creation and disposition of any objects or conditions that are mandatory for a test.

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

In this testing, I will verify the whether the Cookies noitice pop-up.

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


--------------------------------------------------------------------------------------------------------------------
Web Automation Test

Automate following scenarios:

Scenario 1:
    1. Open http://sports.williamhill.com/betting/en-gb
    2. Assert presence of Cookie notice pop-up
    3. Close cookie notice
    4. Assert presence of cdb cookie

Scenario 2:
    1. Assert presence of Join button
    2. Switch language to German
    3. Assert presence of Join button
    4. Assert that Join button label is translated into German
    5. Repeat steps 2-4 for Japanese and Greek
-----------------------------------------------------------------------------------------------------------

import unittest
from selenium import webdriver
import time
import WhBasicOperationsTest

class WhCookie(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        cls.driver = webdriver.Firefox()
        # Deleting all cookies, making sure cookie 'cdb' is not present initially
        cls.driver.delete_all_cookies()
        cls.cookies_name = 'cdb'
        cls.url_es = "https://sports.williamhill.es/betting/es-es"
        cls.url_en = "https://sports.williamhill.com/betting/en-gb"

    def test_cookies_addedES(self):
        ## There is an issue with the English local the cookie popup does not appear, therefore for the purpose of        implementation is used the Spanish locale
        print('There is an issue with the English local, the popup does not appear, therefore for the purpose of implementation is used DE local')
        self.url = self.url_es
        self.cookie_successfully_added(self.cookies_name, self.url)

    def loadPage(self, url):
        self.driver.get(url)
        self.driver.implicitly_wait(15)

    def cookie_successfully_added(self, cookies_name,url):

        ### 1. OPEN "http://sports.williamhill.com/betting/en-gb' ###
        print(' 1. OPEN http://sports.williamhill.com/betting/en-gb  ')
        self.loadPage(url)

        ### 2. ASSERT PRESENCE OF COOKIE NOTICE POP-UP ###
        print(' 2. ASSERT PRESENCE OF COOKIE NOTICE POP-UP  ')
        popup = self.driver.find_element_by_class_name('cookie-disclaimer')
        self.elemExists(popup)

        print('       The cookie notice pop-up is displayed: ', popup.is_displayed())
        button = self.driver.find_element_by_class_name("cookie-disclaimer__button")
        self.elemExists(button)
        self.cookieDoesntExist(self.getCookie(cookies_name))

        ### 3. CLOSE COOKIE NOTICE  ###
        print(' 3. CLOSE COOKIE NOTICE ')
        self.clickElem(button)

        ### 4. ASSERT PRESENCE OF CDB COOKIE  ###
        print(' 4. ASSERT PRESENCE OF CDB COOKIE ')
        cdb_cookie = self.getCookie(cookies_name)
        print('       The new cookie added is: ', cdb_cookie)
        self.cookieExists(cdb_cookie)

    def elemExists(self, elem):
        self.assertTrue(elem.is_displayed())

    def clickElem(self, elem):
        elem.click()

    def getCookie(self, cookie):
        return self.driver.get_cookie(cookie)

    def cookieExists(self, cookie):
        self.assertFalse(cookie is None)

    def cookieDoesntExist(self, cookie):
        self.assertTrue(cookie is None)

    def tearDown(cls):
        file = './screenshots/' + 'WhCookieTest_on_exit' + str(int(round(time.time() * 1000))) +'.png'
        cls.driver.get_screenshot_as_file(file)
        cls.driver.quit()
        print('TEST PASSED')

    if __name__ == '__main__':
        unittest.main()


-------------------------------------------------------


import unittest
from selenium import webdriver
import time

class WhJoinButton(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        cls.driver = webdriver.Firefox()
        url_en = "https://sports.williamhill.com/betting/en-gb"
        cls.driver.get(url_en)
        cls.driver.implicitly_wait(50)
        print('THE USER HAD OPENED http://sports.williamhill.com/betting/en-gb  ')

    def test_join_btn(self):
        print(' 1. HE ASSERTS PRESENCE OF JOIN BUTTON  ')
        self.joinBtnExist()

        language_pairs = [('de','Anmelden'), ('ja', '登録'), ('el', 'Εγγραφή')]
        i = 0
        n = len(language_pairs)
        while (i < n):
            pair = language_pairs[i]
            short_text = str(pair[0])
            text = str(pair[1])
            self.switchLanguageAndAssertTranslation(short_text, text)
            i = i + 1

    def switchLanguageAndAssertTranslation(self, language, label):
        print(' 2. HE SWITCHES THE LANGUAGE TO ', language)
        print(' 3. AND HE SEES JOIN BUTTON')
        print(' 4. HE ASSERTS THE JOIN BUTTON LABEL IS TRANSLATED AS: ',label)
        self.expandLanguageList()
        self.selectLanguage(language)
        self.joinBtnExist()
        self.joinBtnLabelTranslatedTo(label)


    def joinBtnLabelTranslatedTo(self, translation):
        join_new = self.getJoinBtnLabel()
        self.driver.implicitly_wait(10)
        #self.assertTrue(join_new == translation)
        self.assertEqual(join_new,translation)

    def getJoinBtnLabel(self):
        return self.driver.find_element_by_id('joinLink').text

    def selectLanguage(self, language):
        language_selector = self.driver.find_element_by_id(language)
        self.driver.implicitly_wait(10)
        self.elemExists(language_selector)
        self.clickElem(language_selector)
        self.driver.implicitly_wait(50)

    def expandLanguageList(self):
        language_list = self.driver.find_element_by_class_name('js-language-button')
        self.driver.implicitly_wait(10)
        self.elemExists(language_list)
        self.clickElem(language_list)
        self.driver.implicitly_wait(50)

    def joinBtnExist(self):
        join = self.driver.find_element_by_id('joinLink')
        self.assertTrue(join.is_displayed())

    def elemExists(self, elem):
        self.assertTrue(elem.is_displayed())

    def clickElem(self, elem):
        elem.click()

    def tearDown(cls):
        file = './screenshots/' + 'WhCookieTest_on_exit' + str(int(round(time.time() * 1000))) + '.png'
        cls.driver.get_screenshot_as_file(file)
        cls.driver.quit()
        print('TEST PASSED')

    if __name__ == '__main__':
        unittest.main()























