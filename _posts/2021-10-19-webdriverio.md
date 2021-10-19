---
published: false
---
## WebDriverIO

# Devices Farm PoC

Cross-browser and BDD

![server-cluster](https://s3.amazonaws.com/media-p.slid.es/uploads/2008705/images/9029560/undraw_server_cluster_jwwq.svg)

---

# Current Implementation

Jest + Puppeteer

![](https://s3.amazonaws.com/media-p.slid.es/uploads/2008705/images/9028788/pasted-from-clipboard.png)

# Jest

-   JavaScript Testing Framework with a focus on simplicity
-   Test structure and expects

![](https://s3.amazonaws.com/media-p.slid.es/uploads/2008705/images/9028784/pasted-from-clipboard.png)

# Puppeteer

-   High-level API to control Chrome or Chromium over the DevTools Protocol.

-   Navigation, selectors...

![](https://s3.amazonaws.com/media-p.slid.es/uploads/2008705/images/9028783/pasted-from-clipboard.png)

# Problems

-   Puppeteer only works on Chrome-based browsers
-   Lack of testing features

---

# WebDriverIO

Next-gen browser and mobile automation test framework for Node.js

-   Extendable
-   Cross-browser Support
-   Community Plugins

![](https://s3.amazonaws.com/media-p.slid.es/uploads/2008705/images/9029594/pasted-from-clipboard.png)

# Google Lighthouse Integration

```ts
await browser.emulateDevice("iPhone X");
await browser.enablePerformanceAudits({
    networkThrottling: "Good 3G",
    cacheEnabled: true,
    formFactor: "mobile",
});

// open application under test
await browser.url("https://localhost:3000");

expect(await browser.getMetrics().firstMeaningfulPaint).toBeBelow(2500);

const pwaCheckResult = await browser.checkPWA();
expect(pwaCheckResult.passed).toBe(true);
```

# Frameworks Support

```ts
await browser.url("https://ahfarmer.github.io/calculator/");
const appWrapper = await browser.$("div#root");

await browser
    .react$("t", {
        props: { name: "7" },
    })
    .click();
await browser
    .react$("t", {
        props: { name: "x" },
    })
    .click();
await browser
    .react$("t", {
        props: { name: "6" },
    })
    .click();
await browser
    .react$("t", {
        props: { name: "=" },
    })
    .click();
```

# Services

```ts
// wdio.conf.js
export.config = {
    // ...
    services: [
    	'devtools',
        'firefox-profile',
        'crossbrowsertesting',
        'chromedriver',
        'docker',
        'aws-device-farm',
        'ms-teams'
    ],
    // ...
};
```

# Used At


-   Google
-   Netflix
-   Microsoft
-   Mozilla
-   SAP
-   BBVA

---

# PoC

Microsoft Login

![](https://s3.amazonaws.com/media-p.slid.es/uploads/2008705/images/9029533/undraw_Login_re_4vu2.svg)

# Initial Configuration

![](https://s3.amazonaws.com/media-p.slid.es/uploads/2008705/images/9028851/pasted-from-clipboard.png)

# Scenarios

```gherkin
Feature: Login

    Scenario Outline: As a user, I can log into the secure area

        Given I am on the login page
        When I login with <username> and <password> using <loginType>
        Then I should see a logged in page

        Examples:
            | username               | password             | loginType |
            | testuser3@fit-ebike.ch | ******************** | social    |
            | testuser7@fit-ebike.ch | ******************** | fit       |

    Scenario Outline: As a user, I can logout from the secure area

        Given I am logged in with <username> and <password> using <loginType>
        When I logout
        Then I should see a logged out page

        Examples:
            | username               | password             | loginType |
            | testuser3@fit-ebike.ch | ******************** | social    |
            | testuser7@fit-ebike.ch | ******************** | fit       |

```

# Step Definitions

```ts
import { Given, Then, When } from "@wdio/cucumber-framework";
import { LoginPage } from "../pages/login.page";
import { SecurePage } from "../pages/secure.page";

const pages = {
    login: new LoginPage(),
    secure: new SecurePage(),
};

Given(/^I am on the (\w+) page$/, async (page) => {
    await pages[page].open();
});

Given(/^I am logged in with (.*) and (.*) using (.*)$/, async (username, password, loginType) => {
    await pages.login.open();
    await pages.login.login(username, password, loginType);
});

When(/^I login with (.*) and (.*) using (.*)$/, async (username, password, loginType) => {
    await pages.login.login(username, password, loginType);
});

When("I logout", async () => {
    await pages.secure.logout();
});

Then("I should see a logged in page", async () => {
    console.log("hola");
    await expect(pages.login.loggedCheck).toBeExisting();
});

Then("I should see a logged out page", async () => {
    await expect(pages.secure.loggedOutCheck).toBeExisting();
});
```

# Pages

## Login

```ts
/**
 * sub page containing specific selectors and methods for a specific page
 */
export class LoginPage extends Page {
    /**
     * define selectors using getter methods
     */
    get startLoginButton() {
        return $("#MsalAuthenticationModuleLoginBtn");
    }
    get fitLoginUsernameInput() {
        return $("#signInName");
    }
    ...

    /**
     * a method to encapsule automation code to interact with the page
     * e.g. to login using username and password
     */
    async login(username: string, password: string, loginType: LoginType) {
        await this.startLoginButton.waitForExist({ timeout: 30000 });
        await this.startLoginButton.click();

        switch (loginType) {
            case LoginType.FIT_ACCOUNT:
                await this.fitLogin(username, password);
                break;
            case LoginType.SOCIAL_ACCOUNT:
                await this.socialLogin(username, password);
                break;
        }

        await this.loggedCheck.waitForExist();
        await expect(this.loggedCheck.isDisplayed()).resolves.toBe(true);
    }

    ...
}
```

# Pages

## Secure

```ts
export class SecurePage extends Page {

    get logoutButton() {
        return $("#logoutBtn");
    }

    ...

    async logout() {
        await this.logoutButton.waitForExist();
        await this.logoutButton.click();

        // Sometimes there's a confirmation window, but not always
        try {__
            await this.account.waitForExist({ timeout: 3000 });
            await this.account.click();
        } catch {}

        await browser.deleteCookies();
        await browser.sendCommand("Network.clearBrowserCookies", {});
        await browser.sendCommand("Network.clearBrowserCache", {});

        await this.loggedOutCheck.waitForExist();
        expect(await this.loggedOutCheck.isExisting()).toBe(true);
    }
}
```
---
