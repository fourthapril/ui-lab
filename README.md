# UI testing lab

This repository contains the two selected tool parts from the lab: Part 1 (Selenium IDE) and Part 4 (Cypress), plus the shared Part 0 and Part B work.

## Selenium IDE

`selenium-ide/ui-lab.side` contains the Selenium IDE web-form test and the QP-01 customer test. The exported `QP01customerTest.java` is kept as the Step 1.7 deliverable. From `selenium-ide`, set `CHROMEDRIVER` on `PATH` and run:

```powershell
$env:Path = (Split-Path $env:CHROMEDRIVER) + ";" + $env:Path
npx --yes selenium-side-runner@4.0.13 ui-lab.side
```

## Cypress

`cypress/` contains the web-form spec, the single-test QP-01 scenario, the `cy.intercept` API check and the reusable admin-login custom command. From that folder, install the package and run both specs in Chrome:

```powershell
npm install
npx cypress run --browser chrome
```

The screenshots and videos under `cypress/cypress/` are the evidence from the two full-run executions.
