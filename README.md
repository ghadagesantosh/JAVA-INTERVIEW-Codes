=================================================================== *ValidateDropdownOption* =================================================================

public void verifyDropdownText(TestContext testContext, String txt) throws Throwable {
        boolean optionMatched = false;
        String optionActual = "";
        String expectedOptValue = "";

        ScrollToWebElement(testContext, btn_dropDownRelationship);
        waitForElementToBeClickable(testContext, btn_dropDownRelationship);
        click(testContext, btn_dropDownRelationship, "Relationship Dropdown");

        String[] expectedOpts = txt.split(",");
        for (String expectedOpt : expectedOpts) {
            expectedOptValue = expectedOpt;

            List<WebElement> options = testContext.getWebDriverManager().getDriver().findElements(By.xpath("//*[@role=\"option\"]"));
            for (WebElement Option : options) {
                optionActual = Option.getText().trim();
                if (optionActual.equalsIgnoreCase(expectedOpt)) {
                    validate(testContext, expectedOptValue, optionActual, "Validating Dropdown Options", true);
                    optionMatched = true;
                }
            }
        }
        if (!optionMatched) {

            Assert.fail("Validate Dropdown Option" + "  |   Actual: " + optionActual + " |  Expected: " + expectedOptValue);
            logToHtmlReport(testContext, " - " + "Validate Dropdown Option" + "  |   Actual: " + optionActual + " |  Expected: " + expectedOptValue);
        }
        Thread.sleep(2000);
        click(testContext, driver.findElement(By.tagName("body")), "");
    }

=================================================================== *Date Pickers 1* =================================================================

 public void selectDateFromCalendar(TestContext testContext, String date) throws Throwable {
        String[] fromParts = date.split("/");
        String day = fromParts[0];
        String month = fromParts[1];
        String year = fromParts[2];
        String[] months = {"JAN", "FEB", "MAR", "APR", "MAY", "JUN", "JUL", "AUG", "SEP", "OCT", "NOV", "DEC"};
        int monthIndex = Integer.parseInt(month) - 1;
        month = months[monthIndex];

        click(testContext, btn_Calendar, "Calender");
        Thread.sleep(1000);
        click(testContext, btn_year, "Year field of Calender");
        Thread.sleep(1000);

        int targetYear = Integer.parseInt(year);
        int currentYear = LocalDate.now().getYear();

        boolean isFutureDate = currentYear < targetYear;
        boolean yearFound = false;

        while (true) {
            Thread.sleep(1000);
            List<WebElement> yearList = testContext.getWebDriverManager().getDriver().findElements(By.xpath("//td//button[contains(@class,'mat-calendar-body-cell')]"));

            int firstYear = Integer.parseInt(yearList.get(0).getText());
            int lastYear = Integer.parseInt(yearList.get(yearList.size() - 1).getText());

            if (targetYear >= firstYear && targetYear <= lastYear) {
                testContext.getWebDriverManager().getDriver().findElement(By.xpath("//*[contains(text(),' " + targetYear + " ')]")).click();
                break;
            } else if (targetYear > lastYear) {
                Thread.sleep(1000);
                testContext.getWebDriverManager().getDriver().findElement(By.xpath("//*[@aria-label=\"Next 24 years\"]")).click();

            } else if (targetYear < firstYear) {
                Thread.sleep(1000);
                testContext.getWebDriverManager().getDriver().findElement(By.xpath("//*[@aria-label=\"Previous 24 years\"]")).click();
            }

        }
        Thread.sleep(1000);
        WebElement months1 = testContext.getWebDriverManager().getDriver().findElement(By.xpath("//*[contains(text(),'" + month + "')]"));
        click(testContext, months1, "Month from list");
        takeScreenshot(testContext);

        Thread.sleep(1000);
        WebElement day1 = testContext.getWebDriverManager().getDriver().findElement(By.xpath("//*[contains(text(),' " + day + " ')]"));
        click(testContext, day1, "Day from list");
        takeScreenshot(testContext);

    }

