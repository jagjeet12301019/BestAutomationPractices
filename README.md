# BestAutomationPractices - Guide for best automation frame work as per SELENIUM 4
https://www.lambdatest.com/blog/selenium-best-practices-for-web-testing/

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

  
**2.) Always Name the Test methods, classes and suites appropriately -** It has two reasons:
  a.) Your colleagues may need to work in enahancing the framework that you have written
  b.) Whenever a test case fails it should be easy to identify which functionality is getting broken by just looking at the names of test cases.
  

**3.) Keep Browser's default zoom level to 100% -** -> Browser zoom level should always 100% so that the native mouse events can be set to correct coordinates.
  Sometime you see on browser process is working fine but not on others, specifically happens on outdated browsers like internet explorer.    
  Whenever using internet browser always make sure to set zoom level to 100% . Secondly make sure the **protected mode setting in IE for each zone must be same otherwise No such window exception can appear.**
  

**4.) Maximize the Browser Window** -> Always maximize the browser window immediately after loading the URL. Not maximizing can affect the screenshots generation.
  

**5.) IMPORTANT ONE - Choose the most suited Web Locator** -> We see many a times whenever there are changes in products, test cases starts failing because of changes in Webelement attributes of a DOM. So, we should always have a strategy for selecting the best locators.We can keep below factors in mind while selecting the locators for test automation -  
  **Factor a.)** LinkText or PartialLinkText should preferred when there is a dynamic situation. But should be avoided if application is to be used internationally (becoz with language change LinkText will change). So for internation app or localization testing, we should use **partial href**, so that even if the language changes the href should point to the right URL.  
  **Factor b.)** In noraml UIs general locator order is ID >Name >CSSSelector >Xpath . **Why Xpath is on last although we use it frequently ?** because XPath engines vary from browser to browser, so working on one browser do not gurantee its working on another browser. IE does not even have the native Xpath engine and uses JavaScript Xpath Query engine and it is slower than native Xpath engine, which can create problems.  
  **Factor c.)** If project in starting stage Dev team can be instructed to use static IDs or meaning classes for differentiating elements.  
  
  

  
  
  
