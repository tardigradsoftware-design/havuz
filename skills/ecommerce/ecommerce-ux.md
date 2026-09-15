# E-Commerce UX Heuristics & Shopping Experience Architecture

## Purpose
Establishes high-converting, user-centric online shopping experiences based on Baymard Institute benchmarks, eliminating shopper hesitation and friction throughout the buying journey.

## When To Use
- When designing or developing online retail storefronts, product catalogs, shopping carts, and purchase flows.
- When evaluating user friction, navigation clarity, and transaction completion rates.

## When Not To Use
- For B2B enterprise software with contract-based custom invoicing and zero instant online checkout.

## Core Principles
1. **Zero-Surprise Pricing**: Display shipping estimates, taxes, and potential fees early in the funnel. Unexpected costs during the final checkout step are the #1 cause of cart abandonment (~48%).
2. **Instant Visual Feedback**: Adding an item to the cart must instantly open a responsive slide-out cart drawer with clear product details and a direct "Checkout" button.
3. **Effortless Discovery**: Provide predictive search, multi-faceted filtering, and intuitive category taxonomy so shoppers reach desired products in under 3 clicks.

## Rules
- **Rule 1 (Cart Drawer Standard)**:
  When a shopper clicks "Add to Cart":
  - Open a slide-over cart drawer (Sheet/Drawer) immediately.
  - Display item thumbnail, variant (size/color), unit price, quantity incrementer (`-`, `+`), and total cart subtotal.
  - Provide a prominent primary "Proceed to Checkout" button above the fold in the drawer.
  - Allow continuing shopping by clicking outside the drawer or pressing `Escape`.
- **Rule 2 (Sticky Mobile Add-to-Cart Bar)**:
  On mobile devices (`< 768px`), when the main "Add to Cart" button scrolls out of view on a Product Detail Page (PDP), display a sticky bottom bar with thumbnail, price, variant selector, and "Add to Cart" button.
- **Rule 3 (Guest Checkout Non-Negotiable)**:
  Never force shoppers to create an account or verify a password before placing an order. Provide a frictionless **Guest Checkout** option with an optional "Save info for next time" checkbox post-purchase.
- **Rule 4 (Trust Badges & Security Signals)**:
  Place recognized payment icons (Visa, Mastercard, Apple Pay, PayPal) and SSL/guarantee badges directly adjacent to primary checkout buttons to alleviate payment security anxieties.

## Decision Criteria
```text
IF shopper clicks "Add to Cart":
  Trigger optimistic cart drawer opening; NEVER redirect shopper away to a dedicated full-page cart.
IF total order qualifies for free shipping:
  Display dynamic progress bar in cart drawer: "Add $14.00 more to unlock Free Shipping!".
IF product is out of stock:
  Disable "Add to Cart"; replace with "Notify Me When Available" email capture.
```

## Recommended Workflow
1. Map customer purchase journey: Homepage -> PLP -> PDP -> Cart Drawer -> Checkout -> Confirmation.
2. Implement reactive cart state (optimistic client updates synchronized with server backend).
3. Build mobile-first sticky action bars.
4. Integrate transparent shipping calculators in the cart drawer.
5. Audit against Baymard Institute e-commerce UX benchmarks.

## Best Practices
- Retain cart items in persistent storage (HTTP cookies or database) across browser sessions for at least 30 days.
- Include thumbnail images next to line items throughout cart, checkout, and email receipts.
- Enable Apple Pay and Google Pay express one-click checkout buttons.

## Anti-Patterns
- **The Account Creation Wall**: Forcing shoppers to create a password and verify an email before seeing shipping rates.
- **The Silent Add-to-Cart**: Adding an item to the cart without visual confirmation, leaving the user wondering if the click registered.
- **Hidden Fees**: Revealing a mandatory $15 handling fee only on the final payment confirmation screen.

## Validation Checklist
- [ ] Cart drawer opens instantly upon adding any item.
- [ ] Mobile sticky "Add to Cart" bar appears when primary button scrolls out of view.
- [ ] Guest checkout is prominent and requires no upfront account creation.
- [ ] Free shipping progress indicators are displayed accurately.

## Related Skills
- `skills/ecommerce/product-pages.md`
- `skills/ecommerce/checkout.md`
- `skills/ecommerce/conversion.md`

## References
- Baymard Institute E-Commerce UX Research & Benchmarks — https://baymard.com/
- Vercel Headless Commerce Architecture — https://github.com/vercel/commerce

## Last Reviewed
2026-09-15