=================================================================== *Date Pickers 2* =================================================================

String[] fromParts = date.split("/");
        String dayFrom = fromParts[0];
        String monthFrom = fromParts[1];
        String yearFrom = fromParts[2];
        String[] months = {"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"};
        int monthIndex = Integer.parseInt(monthFrom) - 1;
        monthFrom = months[monthIndex];


 waitForElementToBeClickable(testContext, field_NomineeDOB);
        click(testContext, field_NomineeDOB, text);
        Thread.sleep(2000);
        click(testContext, btn_yeardownArrowButton, "Year field of Calender");
        Thread.sleep(1000);
        int GivenYear = Integer.parseInt(year);
        int currentYear = LocalDate.now().getYear();
        System.out.println(currentYear);

        boolean isFutureDate = currentYear < GivenYear;
        boolean yearFound = false;

        while (true) {
            Thread.sleep(2000);
            List<WebElement> yearList = testContext.getWebDriverManager().getDriver().findElements(By.xpath("//android.view.View[@content-desc='" + year + "']"));
            if (!yearList.isEmpty() && yearList.get(0).getDomAttribute("content-desc").equals(year)) {
                yearList.get(0).click();
                break;
            } else {
                Thread.sleep(1000);
                if (isFutureDate) {
                    testContext.getWebDriverManager().getDriver().findElement(By.xpath("//android.widget.ScrollView/android.view.View/android.widget.ImageView[5]")).click();
                } else {
                    testContext.getWebDriverManager().getDriver().findElement(By.xpath("//android.widget.ScrollView/android.view.View/android.widget.ImageView[4]")).click();
                }
            }
        }

        waitForElementToBeVisibleUsingXpath(testContext, "//*[@content-desc='" + monthTxt + "']");
        WebElement month = testContext.getWebDriverManager().getDriver().findElement(By.xpath("//*[@content-desc='" + monthTxt + "']"));
        waitForElementToBeClickable(testContext,month);
        click(testContext, month, "Month from list");
        takeScreenshot(testContext);

        waitForElementToBeVisibleUsingXpath(testContext, "//*[@content-desc='" + dayTxt + "']");
        WebElement day = testContext.getWebDriverManager().getDriver().findElement(By.xpath("(//*[@content-desc='" + dayTxt + "'])[1]"));
        waitForElementToBeClickable(testContext,day);
        click(testContext, day, "Day from list");
        takeScreenshot(testContext);

    }

=================================================================== *Get Data From Table* =================================================================
    public void printTable(TestContext testContext, int num, String tblname) throws InterruptedException {
        Thread.sleep(5000);
        WebElement table = testContext.getWebDriverManager().getDriver().findElement(By.xpath("(//table)[" + num + "]"));
        List<WebElement> headers = table.findElements(By.xpath(".//th"));
        StringBuilder tableData = new StringBuilder();

        System.out.println(tblname);
        for (WebElement header : headers) {

            tableData.append(header.getText().trim()).append(" | ");
        }
        tableData.append("\n");

        List<WebElement> rows = table.findElements(By.xpath(".//tbody//tr"));
        for (WebElement row : rows) {
            List<WebElement> cols = row.findElements(By.xpath("./td"));
            for (WebElement col : cols) {
                tableData.append(col.getText().trim()).append(" | ");
            }
            tableData.append("\n");
        }
        String finalTableData = tableData.toString();
        logToHtmlReport(testContext, tblname + " :-> ");
        logToHtmlReport(testContext, "<prev>" + finalTableData + "</prev>");
        System.out.println(finalTableData);
    }
}


=========================================================================================================================================================================================

Handling popups
 
