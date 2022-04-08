# BestAutomationPractices - Guide for best automation frame work as per SELENIUM 4
https://www.lambdatest.com/blog/selenium-best-practices-for-web-testing/

****PERFORMANCE WISE TODOs****  
****1.)Avoid Blocking using sleep calls****- We should abstain from using Thread.sleep within the test cases. Loading of web elements on any website depends on many factors like load on the server, netwrok speed, device (machine) capabilities, access location, browser (some time performing tests on out-dated browsers) and any more. So keeping this in view we can never sure how much exact time will the element will be going to take. Use of sleep blocking calls blocks the test thread for pre-specified time interval. For example we are putting Thread.sleep(5000) which 5 seconds however but when we are running the test framework using some other environments it is taking 10 seconds and ultimately our test will fail with No such Element exception. If we take more time in the Thread.sleep(9000) then it can increase the time of test pack execution i.e. if the test getting executed within 6 seconds it will still keep the thread on hold for another 3 seconds. Lets say if we have 1000 test cases then extra time will be e.g. 1000*3 seconds .
Alternatives to deal with **NO SUCH ELEMENT** Exception-- Selenium provides inbuilt wait methods and we should alway prefer to use these as below :  
**a.) Implicit Wait -** If element takes less than 5 seconds to load selenium moves on to next line of code and do not hold the process for all 5 seoconds.
SYNTAX ->    driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(5));  
**b.) Explicit Wait -** Explicit wait can be provided with respect to WebElement. So it will hold the execution untill the particular element loads upto a predefined time. If elements loads earlier then the execution moves to next line of code.  
SYNTAX ->     WebDriverWait wait = new WebDriverWait(driver,Duration.ofSeconds(10));  
              wait.until(ExpectedConditions.visibilityOfElementLocated(By.cssSelector(".classlocator")));               
**c.) Fluent Waits -** Fluent wait instance allow to wait for some specific period of time along with freqeuncy duration in which it will check the condition and if it matches it moves on to next line of code without waiting for full pre-specified time.   
SYNTAX ->      Wait<WebDriver> wait = new FluentWait<WebDriver>(driver)  
                .withTimeout(Duration.ofSeconds(30))  // Predfined total time limit it gonna be wait  
                .pollingEvery(Duration.ofSeconds(5))  // Frequency, after every 5 secs it will check the condition if element found it can stop at 5 or 10 or 15 secs  
                .ignoring(NoSuchElementException.class);  // While waiting ignore NoSuchElementException.class  
LINK to know more about WebDriver waits - https://www.selenium.dev/documentation/webdriver/waits/     

**2.) Keep Browser's default zoom level to 100% -** -> Browser zoom level should always 100% so that the native mouse events can be set to correct coordinates.
  Sometime you see on browser process is working fine but not on others, specifically happens on outdated browsers like internet explorer.    
  Whenever using internet browser always make sure to set zoom level to 100% . Secondly make sure the **protected mode setting in IE for each zone must be same otherwise No such window exception can appear.**  

**3.) Maximize the Browser Window** -> Always maximize the browser window immediately after loading the URL. Not maximizing can affect the screenshots generation.    

**4.) IMPORTANT ONE - CHOOSE MOST SUITED WEB BROWSER** -> We see many a times whenever there are changes in products, test cases starts failing because of changes in Webelement attributes of a DOM. So, we should always have a strategy for selecting the best locators.We can keep below factors in mind while selecting the locators for test automation -  
  **Factor a.)** LinkText or PartialLinkText should preferred when there is a dynamic situation. But should be avoided if application is to be used internationally (becoz with language change LinkText will change). So for internation app or localization testing, we should use **partial href**, so that even if the language changes the href should point to the right URL.  
  **Factor b.)** In noraml UIs general locator order is **ID >Name >CSSSelector >Xpath** . **Why Xpath is on last although we use it frequently ?** because XPath engines vary from browser to browser, so working on one browser do not gurantee its working on another browser. IE does not even have the native Xpath engine and uses JavaScript Xpath Query engine and it is slower than native Xpath engine, which can create problems.  
  **Factor c.)** If project in starting stage Dev team can be instructed to use static IDs or meaning classes for differentiating elements.  
  
