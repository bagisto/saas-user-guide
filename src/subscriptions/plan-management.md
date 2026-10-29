# Plan Management

Subscription plans are how the platform earns from its tenants. As super admin you build the plans, set what each one allows, and price them.

::: info What you'll learn
- How to view your subscription plans
- How to create a plan and set its limits, price, themes, and offers
- What happens when you edit, deactivate, or delete a plan
:::

## Viewing plans

Go to **Subscription → Plans** to see every plan, with its code, name, monthly and yearly amount, and status.

<ImagePopup src="/images/plan-management/super-plans.png" alt="Subscription plans list" />

## Creating a plan

Click **Create Plan** and fill in the form.

<ImagePopup src="/images/plan-management/super-create-plan.png" alt="Add Plan form" />

**General**

- **Code** — a unique identifier of at least two characters. Use only letters, numbers, dashes, and underscores, and start with a letter or number.
- **Name** — the plan name a tenant sees, such as *Starter* or *Growth*.
- **Description** — a short summary of what the plan offers. Required.

**Billing Amount**

- **Monthly Amount** — the price charged each month on monthly billing. Set **0** for a free plan.
- **Yearly Amount (Month by Month)** — the price charged once a year on yearly billing. Despite the label, enter the full price for the year, not a per-month figure. Set **0** to offer the plan on monthly billing only.

**Plan Limitation**

Set the ceilings a tenant on this plan cannot exceed: **Allowed Products**, **Allowed Categories**, **Allowed Attributes**, **Allowed Attribute Families**, **Allowed Channels**, and **Allowed Orders**. Each must be at least 1. See [what counts towards a limit](/subscriptions/plan-changes#plan-limits).

**Theme Assignment**

Set **Premium Themes** to **Included** or **Not included**. When included, tick at least one design under **Select Themes** — **Breeze**, **Fashion**, **Commercia**, or **Adornments**. A tenant on the plan can use the Default theme plus the ticked designs. See [Storefront Themes](/tenants/storefront-themes).

**Offers**

Add an optional discount shown against the plan:

- **Status** — **Active** or **Inactive**.
- **Title** — the offer's name, shown to tenants.
- **Type** — **Percentage** or **Fixed**.
- **Discount** — the amount off, at least 1. A percentage cannot exceed 100.

An active offer applies to both the monthly and yearly price. Tenants see the discounted price with the list price struck through.

**Module Assignment**

This section appears only when **Extension Assignment Setting → Status** is turned on under [Configuration → Subscription](/content/configuration#payment-settings). Use it to choose the extensions (modules) a plan includes.

Select **Save Plan** to publish it. New plans are active straight away.

## Editing a plan

Click the **Edit** icon on a plan to change it. The edit form adds a **Status** field: set a plan to **Inactive** to stop offering it. An inactive plan disappears from the tenant **Plans** page and is never offered as a plan change, but tenants already on it are not affected.

When you save a plan, every tenant on it is emailed *Your Plan Has Been Changed*. Their limits do not change immediately, though — each subscription keeps the limits it was bought with. Your edits reach a tenant the next time they buy or change a plan.

## Deleting a plan

Use the **Delete** icon on a plan, or select several plans and delete them together.

::: warning
Deleting a plan cannot be undone, and the platform does not stop you deleting a plan that tenants are on. Their subscriptions are not cancelled — they keep the limits they bought — but the plan itself is gone. To stop selling a plan, set its **Status** to **Inactive** instead.
:::

::: tip
Keep plan limits meaningful — they drive the usage bars a tenant sees on their [billing overview](/subscriptions/tenant-billing), and hitting a limit is what prompts them to upgrade.
:::
