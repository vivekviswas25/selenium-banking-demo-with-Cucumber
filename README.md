# Banking Application BDD Test Automation Framework 🏦

## 📋 Overview
Enterprise-grade **Behavior-Driven Development (BDD)** test automation framework for [Guru99 Banking Demo](https://demo.guru99.com/V4/) application. Combines **Cucumber** with **Selenium WebDriver** and **TestNG** to deliver business-readable test scenarios using Gherkin syntax. Implements **Page Object Model** design pattern with centralized configuration, parallel execution, and cross-browser support.

## 🛠️ Tech Stack
- **Language**: Java 17
- **Automation Tool**: Selenium WebDriver 4.27.0
- **Testing Framework**: TestNG 7.10.2
- **BDD Framework**: Cucumber 7.15.0 (cucumber-java, cucumber-testng)
- **Build Tool**: Maven 3.6+
- **Driver Management**: WebDriverManager 5.9.2
- **Design Patterns**: Page Object Model (POM) + BDD
- **Browsers**: Chrome, Firefox, Edge (parallel execution)

## 📁 Project Structure
```
selenium-banking-demo-with-Cucumber/
├── src/
│   ├── main/java/
│   │   ├── pages/                          # Page Object classes
│   │   │   ├── LoginPage.java             # Login page interactions
│   │   │   └── DashboardPage.java         # Dashboard & sub-pages
│   │   └── utils/                          # Utility classes
│   │       └── DriverManager.java         # Browser driver management
│   └── test/
│       ├── java/
│       │   ├── cucumber/                   # Cucumber BDD components
│       │   │   ├── runners/
│       │   │   │   └── TestRunner.java    # Cucumber TestNG runner
│       │   │   └── stepdefinitions/       # Step definitions (glue code)
│       │   │       ├── Hooks.java         # Before/After hooks
│       │   │       ├── LoginSteps.java    # Login step definitions
│       │   │       ├── DashboardSteps.java
│       │   │       └── BalanceEnquirySteps.java
│       │   └── tests/
│       │       └── BaseTest.java          # Base test configuration
│       └── resources/
│           └── features/                   # Gherkin feature files
│               ├── Login.feature          # Login scenarios
│               ├── Dashboard.feature      # Dashboard scenarios
│               └── BalanceEnquiry.feature # Balance enquiry scenarios
├── pom.xml                                 # Maven dependencies
├── testng.xml                              # TestNG parallel execution config
└── README.md
```

## ✨ Key Features

### 🎯 BDD/Cucumber Features
- ✅ **Gherkin Syntax** - Business-readable test scenarios (Given-When-Then)
- ✅ **Feature Files** - Organized test scenarios by business functionality
- ✅ **Reusable Step Definitions** - DRY principle for test automation
- ✅ **Cucumber-TestNG Integration** - Leverage TestNG's parallel execution
- ✅ **HTML Reports** - Built-in Cucumber HTML reporting
- ✅ **Hooks** - Centralized setup/teardown with browser management

### 🏗️ Test Architecture
- ✅ **Page Object Model** - Separation of page logic and test logic
- ✅ **Centralized Configuration** - Login credentials in `BaseTest`
- ✅ **Helper Methods** - `loginToDashboard()` to eliminate code duplication
- ✅ **Robust Element Interaction** - JavaScript click fallback for reliability
- ✅ **Explicit Waits** - Smart waiting strategies for stable tests

### 🚀 Execution Features
- ✅ **Parallel Execution** - Run tests across 3 browsers simultaneously
- ✅ **Cross-Browser Testing** - Chrome, Firefox, Edge support
- ✅ **Headless Mode** - Fast execution without GUI (CI/CD ready)
- ✅ **Thread-Safe Driver** - ThreadLocal for parallel test safety
- ✅ **Automatic Driver Management** - No manual driver downloads needed

### 🧪 Test Coverage (Feature-Based)

**Login Feature:**
- ✅ Successful login with valid credentials
- ✅ Login failure with invalid credentials
- ✅ Login failure with empty credentials

**Dashboard Feature:**
- ✅ Dashboard elements verification (title, manager ID)
- ✅ Navigation to New Customer form
- ✅ Successful logout functionality

**Balance Enquiry Feature:**
- ✅ Empty account number validation
- ✅ Numeric-only field validation
- ✅ Special characters validation
- ✅ Non-existent account handling

## 🚀 Getting Started

### Prerequisites
- **Java JDK 17** or higher ([Download](https://adoptium.net/))
- **Maven 3.6+** ([Download](https://maven.apache.org/download.cgi))
- **Browsers**: Chrome, Firefox, and/or Edge

### Installation
1. **Clone the repository:**
```bash
git clone https://github.com/jodieweiwei/selenium-banking-demo.git
cd selenium-banking-demo-with-Cucumber
```

2. **Install dependencies:**
```bash
mvn clean install
```

## 🎮 Running Tests

### Run All Cucumber Tests (Parallel Execution)
```bash
mvn clean test
```
This executes all feature files across Chrome, Firefox, and Edge in parallel via `testng.xml`.

### Run in Headless Mode (Faster - No Browser Windows)
```bash
mvn clean test -Dheadless=true
```

### Run Specific Feature File
```bash
mvn test -Dcucumber.features="src/test/resources/features/Login.feature"
```

### Run via TestNG XML
```bash
mvn test -DsuiteXmlFile=testng.xml
```

### View Cucumber HTML Reports
After test execution, open:
```
target/cucumber-reports.html
```

## 📊 Test Execution Configuration

### Parallel Execution (testng.xml)
```xml
<suite name="Banking Automation Suite" parallel="tests" thread-count="3">
    <test name="Chrome Tests">
        <parameter name="browser" value="chrome" />
        <classes>
            <class name="cucumber.runners.TestRunner"/>
        </classes>
    </test>
    <!-- Firefox and Edge tests configured similarly -->
</suite>
```

### Browser Configuration
- **Default**: Headless mode OFF (windows visible)
- **Headless**: Add `-Dheadless=true` to command line
- **Browser**: Configurable per test via TestNG parameters in `testng.xml`

## 🏗️ Framework Highlights

### BDD Feature File Example
```gherkin
Feature: Login Functionality
  As a bank manager
  I want to log in to the banking system
  So that I can manage customers and accounts

  Background:
    Given I am on the login page

  Scenario: Successful Login with Valid Credentials
    When I enter valid credentials
    And I click the login button
    Then I should be redirected to the dashboard

  Scenario: Login Failure with Invalid Credentials
    When I enter invalid credentials "invalid_user" and "invalid_pass"
    And I click the login button
    Then I should see an error alert
```

### Step Definition Example
```java
@Given("I am on the login page")
public void i_am_on_the_login_page() {
    driver.get("https://demo.guru99.com/V4/");
}

@When("I enter valid credentials")
public void i_enter_valid_credentials() {
    LoginPage loginPage = new LoginPage(driver);
    loginPage.enterUsername("mngr647463");
    loginPage.enterPassword("EtUgYbU");
}

@Then("I should be redirected to the dashboard")
public void i_should_be_redirected_to_the_dashboard() {
    DashboardPage dashboard = new DashboardPage(driver);
    assertTrue(dashboard.isManagerIdDisplayed());
}
```

### Cucumber Hooks (Setup/Teardown)
```java
@Before
public void setUp() {
    String browser = DriverManager.getBrowser();
    driver = DriverManager.getDriver();
    driver.manage().window().maximize();
}

@After
public void tearDown(Scenario scenario) {
    if (scenario.isFailed()) {
        // Optional: Take screenshot on failure
    }
    DriverManager.quitDriver();
}
```

### Centralized Login Helper
```java
// In BaseTest.java
protected String loginUsername = "mngr647463";
protected String loginPw = "EtUgYbU";

protected DashboardPage loginToDashboard() {
    LoginPage loginPage = new LoginPage(driver);
    return loginPage.login(loginUsername, loginPw);
}
```

### Robust Click Mechanism
```java
// JavaScript fallback if regular click fails
private void waitSubMenuIsClickableAndClick(By menuLocator) {
    try {
        wait.until(ExpectedConditions.elementToBeClickable(menuLocator)).click();
    } catch (Exception e) {
        System.out.println("Regular click failed, using JavaScript click");
        ((JavascriptExecutor) driver).executeScript("arguments[0].click();", menuButton);
    }
}
```

### Headless Mode Support
```java
// Configurable via system property
boolean isHeadless = Boolean.parseBoolean(System.getProperty("headless", "false"));
if (isHeadless) {
    chromeOptions.addArguments("--headless=new");
}
```

## 📈 Test Results

### Latest Test Run
- **Total Scenarios**: Multiple scenarios across 3 feature files
- **Browsers**: Chrome, Firefox, Edge
- **Execution Mode**: Parallel (3 threads)
- **Reports**: Cucumber HTML reports in `target/cucumber-reports.html`

### Sample Test Reports
![Test Report](screenshots/Screenshot-2026-01-07-182750.png)
![Test Report](screenshots/Screenshot-2025-11-25-191113.png)
![Test Report](screenshots/Screenshot-2025-11-14-230118.png)
![Test Report](screenshots/Screenshot-2025-11-12-132201.png)
![Test Report](screenshots/Screenshot-2025-11-06-182750.png)
![Test Report](screenshots/test-report.png)

## 📝 Test Credentials

### Demo Application
- **URL**: https://demo.guru99.com/V4/
- **Username**: `mngr647463`
- **Password**: `EtUgYbU`

### Getting New Credentials
If credentials expire:
1. Visit https://demo.guru99.com/V4/
2. Check homepage for demo credentials
3. Or click "Get Free Credentials" for new ones via email

**Alternative**: Use [ParaBank](https://parabank.parasoft.com) for more stable practice environment

## 🎯 Future Enhancements
- [ ] Allure reporting integration for richer test reports
- [ ] Cucumber Scenario Outlines for data-driven testing
- [ ] Data tables for complex test data
- [ ] ExtentReports integration
- [ ] Screenshot capture on test failure (automated)
- [ ] API testing integration (RestAssured)
- [ ] CI/CD pipeline (GitHub Actions/Jenkins)
- [ ] Docker containerization
- [ ] Tag-based test execution (@smoke, @regression)

## 🐛 Troubleshooting

### Common Issues

**Issue**: Tests fail in Firefox
- **Solution**: Added explicit waits and JavaScript click fallback in DashboardPage

**Issue**: "CDP implementation matching" warnings
- **Note**: These warnings are harmless, tests will still run successfully

**Issue**: Element not clickable
- **Solution**: Framework uses JavaScript click as fallback automatically

**Issue**: Cucumber can't find step definitions
- **Solution**: Ensure `glue` parameter in TestRunner points to `cucumber.stepdefinitions`

**Issue**: Feature files not found
- **Solution**: Verify `features` parameter in TestRunner: `src/test/resources/features`

## 👥 Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

## 👤 Author
**Wei Wei (Jodie)**
- 🔗 LinkedIn: [wei-wei-jodie](https://www.linkedin.com/in/wei-wei-jodie/)
- 📧 Email: jodieweiwei@gmail.com
- 🐙 GitHub: [jodieweiwei](https://github.com/jodieweiwei)

## 📄 License
This project is for educational and demonstration purposes.

## 🙏 Acknowledgments
- Guru99 for providing the demo banking application
- Selenium WebDriver community
- TestNG framework contributors
- Cucumber BDD community

---
