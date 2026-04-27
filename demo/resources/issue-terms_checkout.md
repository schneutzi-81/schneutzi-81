# Title: Add terms acceptance to checkout process

---

## Overview

Implement terms and conditions acceptance as a required step in the checkout process. Customers must acknowledge and
accept our terms before completing their purchase.

## Requirements

- Add a checkbox during checkout requiring users to accept terms and conditions
- Include a link to download the full terms and conditions document
- Prevent order completion until terms are accepted
- Store acceptance timestamp and IP address for legal compliance

## Dependencies

This feature is **blocked by** the "Allow downloading our terms and conditions" issue, as users need to be able to
access and review the terms via a downloadable link before they can properly accept them.

## Acceptance Criteria

- [ ] Checkbox appears on checkout page with clear terms acceptance text
- [ ] Download link for terms and conditions is prominently displayed
- [ ] Checkout cannot proceed without terms acceptance
- [ ] Acceptance is logged with timestamp and user information
- [ ] Link opens terms document in new tab/window for easy review
