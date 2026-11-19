# UI Lab

งานนี้เลือกทำ **Part 1: Selenium IDE** และ **Part 4: Cypress** ตามโจทย์ Automated UI Testing โดยทดสอบ Selenium Web Form และกรณีทดสอบ QP-01 ของ QuickPizza พร้อมจัดทำผลการทดสอบ หลักฐานภาพหน้าจอและวิดีโอ รวมถึงคำตอบการวิเคราะห์ตาม Part 0 และ Part B

## Selenium IDE

ไฟล์ `selenium-ide/ui-lab.side` เก็บการทดสอบเว็บฟอร์มด้วย Selenium IDE และการทดสอบลูกค้า QP-01 ส่วนไฟล์ `QP01customerTest.java` เป็นไฟล์ที่ export จากขั้นตอน 1.7 จากโฟลเดอร์ `selenium-ide` ให้ตั้งค่า `CHROMEDRIVER` ใน `PATH` แล้วรันคำสั่งต่อไปนี้:

```powershell
$env:Path = (Split-Path $env:CHROMEDRIVER) + ";" + $env:Path
npx --yes selenium-side-runner@4.0.13 ui-lab.side
```

## Cypress

โฟลเดอร์ `cypress/` เก็บสเปกเว็บฟอร์ม, สถานการณ์ QP-01 แบบเทสต์เดียว, การตรวจสอบ API ด้วย `cy.intercept` และคำสั่งกำหนดเองสำหรับล็อกอินแอดมิน จากโฟลเดอร์นี้ให้ติดตั้งแพ็กเกจ แล้วรันสเปกทั้งสองด้วย Chrome:

```powershell
npm install
npx cypress run --browser chrome
```

ภาพหน้าจอและวิดีโอใน `cypress/cypress/` เป็นหลักฐานจากการรันเต็มทั้งสองรอบ