**5.) Create Browser compatibility matrix** - If the system need to be tested on multiple browsers and device combination always create a matrix as it will give you preferences which combinations are most important to run on. **Browser stack and LambdaTest can be used for this purpose** 
  
**6.) Do Not Use a Single Driver Implementation** - WebDrivers in Selenium are not interchangeable. The situation of executing automated cross browser tests on a local machine is entirely different from being executed on a continuous build server. In that environment, it will be wrong to assume that the next test will use Firefox WebDriver (or Chrome WebDriver or any other WebDriver).  
When integration tests are executed are in a continuous build environment, the test will only receive a RemoteDriver (i.e., WebDriver for any target browser). Amongst all Selenium best practices, it is recommended to use Parameter notes to manage different browser types and get the code ready for simultaneous execution (or parallel testing). Small frameworks can be created in Selenium using LabelledParameterized (@Parameters in TestNG and @RunWith in JUnit).  
This practice will be useful in ensuring that the implementation is flexible enough to work with different browser types. 
**7.) Use of Hard and Soft (verify) Assertions in TestNG** - There are some test cases where we just want to check if a test stpes working or not and don't want to halt the execution there we can use either **CHECKPOINT CONCEPT OR SOFT ASSERTION (Verify)** . But there are some test steps and if those don't work we can not move forward and there we have to use **HARD ASSERTIONS** e.g if we are not able to sign in then we can not proceed further and continue testing so we need to stop execution right there using hard assertion, may be if a Web URL is not working we can try once more and then have to stop the complete execution pack.  

  

****MAINTENABILITY, RESUABILITY AND CODE WISE TODOS****   
**8.) Always Name the Test methods, classes and suites appropriately -** It has two reasons:
  a.) Your colleagues may need to work in enahancing the framework that you have written
  b.) Whenever a test case fails it should be easy to identify which functionality is getting broken by just looking at the names of test cases.
  
**9.) Always follow OOPS principles in your design along with proper documentation (like Understandable comments on methods)** This makes the code modular and open for enahncements.  
**10.) Alway follow design patterns and model like PAGE OBJECT MODEL** This enhances code reusability,code size reduction, maintenance, minimized code changes due to UI updates and overall good visualization of pages under test.  
**11.) Implement good Logging and reporting**  Console logs at appropriate places with appropriate infomation can be huge saviours in case of any test failures. Adding unnecessary logs in test implementation can cause confusion and delay in execution. Alway add logs with level error. **Log4j -> info/error**  
  Good reporting can also help you to navigate through overall pass/fail test percentage. **ExtentReport / Allure can be used**
**12.) Follow a Uniform Directory Structure** Test Methods should be separate from other things like Page Object and method classes, Utilities, base classes and other generic functions.   
  
**13.) Avoid dependency of Test cases among each other** - Test cases should be autonomous and should not depend on any other test for its working.
  
**14.) Use of Checkpoint class** - Where we have to make multiple test steps validations in single test, checkpoint class is really handy. This takes a list of PASS/FAIL from test steps and then at final steps checks if any of test step is FAIL or PASS. This helps in not halting the execution on one test step failure and let the system check for remaining test steps. For example if we have to compare if a page object say work package have some 6 text values on it or not. even if it don't have the first value test will check for remaining 5 values.
  
**15.) Avoid code duplication and always wrap Selenium API calls** - Any code which is to be used multiple time in your framework should always be created as separate APIs or utilities or generic functions. This enhance usability, maintenance and code size. Wrapping selenium API methods also give us option to add better logging as per project. We can also overload the methods with more and more information.
  


  

  
  
  
