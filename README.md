# cucumber-bdd-automation-framework
The framework follows industry best practices and is production-ready. You can easily adapt it for different applications by updating the configuration files and adding domain-specific page objects and API clients.

A comprehensive, production-ready BDD (Behavior-Driven Development) automation framework built with Cucumber.js, Selenium WebDriver, and modern testing practices. Features UI & API testing, parallel execution, Docker support, and clean architecture for enterprise-scale test automation.

## 🚀 Features

- **Multi-Domain Support**: Easily adaptable for different applications and domains
- **UI & API Testing**: Comprehensive support for both web UI and API testing
- **Page Object Model**: Clean, maintainable page object implementation
- **Parallel Execution**: Run tests in parallel for faster execution
- **Rich Reporting**: HTML reports with screenshots and detailed logs
- **Cross-Browser Support**: Chrome, Firefox, and other browsers
- **Environment Management**: Multiple environment configurations
- **Data-Driven Testing**: JSON-based test data management
- **Logging & Debugging**: Comprehensive logging with Winston
- **Code Quality**: ESLint and Prettier integration

## 📁 Project Structure

```
cucumber-bdd-automation-framework/
├── src/
│   ├── features/                 # Feature files
│   │   ├── ui/                  # UI test features
│   │   └── api/                 # API test features
│   ├── step-definitions/        # Step definition files
│   │   ├── ui/                  # UI step definitions
│   │   ├── api/                 # API step definitions
│   │   └── common/              # Common/shared step definitions
│   ├── page-objects/            # Page Object Model classes
│   ├── api/                     # API client classes
│   ├── support/                 # Test support files
│   │   ├── world.js            # Custom World constructor
│   │   └── hooks.js            # Before/After hooks
│   ├── utils/                   # Utility functions
│   └── test-data/              # Test data files
├── reports/                     # Test reports and screenshots
├── logs/                       # Application logs
├── scripts/                    # Utility scripts
├── cucumber.js                 # Cucumber configuration
├── package.json               # Dependencies and scripts
└── README.md                  # This file
```

## 🛠 Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd cucumber-bdd-automation-framework
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Setup environment variables**
   ```bash
   cp .env.example .env
   # Edit .env file with your configuration
   ```

4. **Install browser drivers**
   - Chrome: Download ChromeDriver and add to PATH
   - Firefox: Download GeckoDriver and add to PATH

## 🚦 Quick Start

### Running Tests

```bash
# Run all tests
npm test

# Run specific test suites
npm run test:smoke      # Smoke tests
npm run test:regression # Regression tests
npm run test:api        # API tests only
npm run test:ui         # UI tests only

# Run tests in parallel
npm run test:parallel

# Generate HTML report
npm run test:report
npm run generate-report
```

### Environment Configuration

Set environment variables in `.env` file:

```env
BROWSER=chrome
HEADLESS=false
BASE_URL=https://your-app.com
API_URL=https://api.your-app.com
TIMEOUT=30000
PARALLEL=2
```

## 📝 Writing Tests

### Feature Files

Create feature files in `src/features/` using Gherkin syntax:

```gherkin
@ui @login
Feature: User Login
  As a user
  I want to login to the application
  So that I can access my account

  @smoke
  Scenario: Successful login
    Given I am on the login page
    When I enter username "user@example.com"
    And I enter password "password123"
    And I click the login button
    Then I should be redirected to the dashboard
```

### Step Definitions

Implement step definitions in `src/step-definitions/`:

```javascript
const { Given, When, Then } = require('@cucumber/cucumber');

Given('I am on the login page', async function () {
  await this.initializeBrowser();
  await this.loginPage.navigateToLogin();
});

When('I enter username {string}', async function (username) {
  await this.loginPage.enterUsername(username);
});
```

### Page Objects

Create page objects in `src/page-objects/`:

```javascript
const BasePage = require('./base-page');

class LoginPage extends BasePage {
  constructor(driver) {
    super(driver);
    this.elements = {
      usernameField: By.id('username'),
      passwordField: By.id('password'),
      loginButton: By.css('button[type="submit"]')
    };
  }

  async enterUsername(username) {
    await this.type(this.elements.usernameField, username);
  }
}
```

## 🏗 Framework Architecture

### Core Components

1. **World Constructor** (`src/support/world.js`)
   - Manages browser instances
   - Handles API clients
   - Stores test data and context

2. **Base Page** (`src/page-objects/base-page.js`)
   - Common web element interactions
   - Wait strategies
   - Error handling

3. **Base API** (`src/api/base-api.js`)
   - HTTP client wrapper
   - Request/response handling
   - Authentication management

