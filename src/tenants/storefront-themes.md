# Storefront Themes

A tenant's storefront can wear a different look depending on their subscription. Every store starts on the **Default** theme, and plans can unlock **premium themes** — ready-made designs a merchant can switch to without any code.

::: info What you'll learn
- Which premium designs exist and how plans unlock them
- How a merchant activates a theme on a channel
- How a merchant customizes a theme with the sections editor
- What happens to themes when a plan is downgraded or cancelled
:::

The premium designs are **Breeze**, **Fashion**, **Commercia**, and **Adornments**.

## Which themes a tenant can use

Premium themes are a plan allowance. On each subscription plan the super admin sets **Premium Themes** to **Included** or **Not included**, and when included, ticks the specific themes the plan unlocks under **Select Themes** (see [Plan Management](/subscriptions/plan-management)). A plan can therefore unlock all four premium designs or only some of them.

A tenant can compare plans on the **Billing → Plans** screen, where each plan lists its **Premium Themes** allowance as the number of designs it includes, or **Not included**.

- On a plan **without** premium themes, only the **Default** theme is available.
- On a plan **with** premium themes, the Default theme plus every premium design that plan includes are available.

Themes the current plan does not include are not shown to the tenant at all.

## Activating a theme

In the Tenant Admin panel, go to **Appearance → Themes**. The **Themes** page lists the themes available for the storefront under **My Themes**, and each card shows where it is in use — **Active on:** followed by the channels, **Active on every channel**, or **Not in use**.

<ImagePopup src="/images/storefront-themes/tenant-theme-channel.png" alt="Appearance themes gallery" />

To switch a channel to a theme:

1. Select **Activate** on the theme's card.
2. In the **Activate Theme** dialog, choose the **Channels** that should start using the theme. Hold **Ctrl** (**Cmd** on Mac) to pick more than one channel.
3. Select **Activate**. The storefront for those channels switches straight away.

If a channel already has customizations built for its current theme, the dialog warns you before switching. Those customizations stay saved — they are only hidden while the other theme is active, and reappear when you activate the original theme again.

::: info
The theme on an existing channel is managed from **Appearance → Themes**, not from the channel form. **Settings → Channels** shows the channel's current theme with a link to the Themes page; a theme dropdown appears only when creating a new channel.
:::

## Customizing a theme

Only a theme that is active on a channel can be customized. On its card, select **Customize** to open the **sections editor** for that theme, channel, and locale.

<ImagePopup src="/images/storefront-themes/tenant-theme-customization.png" alt="Theme sections editor" />

- Use the **Channel** and **Locale** switchers at the top to choose which storefront you are editing.
- Select **Create Section** to add a block — **Image Carousel**, **Category Carousel**, **Product Carousel**, **Static Content**, **Footer Links**, or **Services Content**.
- **Duplicate** a section to start from an existing one, drag sections to reorder them, and use the status toggle to show or hide a section.
- The **Preview** shows the storefront live, with **Desktop**, **Tablet**, and **Mobile** views.

Edits are kept as a draft — the editor shows **Unsaved changes** — until you select **Publish**. Select **Discard** to throw the draft away and return to the published version.

## Ready-made content

Every new store is provisioned with ready-made sections for the Default theme and for each premium theme, so a premium design looks complete the moment the plan unlocks it — there is no need to build it from scratch. Activating **Adornments** also comes with its own set of storefront CMS pages.

## Theme examples

The same store looks completely different depending on the theme applied. The **Commercia** theme suits a grocery or everyday-essentials store:

<ImagePopup src="/images/storefront-themes/commercia-storefront.png" alt="Commercia storefront theme" />

While the **Fashion** theme gives a clean, editorial look built for apparel:

<ImagePopup src="/images/storefront-themes/theme-fashion.png" alt="Fashion storefront theme" />

::: warning Downgrades and cancellations
If a tenant moves to a plan that no longer includes the premium theme a channel is using — or their subscription is cancelled — that channel is switched back to the **Default** theme automatically, and the merchant is emailed a list of the affected channels. Channels on themes the new plan still includes are left alone.

Theme customizations are kept. After upgrading to a plan that includes the theme again, the merchant re-activates it from **Appearance → Themes**, and the saved design comes back.
:::

::: tip
Match the theme to the catalog — Commercia for everyday goods, Fashion for apparel. Switching keeps every theme's customizations, so a merchant can try a design and change their mind without losing work.
:::
