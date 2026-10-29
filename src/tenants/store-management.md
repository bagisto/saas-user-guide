# Store & Admin Panel

After registration the store is live and the merchant has a full Bagisto backend to run it. This page covers the storefront and the admin panel a tenant works in day to day.

::: info What you'll learn
- What the provisioned storefront looks like
- How the tenant admin panel and its setup checklist work
- How to find anything quickly with the command palette
- Where day-to-day store operations are documented
:::

## The storefront

Once registration finishes, the store is live and ready for customers to browse and buy. It starts on the **Default** theme with a channel named **Default Channel**, already filled with ready-made homepage sections.

<ImagePopup src="/images/store-management/tenant-store.png" alt="Tenant storefront" />

## The admin panel

Signing in opens the Tenant Admin dashboard. Above the store stats and sales, a **Store Setup** checklist walks a new merchant from an empty store to one that is ready to sell.

<ImagePopup src="/images/store-management/tenant-dashboard.png" alt="Tenant admin dashboard with store setup checklist" />

### The Store Setup checklist

The checklist has seven steps, each with a button that opens the right screen:

1. **Add your first product** — **Add Product**
2. **Group your products into a category** — **Add Category**
3. **Name your store and upload a logo** — **Edit Store**
4. **Choose a theme for your storefront** — **Choose Theme**
5. **Turn on a payment method** — **Set Up Payments**
6. **Turn on a shipping method** — **Set Up Shipping**
7. **Receive your first order** — **View Orders**

A progress bar shows how many steps are complete. Finished steps are marked **Done**, and the next step to tackle is highlighted. Steps tick themselves off as the merchant works — for example, the store step completes once a logo is uploaded or the channel is renamed from *Default Channel*, and the theme step once any channel uses a theme other than Default. Progress can take a few minutes to refresh.

When every step is done, the checklist reads **Your Store Is Ready**; select **Done, Hide This** to put it away. A merchant can also select **Hide Checklist** at any time to dismiss it for good.

## Finding things fast

Press **Ctrl K** (**Cmd K** on Mac), or click the search box in the header, to open the **command palette**. Type to search menus, settings, pages, and actions, and jump straight to a product, order, category, or customer in the store. Results only ever come from the merchant's own store.

## Running the store

The Tenant Admin panel is a standard Bagisto backend, so day-to-day store operations work exactly as in any Bagisto store:

- **Catalog** — create and manage products, categories, attributes, and attribute families under **Catalog**.
- **Sales** — handle orders, invoices, and shipments under **Sales**.
- **Customers** — manage customers and customer groups under **Customers**.
- **Appearance** — choose and customize the storefront theme. See [Storefront Themes](/tenants/storefront-themes).
- **Marketing, CMS, Reporting** — promotions, storefront pages, and reports under their own menus.

For step-by-step guidance on these, see the main [Bagisto User Guide](https://docs.bagisto.com/).

::: tip
The **Billing** menu is unique to the SaaS. It holds **Overview**, **Plans**, **Invoices**, and **Billing Address** — everything about the store's subscription, usage, and invoices. See [Tenant Billing](/subscriptions/tenant-billing).
:::