4. **Hooks** (`src/support/hooks.js`)
   - Test setup and teardown
   - Screenshot capture on failure
   - Logging and reporting

### Best Practices Implemented

- **Single Responsibility**: Each class has a single, well-defined purpose
- **DRY Principle**: Common functionality is abstracted into base classes
- **Page Object Model**: UI elements and actions are encapsulated
- **Data Separation**: Test data is externalized in JSON files
- **Error Handling**: Comprehensive error handling and logging
- **Parallel Execution**: Tests can run in parallel for faster feedback

## 🔧 Configuration

### Cucumber Configuration (`cucumber.js`)

```javascript
module.exports = {
  default: {
    features: ['src/features/**/*.feature'],
    require: ['src/step-definitions/**/*.js', 'src/support/**/*.js'],
    format: ['progress-bar', 'json:reports/cucumber-report.json'],
    parallel: 2,
    retry: 1
  }
};
```

### Browser Configuration

Supports multiple browsers with customizable options:
- Chrome (default)
- Firefox
- Headless mode
- Custom browser options

## 📊 Reporting

The framework generates comprehensive reports:

- **HTML Reports**: Visual test execution reports
- **JSON Reports**: Machine-readable test results
- **Screenshots**: Automatic capture on test failures
- **Logs**: Detailed execution logs with Winston

Generate reports:
```bash
npm run generate-report
```

## 🧪 Test Data Management

Test data is managed through JSON files in `src/test-data/`:

```json
{
  "validUser": {
    "username": "testuser@example.com",
    "password": "ValidPass123!",
    "name": "Test User"
  },
  "invalidUsers": [
    {
      "username": "",
      "password": "ValidPass123!",
      "expectedError": "Username is required"
    }
  ]
}
```

## 🏷 Tagging Strategy

Use tags to organize and filter tests:

- `@smoke`: Critical functionality tests
- `@regression`: Full regression test suite
- `@ui`: User interface tests
- `@api`: API tests
- `@positive`: Happy path scenarios
- `@negative`: Error handling scenarios

## 🔍 Debugging

### Logging

The framework uses Winston for structured logging:

```javascript
const logger = require('../utils/logger');
logger.info('Test step executed successfully');
logger.error('Test failed with error:', error);
```

### Screenshots

Screenshots are automatically captured on test failures and saved to `reports/screenshots/`.

### Debug Mode

Run tests with additional debugging:

```bash
DEBUG=true npm test
```

## 🚀 Extending the Framework

### Adding New Domains

1. Create domain-specific feature files
2. Implement corresponding page objects
3. Add domain-specific test data
4. Create custom step definitions if needed

### Custom Step Definitions

Add reusable step definitions in `src/step-definitions/common/`:

```javascript
Given('I wait for {int} seconds', async function (seconds) {
  await TestHelpers.wait(seconds * 1000);
});
```

### API Testing

Extend the API client for new endpoints:

```javascript
class CustomAPI extends BaseAPI {
  async getCustomData() {
    return await this.get('/custom-endpoint');
  }
}
```

## 📈 Performance Optimization

- **Parallel Execution**: Run tests concurrently
- **Smart Waits**: Efficient element waiting strategies
- **Resource Management**: Proper cleanup of browser instances
- **Selective Testing**: Use tags to run specific test subsets

## 🤝 Contributing

1. Follow the established coding standards
2. Write comprehensive tests for new features
3. Update documentation for any changes
4. Use meaningful commit messages
5. Run linting and formatting before commits

```bash
npm run lint
npm run format
```

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Support

For questions and support:
- Check the documentation
- Review existing issues
- Create detailed bug reports
- Follow the contribution guidelines

---

**Happy Testing! 🎉**
## 🌟 S
tar History

[![Star History Chart](https://api.star-history.com/svg?repos=YOUR_USERNAME/cucumber-bdd-automation-framework&type=Date)](https://star-history.com/#YOUR_USERNAME/cucumber-bdd-automation-framework&Date)

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Cucumber.js](https://cucumber.io/) for the BDD framework
- [Selenium WebDriver](https://www.selenium.dev/) for browser automation
- [Winston](https://github.com/winstonjs/winston) for logging
- [Chai](https://www.chaijs.com/) for assertions
- All contributors who help improve this framework

## 📞 Support

- 📖 [Documentation](https://github.com/YOUR_USERNAME/cucumber-bdd-automation-framework/wiki)
- 🐛 [Issue Tracker](https://github.com/YOUR_USERNAME/cucumber-bdd-automation-framework/issues)
- 💬 [Discussions](https://github.com/YOUR_USERNAME/cucumber-bdd-automation-framework/discussions)
- ⭐ Star this repo if you find it helpful!

---

**Made with ❤️ for the QA Community**
