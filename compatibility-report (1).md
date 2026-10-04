# CodeCraft Infotech - Quality Assurance Internship

## Task 2: Compatibility Testing for a Basic Web Page

### Objective
To test a web page across different browsers and devices, check for layout issues, broken links, and functionality discrepancies, and document findings with recommended fixes.

### Application Under Test
**Site:** Swag Labs demo e-commerce site — [https://www.saucedemo.com](https://www.saucedemo.com)
**Flow tested:** Product listing → Add to cart → View cart → Checkout (info) → Checkout (overview) → Order confirmation

### Environments Tested

| Browser / Device | OS | Status |
|---|---|---|
| Google Chrome (desktop) | Windows | Tested |
| Microsoft Edge (desktop) | Windows | Tested |
| Mozilla Firefox (desktop) | Windows | Tested |
| Google Chrome (mobile) | Android | Tested |
| Safari | — | Not tested — Safari is not available on Windows and no iOS/macOS device was accessible during testing |

### Test Scenarios

For each environment, three scenarios were run through the full checkout flow:

1. **Valid flow** — add an item to cart, enter valid checkout info, complete the order.
2. **Empty cart flow** — go to checkout with zero items in the cart.
3. **Invalid info flow** — enter non-conforming data in the checkout info fields (numbers in name fields, symbols in zip code).

### Compatibility Matrix

| Check | Chrome (desktop) | Edge (desktop) | Firefox (desktop) | Chrome (mobile) |
|---|---|---|---|---|
| Product page layout renders correctly | Pass | Pass | Pass | Pass |
| Add to cart updates cart badge | Pass | Pass | Pass | Pass |
| Cart page shows correct item/price | Pass | Pass | Pass | Pass |
| Checkout info form accepts valid input | Pass | Pass | Pass | Pass |
| Order totals calculate correctly (valid flow) | Pass ($32.39) | Pass ($32.39) | Pass ($32.39) | Pass ($32.39) |
| Order confirmation page displays | Pass | Pass | Pass | Pass |
| Empty cart blocked from checkout | **Fail** | **Fail** | **Fail** | **Fail** |
| Checkout info validates field content | **Fail** | **Fail** | **Fail** | **Fail** |
| No broken links/images observed | Pass | Pass | Pass | Pass |

No layout shifts, broken images, or broken links were observed across any of the four tested environments. Visual rendering was consistent on all of them. The two issues below are functional/logic defects, not compatibility differences — both reproduce identically on every environment tested, which indicates the problem is in the site's shared logic rather than any one browser.

### Findings

---

**DEF-001 — Checkout permits an order with an empty cart**

| Field | Details |
|---|---|
| Severity | Medium |
| Browsers confirmed | Chrome (desktop), Edge (desktop), Firefox (desktop), Chrome (mobile) — reproduces on all four tested environments |
| Steps to Reproduce | 1. Go to the cart page with 0 items in it<br>2. Click Checkout<br>3. Enter valid info on "Checkout: Your Information" and click Continue<br>4. On "Checkout: Overview", observe Item total: $0, Tax: $0.00, Total: $0.00<br>5. Click Finish |
| Expected Result | The site should block checkout when the cart is empty (e.g. disable the Checkout button, or show a message such as "Your cart is empty") |
| Actual Result | The full checkout flow completes normally and shows the "Thank you for your order!" confirmation page, despite there being nothing in the cart |
| Suggested Fix | Add a cart-not-empty check before allowing checkout to proceed, either by disabling the Checkout button on an empty cart or validating cart contents before showing the Overview/confirmation pages |
| Status | Open |
| Evidence | `screenshots/chrome-desktop/empty-cart/`, `screenshots/edge-desktop/empty-cart/`, `screenshots/firefox-desktop/empty-cart/`, `screenshots/chrome-mobile/empty-cart/` |

---

**DEF-002 — Checkout info form has no input validation**

| Field | Details |
|---|---|
| Severity | Low–Medium |
| Browsers confirmed | Chrome (desktop), Edge (desktop), Firefox (desktop), Chrome (mobile) |
| Steps to Reproduce | 1. Add an item to cart and go to Checkout<br>2. On "Checkout: Your Information", enter a number or symbol string in First Name (e.g. `12345`), Last Name (e.g. `@aed_`), and Zip/Postal Code (e.g. `41@#78`)<br>3. Click Continue |
| Expected Result | The form should reject non-alphabetic characters in First/Last Name and non-numeric or malformed input in Zip/Postal Code, with a validation message |
| Actual Result | All tested combinations of numbers, symbols, and mixed characters were accepted with no validation error, and checkout proceeded normally through to order confirmation |
| Suggested Fix | Add client-side (and server-side) validation: restrict Name fields to letters/spaces/hyphens, and restrict Zip/Postal Code to the expected numeric or alphanumeric postal format for the target region |
| Status | Open |
| Evidence | `screenshots/chrome-desktop/invalid-info/`, `screenshots/edge-desktop/invalid-info/`, `screenshots/firefox-desktop/invalid-info/` |

---

### Screenshot Index

All evidence is under `screenshots/`, organized by browser/device and scenario:

```
screenshots/
├── chrome-desktop/
│   ├── valid-flow/        (01 products → 05 order complete, $32.39 total)
│   ├── empty-cart/        (01 empty cart → 05 order complete, DEF-001)
│   └── invalid-info/      (01 cart → 04 order complete, DEF-002)
├── edge-desktop/
│   ├── valid-flow/        (01 products → 05 order complete, $32.39 total)
│   ├── empty-cart/        (01 empty cart → 04 order complete, DEF-001)
│   └── invalid-info/      (01 invalid info → 03 order complete, DEF-002)
├── firefox-desktop/
│   ├── valid-flow/        (01 products → 06 order complete, $32.39 total)
│   ├── empty-cart/        (01 empty cart → 04 order complete, DEF-001)
│   └── invalid-info/      (01 cart → 04 order complete, DEF-002)
└── chrome-mobile/
    ├── valid-flow/        (01 products → 05 order complete, $32.39 total)
    └── empty-cart/        (01 empty cart → 04 order complete, DEF-001)
```

### Conclusion

Across four environments (Chrome desktop, Edge desktop, Firefox desktop, Chrome mobile), the Swag Labs demo site rendered consistently with no layout, image, or link issues. Both functional defects — checkout allowing an empty-cart order, and no input validation on the checkout info form — were confirmed to reproduce identically on **all four** tested environments, which points to the bugs being in the site's shared application logic rather than browser-specific rendering differences. Safari was not tested due to lack of access to Apple hardware or software during this testing cycle.
