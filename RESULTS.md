# UI lab results

Tool parts that I chose: Part 1 - Selenium IDE and Part 4 - Cypress

## Part 0

- Step 0.1, Chrome version: Google Chrome 158.0.8127.36 (Official Build) (64-bit)
- Step 0.2, ChromeDriver: downloaded outside the repository under `C:\drivers` so the driver binary is not committed with the lab
- Step 0.3, ChromeDriver version: 158.0.8127.36, matching the Chrome major version
- Step 0.3, environment variable: `CHROMEDRIVER` points to the driver outside the repository and is added to `PATH` only when running Selenium IDE
- Step 0.4, locator of Pizza, Please!: `name=pizza-please`
- Step 0.4, locator of Excluded tools: `css=select`
- Step 0.4, locator of Must be vegetarian: `id=default-checkbox`
- Step 0.4, locator of Custom Pizza Name: `css=input#pizza-name`
- Step 0.4, locator note: `#floating_filled` is reused by three number inputs, so the calories input is located from its label instead of the duplicated id
- Step 0.5, pizza name, tool and calories of the run by hand: `Lab Pizza 20261119-091205`, `Scissors`, 525 calories per slice
- Step 0.5, manual run note: the pizza name includes the date and time so repeated runs do not collide with an earlier pizza

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1 | The heading read `Looking to break out of your pizza routine?` | Pass |
| 2 | Advanced showed Max Calories per Slice, Excluded tools, Must be vegetarian and Custom Pizza Name | Pass |
| 3 | Entered 600, selected Knife, checked vegetarian and entered the timestamped pizza name | Pass |
| 4 | The answer showed the pizza name, returned `Scissors`, and 525 calories per slice | Pass |
| 5 | Clicking Love it! without logging in showed `Please log in first.` | Pass |
| 6 | The admin page showed `Latest pizza recommendations` after signing in as admin/admin | Pass |
| 7 | The admin list contained the pizza name and the same tool, `Scissors` | Pass |

## Part 1: Selenium IDE

- Firefox version and Node.js version: Firefox 159.0.1; Node.js v24.22.1
- Step 1.1, Selenium IDE version: 3.17.4
- Step 1.3, runner command: `npx --yes selenium-side-runner@4.0.13 ui-lab.side`; the web-form test completed successfully
- Step 1.4, the failed check: `Actual value 'Received!' did not match 'Received' during assertText:id=message:Received`; `Tests: 1 failed, 1 passed, 2 total`.
- Step 1.4, restored check: the assertion was changed back to `Received!`, and the final web-form run passed
- Step 1.5, target chosen for Submit: `xpath=//button[@type='submit']`; the recorded `css=.btn` was fragile because a class can be shared by several buttons.
- Step 1.5, target verification: the stable XPath was used in `ui-lab.side` instead of the recorded class selector
- Step 1.6, changes after recording: used full URLs, replaced the Submit target, added waits and assertions, removed the redundant option click, and added the complete QP-01 customer flow.
- Step 1.6, pizza names: run 1 `Lab Pizza 20261119-093744`; run 2 `Lab Pizza 20261119-094118`.
- Step 1.6, both runs: `Tests: 2 passed, 2 total`.
- Step 1.7, line that needs rewriting: the exported Java assertion that compares against the literal `vars.get("pizzaName").toString()`; it must use the variable value instead of a quoted expression.

## Part 4: Cypress

- Node.js: `v24.22.1`; npm: `11.19.1`; Cypress: `16.2.1`
- Step 4.3, intentional wrong-title failure: Cypress timed out while the title assertion expected `Web form` but the page showed `Web Form`; the assertion was restored before the successful run.
- Step 4.6, intercepted `/api/pizza` response: tool `Pizza cutter`, 425 calories per slice, status 200, and the request carried `maxCaloriesPerSlice: 600`.
- Step 4.7, admin-list line: `Lab Pizza 20261119-101743 (tool=Pizza cutter, ingredients_number=6) (newest)`.
- Step 4.8, run 1: PASS, 2 specs and 2 tests, 12 seconds total; pizza `Lab Pizza 20261119-100840`.
- Step 4.8, run 2: PASS, 2 specs and 2 tests, 12 seconds total; pizza `Lab Pizza 20261119-101743`.
- Step 4.9 (optional): Not run.

## Part B

1. The Max Calories per Slice input cannot be located reliably with `#floating_filled` because the page reuses that id on three number inputs. The test finds the label text and then the input next to that label. Giving this input a unique id would make the locator shorter, valid HTML and less likely to select the wrong field.
2. Step 7 is a consistency oracle: it compares the tool shown in the admin list with the tool returned by the recommendation. It would catch a fault where QuickPizza stored or displayed a different tool from the one that the customer actually received.
3. The test could check the `vegetarian` field in the intercepted `/api/pizza` response and verify that it is `true`. A UI-only check would need a visible vegetarian result or ingredient information on the page; the current page text alone is not enough to prove the property.
4. Selenium IDE records commands quickly and waits with explicit commands such as `wait for element visible`; its saved project is easy to replay but its failure details are less convenient to review. Cypress uses automatic retrying assertions, `cy.intercept` and a custom command, so QP-01 is concise and its command log, screenshots and video make the run easy to inspect.
5. I would choose Cypress for Project 2. Its assertions automatically retry while the page changes, and `cy.intercept` can verify both the request and the response while the UI test checks the page. The full-run screenshots and videos also provide direct evidence of what the user saw.
6. For Part 1, the recorder's `css=.btn` is fragile because any new button sharing that class could be selected. `xpath=//button[@type='submit']` is more stable because it identifies the button's semantic type.
7. For Part 4, QP-01 must stay one Cypress test because Cypress clears cookies before each test. If steps 6 and 7 were a second test, the browser could lose the cookie that pins it to the same QuickPizza server, and the second test would not reliably see the pizza created by steps 1 to 5.
