# Introduction

[Multi-Tenant eCommerce](https://bagisto.com/en/laravel-multi-tenant-saas/) in Bagisto lets you run a SaaS platform where many merchants create and manage their own individual stores under a single system. Each merchant — a **tenant** — gets a dedicated storefront and admin panel, while you, the platform owner, manage the infrastructure, subscription plans, and revenue from one place.

::: info What you'll learn
- Who the two audiences of the platform are
- The two types of multi-tenant setup: single database and multiple databases
- What a super admin controls and what a tenant controls
- Where to go next for each task
:::

## Two panels, two audiences

The platform is run from two separate panels.

- The **Super Admin panel** is where you oversee every tenant, subscription plan, and platform-wide setting.
- The **Tenant Admin panel** is a Bagisto-like backend each merchant uses to run their own store.

<ImagePopup src="/images/introduction/multi-tenant.webp" alt="Multi-tenant SaaS platform overview" />

## Two types of multi-tenant setup

This guide compares two SaaS database architectures. Both provide each tenant with its own storefront and admin panel, but differ in how they separate store data.

- **[Single Database](/getting-started/single-database)** — all tenants share one database and are separated by a tenant identifier. A shared schema reduces database administration and can suit merchants with similar service requirements.
- **[Multiple Databases](/getting-started/multi-database)** — each tenant gets its own dedicated database, created when the merchant registers. A separate store database provides a clearer boundary for individual database operations; provisioning, backups, and upgrades need to be managed across tenants.

Both variants provide central platform administration and separate merchant experiences. Database provisioning, reporting internals, and maintenance differ; features and screens should be checked against the installed variant and version.

## Single database vs multiple databases

| Consideration | Single database | Multiple databases |
| --- | --- | --- |
| Store data | Shared tables with tenant ownership rules | Separate operational database per store |
| Platform records | Stored within the shared database design | Central landlord database |
| New store setup | Initializes tenant records and store data | Provisions a database from a prepared template |
| Store isolation | Depends on tenant-scoped access throughout the application | Uses tenant database routing, plus boundaries for files, sessions, cache, and jobs |
| Reporting | Can aggregate shared store tables | Uses selected reporting copies in the landlord |
| Individual recovery | Requires extracting related tenant records | Has a separate database restore boundary; files and platform records still need coordination |
| Schema updates | Apply to the shared store schema | Apply across tenant databases and maintain the template |
| Resource usage | Stores share database capacity | Databases may still share server capacity |

Choose based on the service you need to operate: merchant workloads, recovery expectations, data placement, and the team's ability to maintain the platform. Neither variant guarantees a particular tenant capacity or response time. The detailed pages explain the benefits and operating responsibilities of each model.

## What the super admin does

The super admin is the top authority on the platform. From the Super Admin panel you can:

- **Manage tenants** — register merchants, view their store insights, edit them, and sign in as any tenant. See [Tenant Management](/super-admin/tenant-management).
- **Sell subscriptions** — create plans, track purchased plans, and review invoices. See [Plan Management](/subscriptions/plan-management).
- **Run the platform** — manage agents and roles, currencies, exchange rates, webhooks, and themes. See [Agents & Roles](/settings/agents-and-roles).
- **Publish content** — manage CMS pages and platform-wide configuration. See [CMS Pages](/content/cms-pages).

## What a tenant does

A tenant signs up, gets a storefront and admin panel provisioned automatically, and starts selling. A merchant manages products, categories, orders, and customers, chooses a storefront theme allowed by their plan, and reviews their own subscription and invoices. See [Registration & Sign-in](/tenants/registration).

::: tip
New to the platform? If you run the SaaS, start with the [Super Admin Dashboard](/super-admin/dashboard). If you're a merchant setting up a store, start with [Registration & Sign-in](/tenants/registration).
:::
