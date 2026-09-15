# Frictionless Checkout Funnels & Payment Architecture

## Purpose
Engineers rock-solid, frictionless, and secure checkout funnels that maximize completion rates while strictly conforming to PCI-DSS payment security standards.

## When To Use
- When developing checkout workflows, order confirmation screens, and payment gateway integrations (Stripe, Adyen).
- When configuring webhook processors, inventory reservation locks, and fraud prevention controls.

## When Not To Use
- For free digital downloads or lead generation forms that do not collect monetary transactions.

## Core Principles
1. **Zero Distraction Checkout**: Strip away global navigation, headers, footers, and marketing banners during the checkout flow to keep the shopper 100% focused on completing the transaction.
2. **PCI-DSS Compliance via Hosted Fields**: Never handle, process, or transmit raw credit card numbers through your application servers. Use Stripe Elements / Hosted Fields to collect payment data securely in browser iframes.
3. **Idempotent Webhook Processing**: Fulfill orders based on signed, verified server-to-server webhooks—never rely solely on client-side redirect callbacks to confirm payments.

## Rules
- **Rule 1 (The 3-Step Focused Checkout Architecture)**:
  Structure checkout into 3 logical, linear stages (Single-page accordion or clean steps):
  1. *Customer Information & Shipping Address* (with address autocomplete via Google Places or Radar API).
  2. *Shipping Method Selection* (clearly displaying delivery times and prices).
  3. *Payment Details & Billing Address* (Stripe Payment Element with express buttons).
- **Rule 2 (Address Auto-Complete & Inline Validation)**:
  - Provide street address autocomplete to reduce typing effort and eliminate typographical delivery errors.
  - Automatically infer City and State when the user enters a valid ZIP/Postal Code.
- **Rule 3 (Stripe Payment Element Integration)**:
  Integrate payment methods using official modern SDKs:
  ```tsx
  import { Elements, PaymentElement, useStripe, useElements } from '@stripe/react-stripe-js';

  export function CheckoutPaymentForm() {
    const stripe = useStripe();
    const elements = useElements();
    const [isProcessing, setIsProcessing] = useState(false);

    const handleSubmit = async (e: React.FormEvent) => {
      e.preventDefault();
      if (!stripe || !elements) return;

      setIsProcessing(true);
      const { error } = await stripe.confirmPayment({
        elements,
        confirmParams: {
          return_url: `${window.location.origin}/checkout/confirmation`,
        },
      });

      if (error) setIsProcessing(false);
    };

    return (
      <form onSubmit={handleSubmit} className="space-y-6">
        <PaymentElement />
        <button type="submit" disabled={!stripe || isProcessing} className="w-full py-3 bg-primary text-primary-foreground font-semibold rounded-lg">
          {isProcessing ? 'Processing Payment...' : 'Complete Purchase'}
        </button>
      </form>
    );
  }
  ```
- **Rule 4 (Server-Side Webhook Verification)**:
  Confirm and provision orders strictly inside the `payment_intent.succeeded` webhook handler:
  ```typescript
  import { stripe } from '@/lib/stripe';

  export async function POST(req: Request) {
    const body = await req.text();
    const sig = req.headers.get('stripe-signature')!;
    
    let event;
    try {
      event = stripe.webhooks.constructEvent(body, sig, process.env.STRIPE_WEBHOOK_SECRET!);
    } catch (err) {
      return new Response(`Webhook Error: ${err}`, { status: 400 });
    }

    if (event.type === 'payment_intent.succeeded') {
      const paymentIntent = event.data.object;
      await OrderService.fulfillOrder(paymentIntent.metadata.orderId);
    }

    return new Response(JSON.stringify({ received: true }), { status: 200 });
  }
  ```

## Decision Criteria
```text
IF shopper supports Apple Pay or Google Pay:
  Display Express Checkout Button prominently at top of checkout flow.
IF order submission is clicked:
  Disable button, show spinner, and lock input fields to prevent double charges.
IF payment is rejected by bank:
  Display clear inline error ("Card declined. Please try another card.") and retain all entered form data.
```

## Recommended Workflow
1. Initialize PaymentIntent on server with calculated order total and metadata.
2. Render enclosed checkout layout without external distracting links.
3. Mount Stripe Elements with brand styling tokens.
4. Confirm payment via Stripe SDK.
5. Fulfill order via secure webhook handler.
6. Display order confirmation page with order number, summary, and receipt download.

## Best Practices
- Display persistent order summary on desktop (right column) showing all items, quantities, taxes, and shipping fees.
- Pre-fill customer address if user is logged in.
- Enable automatic currency conversion for international cross-border shoppers.

## Anti-Patterns
- **Handling Raw Card Numbers**: Storing `cardNumber` or `cvv` in database or server memory (Severe PCI violation).
- **Fulfilling Orders on Client Redirect**: Trusting the `return_url` redirect to ship goods without verifying server webhook.
- **Clearing Form on Error**: Forcing the user to re-type their entire shipping address because their credit card was declined.

## Validation Checklist
- [ ] Checkout layout removes header/footer navigation distractions.
- [ ] Card collection uses Stripe Elements or equivalent iframe SDK.
- [ ] Webhook handler verifies cryptographic signatures.
- [ ] Express payment options (Apple Pay / Google Pay) are supported.

## Related Skills
- `skills/ecommerce/ecommerce-ux.md`
- `skills/security/api-security.md`
- `skills/backend/error-handling.md`

## References
- Stripe Official Checkout Documentation — https://stripe.com/docs/payments/accept-a-payment
- Baymard Institute: 18 Cart Abandonment Reasons & Checkout UX

## Last Reviewed
2026-09-15