public void verify_YFEI_UPI_LandingPage(TestContext testContext) throws Throwable {
        Thread.sleep(5000);
        if (biometricPOPUP_CrossImage.isDisplayed()) {
            click(testContext, biometricPOPUP_CrossImage, "Cross image");
        }
        if (txt_Do_It_Later.isDisplayed()) {
            for (int i = 0; i <= 2; i++) {
                Thread.sleep(2000);
                click(testContext, txt_Do_It_Later, "Do it later");
            }
        }
        isDisplayed(testContext, lbl_hello);
        logToHtmlReport(testContext, "User is successfully navigated to UPI Landing Page");
    }

-------------------------------------------------------------------------------------------------------------------------------------------------------
Verifying Text Formats

case "UPIIDValue":
                waitForElementToBeVisible(driver, txt_UPIID);
                isDisplayed(testContext, txt_UPIID);
                String UPIid = txt_UPIID.getAttribute("content-desc");
//                System.out.println(UPIid);
                boolean isValidUPI = UPIid.matches("^\\d{10}@[a-zA-Z]+$");

                if (isValidUPI) {
                    logToHtmlReport(testContext, "Valid UPI ID format");
                } else {
                    throw new RuntimeException("Invalid UPI ID format");
                }
                takeScreenshot(testContext);

                break;

            case "DateFormat":
                waitForElementToBeVisible(driver, txt_DateTime);
                isDisplayed(testContext, txt_DateTime);
                String dateText = txt_DateTime.getAttribute("content-desc");
                boolean isValidDate = dateText.matches("^\\d{2}/\\d{2}/\\d{4} at \\d{1,2}:\\d{2} [AP]M$");

                if (isValidDate) {
                    logToHtmlReport(testContext, "Valid Date & Time format");
                } else {
                    throw new RuntimeException("Invalid UPI ID format");
//                    logToHtmlReport(testContext, "Invalid Date & Time format");
                }
                takeScreenshot(testContext);

                break;

            case "AmountinRS":
                waitForElementToBeVisible(driver, txt_PayAmount);
                isDisplayed(testContext, txt_PayAmount);

                String AmountText = txt_PayAmount.getAttribute("content-desc");
                boolean isValidAmount = AmountText.matches("^₹?\\d+(\\.\\d{2})?$");

                if (isValidAmount) {
                    logToHtmlReport(testContext, "Valid Amount format");
                } else {
                    throw new RuntimeException("Invalid UPI ID format");
//                    logToHtmlReport(testContext, "Invalid Amount format");
                }
                takeScreenshot(testContext);

                break;
=========================================================================================================================================================

Handling popups

    public void clickonDoit(TestContext testContext) throws Throwable {
        Thread.sleep(5000);
        int maxRetries=3;
        int attempts=0;

        while (btn_Doit.isDisplayed() && attempts < maxRetries){
            Thread.sleep(3000);
            click(testContext, btn_Doit, "Do it later");


            attempts++;
        }
        takeScreenshot(testContext);
        if (attempts == maxRetries){
            logToHtmlReport(testContext,"Do it later button appeared too many times");
        }
}}
=========================================================================================================================================================
Validate list of items
public void verifyContactList(TestContext testContext) {
        for (WebElement list : contanct_List) {
            isDisplayed(testContext, list);
        }
    }
=========================================================================================================================================================
Applicable for Single Webelement
 public void CheckListOfBlockedUPI(TestContext testContext){

        waitForElementToBeVisible(driver,list_BlockedContact);
     if(list_BlockedContact.isDisplayed())
     {
         logToHtmlReport(testContext,list_BlockedContact.getSize()+"="+ "Blocked Contacts");
     }
     else {
         logToHtmlReport(testContext,"No Blocked Contacts");
     }
    }
