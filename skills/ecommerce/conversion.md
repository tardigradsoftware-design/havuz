# Conversion Rate Optimization (CRO) & Persuasive Design

## Purpose
Applies ethical persuasive design, behavioral economics principles, and conversion optimization strategies to maximize visitor-to-customer conversion rates across all web funnels.

## When To Use
- When optimizing landing pages, product pages, pricing tiers, and checkout funnels.
- When designing Call-to-Action (CTA) buttons, social proof displays, and onboarding steps.

## When Not To Use
- When designing internal administrative consoles or tools where persuasion is irrelevant.

## Core Principles
1. **Value Before Friction**: Demonstrate tangible value and proof before asking for user commitment or sensitive payment details.
2. **Cognitive Ease**: Minimize the mental effort required to make a decision. Present fewer, clearer choices (Hick's Law).
3. **Ethical Persuasion Over Dark Patterns**: Never use deceptive countdown timers, fake low-stock alerts, or hidden recurring subscriptions. True conversion optimization builds lasting customer trust.

## Rules
- **Rule 1 (The Single Primary Action Principle)**:
  Every conversion screen must have exactly one dominant primary action. Secondary actions must be visually subdued:
  - Primary CTA: High contrast, solid fill, prominent scale (`w-full py-4 text-base font-semibold bg-primary text-primary-foreground`).
  - Secondary Action: Ghost or outline style (`variant="ghost" text-muted-foreground`).
- **Rule 2 (Frictionless Social Proof Integration)**:
  Place credible social proof immediately adjacent to decision points:
  - Under hero CTA: "Trusted by 40,000+ engineers at teams like..." (Client logos).
  - Above product buy button: "★ ★ ★ ★ ★ 4.9/5 from 820 verified reviews".
  - Near payment form: Security seals and money-back guarantee badges.
- **Rule 3 (The Default Effect & Recommended Tiers)**:
  When displaying pricing plans or package options:
  - Highlight the single recommended plan visually (elevated card, "Most Popular" badge, subtle border ring).
  - Pre-select the recommended plan or billing interval (e.g. Annual with "2 Months Free" discount badge).
- **Rule 4 (Loss Aversion & Risk Reversal)**:
  Prominently communicate risk reversal guarantees near the final action:
  - "30-day money-back guarantee, no questions asked."
  - "Cancel anytime in 1 click from your settings."
  - "Free returns & exchanges within 14 days."

## Decision Criteria
```text
IF user hovers over CTA button:
  Apply subtle scale and brightness shift (e.g. hover:brightness-105 active:scale-[0.98]).
IF page has 3 pricing tiers:
  Highlight the middle tier with variant="popular" border-primary shadow-lg.
IF form has > 4 fields:
  Break into multi-step wizard with visual progress bar; reducing perceived friction.
```

## Recommended Workflow
1. Identify funnel drop-off points using web analytics or session recordings.
2. Formulate hypothesis (e.g., "Adding a 30-day refund guarantee badge under checkout button will increase conversions").
3. Implement clean, high-contrast visual hierarchy with clear value propositions.
4. Add authentic social proof and testimonials.
5. Conduct A/B testing using split testing tools (LaunchDarkly, PostHog, GrowthBook).
6. Measure statistical significance before rolling out change globally.

## Best Practices
- Use active, value-oriented copy on buttons: "Get Instant Access" or "Start Free 14-Day Trial", not generic "Submit".
- Include real customer quotes with verifiable names, avatars, and company titles.
- Keep microcopy helpful: "No credit card required" under free trial buttons.

## Anti-Patterns
- **Dark Patterns**: Countdown timers that reset on page reload, or pre-checking hidden subscription upsells.
- **Vague Value Propositions**: "We provide revolutionary synergy for modern enterprises" (User has no idea what you do).
- **The 14-Field Lead Form**: Asking for company fax number, employee count, and budget before letting someone test the product.

## Validation Checklist
- [ ] Every page has a distinct, unmistakable primary Call-to-Action.
- [ ] Social proof is colocated next to primary decision points.
- [ ] Risk reversal guarantees (refunds, cancellations) are clearly stated.
- [ ] Zero deceptive dark patterns exist.

## Related Skills
- `skills/ecommerce/ecommerce-ux.md`
- `skills/ui/visual-hierarchy.md`
- `skills/saas/dashboard-ux.md`

## References
- Influence: The Psychology of Persuasion — Robert Cialdini
- Baymard Institute Conversion Rate Optimization Principles

## Last Reviewed
2026-09-15
