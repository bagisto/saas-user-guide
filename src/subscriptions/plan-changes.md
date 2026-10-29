# Plan Changes, Trials & Renewals

This page covers what happens to a subscription after it starts — free trials, upgrading and downgrading, plan limits, failed payments, and what a store sees when its subscription stops.

::: info What you'll learn
- How the free trial works
- How a tenant upgrades or downgrades a paid plan
- What counts towards a plan limit
- What happens when a payment fails or a subscription ends
:::

## Free trial

The super admin turns the trial on under **Configure → Subscription → Payment Settings → Trial Plan**: **Allow Trial**, the number of **Trial Days**, and which **Trial Plan** the trial runs on. See [Configuration](/content/configuration#payment-settings).

- A new store starts its trial automatically at registration — unless the merchant picked a plan on the landing page.
- A tenant can also start it from **Billing → Plans** by choosing the trial plan. It activates straight away, with no checkout.
- Each store gets one trial. Afterwards the trial plan shows *Your trial has already been used.*
- The trial plan is never offered as a plan change.

When the trial ends, the admin panel shows **Your trial has expired** with a **Choose Plan** button. There is no grace period after a trial.

## Changing a paid plan

When a store already pays for a plan through Stripe, PayPal, or Manual billing, it changes plan from **Billing → Plans** using the **Upgrade** or **Downgrade** button on another plan. A plan with a higher monthly price is an upgrade; a lower one is a downgrade. Checkout and **Choose** are not used for this.

### Upgrading now

On **Stripe**, the button reads **Upgrade now for** followed by an amount. That amount is a one-time charge for **only the days left in the current billing period**, worked out in two parts:

1. **The new plan for the days left** — the new plan's price × days left ÷ days in the period.
2. **Less the unused days of the current plan** — the current plan's price × days left ÷ days in the period. Only the unused part is credited, never the whole amount already paid, because the tenant has used the days before the upgrade.

The note under the button shows both figures. Time is measured to the second from the billing period Stripe reports, not in whole days.

> **Example:** a tenant on a $20/month plan upgrades to a $50/month plan on day 20 of a 30-day period, with 10 days left.
>
> - New plan for the 10 days left: $50 × 10 ÷ 30 = **$16.67**
> - Less the 10 unused days of the current plan: $20 × 10 ÷ 30 = **$6.67**
> - Charged now: $16.67 − $6.67 = **$10.00**
>
> Over the whole period the tenant pays $20 + $10 = $30 — exactly 20 days at $20/month plus 10 days at $50/month. On the next billing date they are billed the full $50.

The billing date and cycle stay the same, and later bills are at the new plan's full price. The tenant pays through Stripe and sees *Your plan has been upgraded successfully.* If the upgrade cannot be applied after payment, the payment is refunded. Once the billing period has ended, no upgrade is offered until Stripe starts the next one.

On **Manual** billing, the same button applies the upgrade immediately, with an invoice for the full price difference — Manual upgrades are not prorated.

### Changing at the next renewal

Downgrades on every payment method, and upgrades on **PayPal**, take effect at the next renewal. The button reads **Downgrade on** or **Upgrade on** followed by the date, with a note on what the store keeps until then and what it pays afterwards. After confirming, the tenant sees *Your plan changes to … on …*.

Until the date arrives:

- **Billing → Plans** shows a banner with the coming change and a **Keep current plan** button that cancels it.
- The new plan's card shows **Starts on** and the date.
- **Billing → Overview** shows the **Scheduled Change**.

On PayPal, a higher price may need the tenant to approve a new agreement on PayPal before the change is booked.

### Plans that can't be switched to

A plan the store's usage already exceeds shows *Your current usage exceeds this plan.* Inactive plans and the trial plan are never offered as a change, and some combinations show *This plan can't be switched to from your current subscription.*

::: info
Scheduled changes are applied when the subscription renews. A daily `saas:subscription:plan-changes` task also applies any change whose renewal notice from Stripe or PayPal never arrived, so the server's scheduler must be running.
:::

## Plan limits

Each plan caps products, categories, attributes, attribute families, channels, and orders. Once a cap is reached, creating another — or placing another order, or importing more products — is refused with a message saying how many remain and suggesting an upgrade.

What counts:

- **Products** — every product, including each variant of a configurable product.
- **Categories** — every category except the root.
- **Attributes** and **Attribute families** — only the ones the merchant created, not the built-in ones.
- **Channels** — every channel except the default one.
- **Orders** — orders placed in the month up to the end of the current billing cycle.

## Failed payments

When Stripe or PayPal reports a failed payment, the tenant's admin panel shows *We could not process your last payment* with an **Update Payment Method** link to the billing overview. The tenant is emailed *Action needed: your subscription payment failed*, and the super admin receives an alert too. After repeated failed PayPal payments, the subscription is suspended.

## When a subscription stops

A subscription stops when it expires, is suspended, or is cancelled by the super admin. Paid Stripe and PayPal subscriptions get one extra day after the billing period ends (five days after a skipped PayPal payment) before the store stops. Trials and Manual plans stop at the end of the period.

Once stopped, every page of the tenant's admin panel redirects to **Billing → Overview**, with a notice such as **Billing payment due**, **Your subscription has been suspended**, **Your subscription has stopped**, or **Purchase plan to proceed**. The **Billing** pages, **Profile**, **Billing Address**, and **My Account** stay open, so the merchant can pay or choose a new plan. Any premium storefront theme falls back to the Default theme on cancellation.

On the super admin's [tenants list](/super-admin/tenant-management#viewing-tenants), such a store's **Service** shows **Paywalled**.

::: warning
Tenants cannot cancel their own subscription from the panel. Cancellation is done by the super admin with **Cancel Plan** on the tenant's [insights page](/super-admin/tenant-management#the-tenant-s-plan).
:::

## Lifecycle emails

The platform emails tenants about their subscription automatically — trial ending, renewal coming up, subscription expired, and overdue Manual invoices — and sends the super admin a digest of expired subscriptions. Each can be switched on or off under [Configuration → Notifications](/content/configuration#notifications).

Tenants are also emailed when the super admin assigns, cancels, or changes their plan, and when a plan change removes a premium theme they were using.