=========================================================================================================================================================
List <Webelement>
 public void CheckListOfBlockedUPI(TestContext testContext,String text1 ,String text2) throws InterruptedException {

        Thread.sleep(4000);
        try {
            if (!list_BlockedContact.isEmpty()) {
                int totalBlocked = list_BlockedContact.size();
//                String num=String.valueOf(totalBlocked);
                logToHtmlReport(testContext, "TotalBlockedContacts: " + totalBlocked);
                Thread.sleep(2000);
                isDisplayedWithTextValidation(testContext, txt_YouCannot, textAttributeBasedOnDriver, text1);
                takeScreenshot(testContext);
            }
        } catch(NoSuchElementException e)
      {
         logToHtmlReport(testContext,"No Blocked Contacts");
         Thread.sleep(2000);
         isDisplayedWithTextValidation(testContext,txt_NoBlockedContacts,textAttributeBasedOnDriver,text2);

=========================================================================================================================================================
App- Click on perticular place in app
TouchActionEvents.tap(driver,884,492);
         takeScreenshot(testContext);
     }

    }

=========================================================================================================================================================

public void clickonDoit(TestContext testContext) throws Throwable {

        Thread.sleep(5000);
        int maxRetries = 3;
        int attempts = 0;

        while (attempts < maxRetries) {
            try {
                if (btn_Doit.isDisplayed()) {
                    click(testContext, btn_Doit, "Do it later");
                    attempts++;
                    Thread.sleep(3000);
                } else {
                    break;

                }

            } catch (Exception e) {

                break;
            }
        }
        if (attempts == maxRetries) {
            logToHtmlReport(testContext, "Do it later button appeared too many times");
        }
    }
}
=========================================================================================================================================================
Scroll element
TouchActionEvents.swipe(driver, 309, 1230, 309, 1100);

Swipe - To sroll element --------- swipe(driver, 564, 2098, 546, 856);
tap - to click on element
=========================================================================================================================================================
check Availability and display details using try catch mechanism

 public void verifyTextOnNotification(TestContext testContext, String text) throws InterruptedException {
        switch (text) {
            case "Notifications":
//             swipe(driver,344,1000,350,1350);
                isDisplayedWithTextValidation(testContext,txt_notification,textAttributeBasedOnDriver,text);
                takeScreenshot(testContext);
                break;

            case "Payments":
                try {
                    if (!txt_payments.isDisplayed()) {
                        logToHtmlReport(testContext, "Text Payments not available due to there are no new Notification");
                    } else {
                        isDisplayedWithTextValidation(testContext, txt_payments, textAttributeBasedOnDriver, text);
                        takeScreenshot(testContext);
                    }

                }catch(Exception e){
                    logToHtmlReport(testContext, "Text Payments not available due to there are no new Notification");
                }
                break;

            case "Request Details":
                try {
                    Thread.sleep(4000);
                    int list = txt_NotificationDetails.size();   //LinkedHashSet
                    if (list == 0) {
                        logToHtmlReport(testContext, "There are no new Notification");
                    } else {
                        for (WebElement NotificationDetail : txt_NotificationDetails) {
                            isDisplayed(testContext, NotificationDetail);
                        }
                        takeScreenshot(testContext);

                    }
                }catch(Exception e){
                    logToHtmlReport(testContext, "There are no new Notification");
                }
                break;

            case "View Details":
                try {
                    if (!btn_ViewDetails.isDisplayed()) {
                        logToHtmlReport(testContext, "View Details Button not available due to there are no new Notification");
                    } else {
                        waitForElementToBeVisible(driver, btn_ViewDetails);
                        isDisplayed(testContext, btn_ViewDetails);
                        takeScreenshot(testContext);
                    }

                }catch(Exception e){
                    logToHtmlReport(testContext, "View Details Button not available due to there are no new Notification");
                }
                break;

            default:
                throw new RuntimeException("Invalid profile text passed");
        }
    }


=========================================================================================================================================================
Set<String> allWindows = driver.getWindowHandles();
                List<String> windowID = new ArrayList<>(allWindows);
                String parentID = windowID.get(0);
                String childID = windowID.get(1);
                driver.switchTo().window(childID);
                String currentURL = driver.getCurrentUrl();
                logToHtmlReport(testContext, "Current_Page_URL:" + currentURL);
                takeScreenshot(testContext);
                Thread.sleep(4000);
                driver.switchTo().window(parentID);

