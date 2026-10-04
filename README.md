# CODECRAFT_QA_02

## CodeCraft Infotech Quality Assurance Internship

### Task 2: Conduct Compatibility Testing for a Basic Web Page

#### Objective
To test a simple web page across different browsers and devices, check for layout issues, broken links, and functionality discrepancies, and document findings with recommended fixes.

#### Application Under Test
**Site:** Swag Labs demo e-commerce site — [https://www.saucedemo.com](https://www.saucedemo.com)

#### Environments Tested
- Google Chrome (desktop, Windows)
- Microsoft Edge (desktop, Windows)
- Mozilla Firefox (desktop, Windows)
- Google Chrome (mobile, Android)
- Safari — not tested (not available on Windows; no Apple device accessible)

#### Deliverable
The detailed compatibility matrix and findings are available in:

**[compatibility-report.md](compatibility-report.md)**

#### Summary
The product, cart, and checkout flow rendered consistently across all four tested browsers/devices with no layout, image, or broken-link issues. Two functional defects were found and confirmed to reproduce on every environment tested:

1. **DEF-001:** Checkout allows an order to be completed with an empty cart ($0.00 total).
2. **DEF-002:** The checkout info form accepts invalid data (numbers/symbols) in Name and Zip fields with no validation.

Full steps to reproduce, expected/actual results, and suggested fixes for both are in `compatibility-report.md`.

#### Internship
**Organization:** CodeCraft Infotech
**Track:** Quality Assurance
**Task:** 2 – Conduct Compatibility Testing for a Basic Web Page