=========================================================================================================================================================
 public void verifyNomineeDateOfBirthBelowEighteen(TestContext testContext, String s) {
        String name = ipt_nomineeDateOfBirth.getAttribute("value");
        name = name.trim();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
        LocalDate dob = LocalDate.parse(name, formatter);
        LocalDate currentDate = LocalDate.now();
        //Current age
        int age = Period.between(dob, currentDate).getYears();
        if (age < 18) {
            validate(testContext, s, name, "nominee age is correct");
            takeScreenshot(testContext);
        } else {
            throw new RuntimeException("incorrect nominee age");
        }
    }
=========================================================================================================================================================

When the user navigates to the "Life Insurance" screen via the "LIFE INSURANCE" card


 public void SelectCard(TestContext testContext, String txt2, String card) throws Throwable {
        waitForLoaderOverlayToVanishInWebPortal(driver);
        int counter = 0;
        while (counter < 10) {
            waitForElementToBeClickable(driver, btn_navigate);
            click(testContext, btn_navigate, "");
            WebElement lbl_CardName = driver.findElement(By.xpath("//*[contains(text(),'" + card + "')]"));
            WebElement btn_Card = driver.findElement(By.xpath("//*[contains(text(),'" + card + "')]//following::a[1]"));
            if (lbl_CardName.isDisplayed() && lbl_CardName.getText().equals(card) && btn_Card.isDisplayed()) {
                click(testContext, btn_Card, card);
                break;
            } else {
                clickUntilVisible12(testContext);
            }
            counter++;
        }
        takeScreenshot(testContext);
    }

    public void clickUntilVisible12(TestContext testContext) throws Throwable {
        Thread.sleep(2000);
        click(testContext, btn_navigate, "");
    }

=========================================================================================================================================================

    public double calculateTotalFDAmount(TestContext testContext) {
        // Find all the FD elements
        scrollToEndOfAppPage(testContext);
        singleScrollToEndOfAppPage(testContext);
//        scrollToMobileElement(testContext,textAttributeBasedOnDriver,ele_last);
        List<WebElement> allFDs = AllFD;
        double totalSum = 0.0;

        // Iterate through each element
        for (WebElement fdElement : allFDs) {
            // Extract the visible text using getText() or getAttribute("content-desc")
            // For Appium/Android, 'content-desc' is often the most reliable attribute
            String elementText = fdElement.getDomAttribute("content-desc");

            // Extract the numeric value from the text string using a regular expression
            double amount = extractAmountFromString(elementText);

            // Add to the total sum
            totalSum += amount;
        }

        return totalSum;
    }
    private double extractAmountFromString(String text) {
        // Pattern to find a number, possibly with decimals
        Pattern pattern = Pattern.compile("([0-9]+\\.[0-9]+|[0-9]+)");
        Matcher matcher = pattern.matcher(text);

        if (matcher.find()) {
            return Double.parseDouble(matcher.group(1));
        }
        return 0.0;
    }
    public double getCombinedCurrentValue() {
        // You need a specific locator for the "Combined Current Value" display
        WebElement combinedValueElement = driver.findElement(By.xpath("//android.view.View[@content-desc=\"COMBINED CURRENT VALUE\"]/following-sibling::android.view.View"));
        String combinedValueText = combinedValueElement.getDomAttribute("content-desc"); // Use getText() or getAttribute() as appropriate for your app
        return extractAmountFromString(combinedValueText);
    }

    public void verifyFDTotalValue(TestContext testContext) {
        double calculatedTotal = calculateTotalFDAmount(testContext);
        double expectedCombinedValue = getCombinedCurrentValue();

        // Use TestNG/JUnit assertion to compare the values
        validate(testContext,calculatedTotal,expectedCombinedValue,"Addition of all Deposits with combined value",true);

    }





