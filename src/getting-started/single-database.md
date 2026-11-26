---
title: Single Database SaaS Architecture
description: Understand shared-database multitenancy in Bagisto SaaS, including tenant isolation, operating costs, backups, scaling, and when to choose it.
---

# Single Database

A **single database** SaaS architecture stores multiple merchants' data in shared tables. Each merchant, or **tenant**, has its own storefront and admin experience, while tenant identifiers and application access rules separate the store records.

This approach suits a platform offering a consistent service to many merchants with similar requirements. Its main benefit is operational simplicity: the platform team maintains one shared store schema instead of a separate database for every merchant.

::: info What you'll learn
- How shared tables keep each store's data separate
- What this model means for cost, performance, reporting, and recovery
- How to decide between a shared database and a database per tenant
:::

## How the architecture works

<svg viewBox="0 0 688 372" role="img" aria-label="Single shared database for all tenants" style="width:100%;height:auto;font-family:var(--vp-font-family-base)">
<title>Single shared database for all tenants</title><defs><marker id="arrow-single" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" style="fill:var(--vp-c-text-3)"/></marker></defs>
<rect x="16" y="16" width="152" height="48" rx="8" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-divider);stroke-width:1"/>
<text x="92" y="45" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Super admin panel</text>
<rect x="184" y="16" width="152" height="48" rx="8" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-divider);stroke-width:1"/>
<text x="260" y="45" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant A store</text>
<rect x="352" y="16" width="152" height="48" rx="8" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-divider);stroke-width:1"/>
<text x="428" y="45" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant B store</text>
<rect x="520" y="16" width="152" height="48" rx="8" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-divider);stroke-width:1"/>
<text x="596" y="45" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant C store</text>
<line x1="92" y1="64" x2="92" y2="100" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-single)"/>
<line x1="260" y1="64" x2="260" y2="100" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-single)"/>
<line x1="428" y1="64" x2="428" y2="100" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-single)"/>
<line x1="596" y1="64" x2="596" y2="100" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-single)"/>
<rect x="16" y="104" width="656" height="56" rx="8" style="fill:var(--vp-c-brand-soft);stroke:var(--vp-c-brand-1);stroke-width:1"/>
<text x="344" y="128" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:13px;font-weight:600">Bagisto SaaS platform</text>
<text x="344" y="147" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">resolves the store domain and scopes access to tenant-owned data</text>
<line x1="344" y1="160" x2="344" y2="194" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-single)"/>
<path d="M 16 214 L 16 344 A 328.0 16 0 0 0 672 344 L 672 214" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/><ellipse cx="344.0" cy="214" rx="328.0" ry="16" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/>
<text x="344" y="248" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:13px;font-weight:600">One shared database</text>
<rect x="26" y="266" width="132" height="26" rx="13" style="fill:var(--vp-c-brand-soft);stroke:none"/>
<text x="92" y="283" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">Platform data</text>
<rect x="194" y="266" width="132" height="26" rx="13" style="fill:var(--vp-c-brand-soft);stroke:none"/>
<text x="260" y="283" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">Tenant A rows</text>
<rect x="362" y="266" width="132" height="26" rx="13" style="fill:var(--vp-c-brand-soft);stroke:none"/>
<text x="428" y="283" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">Tenant B rows</text>
<rect x="530" y="266" width="132" height="26" rx="13" style="fill:var(--vp-c-brand-soft);stroke:none"/>
<text x="596" y="283" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">Tenant C rows</text>
<text x="344" y="320" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">shared store tables — tenant-owned data is scoped to its tenant</text>
</svg>

The domain identifies the store a visitor is accessing. The application then uses that tenant context to read and write the appropriate store data. Platform-level records, such as tenant accounts and subscriptions, support central administration.

For example, two merchants can both sell a product called “Classic T-shirt.” Each product belongs to its own tenant, and a customer browsing one merchant's store should only see that merchant's catalog, prices, and availability.

A typical request follows this sequence:

1. A customer opens a merchant's store domain.
2. The application resolves the domain to a tenant.
3. Store queries apply the tenant's ownership rules.
4. New records are associated with the correct tenant.
5. The storefront or admin panel displays the permitted results.

The exact ownership columns and relationships depend on the installed module version. Tenant-specific records need an ownership boundary; shared reference data does not necessarily need a tenant identifier on every row.

## What data isolation means

Sharing a database does not mean merchants share access to one another's records. Isolation depends on consistently enforcing tenant context throughout the application, including APIs, exports, background jobs, and extensions.

For example, looking up an order by its ID alone is insufficient if that order might belong to another tenant. The application must also check ownership and the user's permissions. Custom integrations need to preserve these rules when they bypass standard repositories or run direct queries.

Files, cache entries, sessions, and search results also need tenant boundaries. Database filtering alone does not separate these resources.

## Benefits for the platform owner

### A simpler database estate

A shared store schema gives the team fewer database objects and connections to manage. Schema changes run against the shared database, and infrastructure monitoring has fewer individual databases to track. This can reduce routine administration, particularly when merchants use the same features.

### Efficient use of shared capacity

Smaller stores can share available database capacity instead of each requiring a separate database deployment. Actual operating cost still depends on traffic, storage, availability, and support requirements; tenant count alone is not a reliable sizing measure.

### Straightforward tenant onboarding

Creating a tenant does not require creating a separate database. Registration still needs to initialize the store's account, settings, and other required data, so readiness depends on the onboarding workflow and services involved.

### Central reporting

Shared tables make it easier to aggregate store activity for authorized platform reports. Merchant-facing reports must remain scoped to the current tenant. See [Tenant Data](/super-admin/tenant-data) for the reporting views in this guide.

## Operating a shared database

### Performance and growth

A busy store can compete with others for database connections, CPU, memory, and disk throughput. Common pressure points include bulk catalog imports, large reports, and concurrent checkout activity. This is often called the **noisy neighbor** problem.

Plan capacity around representative workloads and monitor slow queries, connection usage, and peak demand. Indexes should support tenant-scoped access patterns. Moving to larger infrastructure or distributing tenants across databases is an architectural decision, not an automatic consequence of adding more merchants. Microsoft's [multitenant storage guidance](https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/approaches/storage-data) discusses shared-resource contention and capacity planning.

### Backups and individual store recovery

A full database backup contains data for multiple stores. Recovering one store requires identifying its related records and restoring them consistently, including dependencies such as order items and customer addresses.

A production recovery procedure should restore a backup into a separate environment before extracting the required tenant data. Restoring the entire shared database over production would also roll back other tenants. Store files and external payment activity need to be reconciled separately.

### Upgrades and extensions

One shared schema reduces the number of migration targets, but an incompatible migration can affect every store using that schema. Test releases with realistic data and plan how to recover from a failed deployment.

Extensions must support tenant ownership rules. Installing an extension designed for a standalone store does not by itself make its queries, jobs, or exports safe for a shared database.

## When to choose single database

This model is a practical starting point when merchants accept shared infrastructure, use a common feature set, and have similar service requirements. It also helps teams that want to keep database administration manageable while developing their SaaS offering.

Consider a platform serving independent boutiques with modest catalogs and broadly similar traffic. Shared tables can be a reasonable fit if the platform can meet the merchants' performance and recovery expectations.

If individual store recovery or a separate database boundary is a core requirement, evaluate [Multiple Databases](/getting-started/multi-database). The decision should reflect the service you promise and your ability to operate it, alongside cost. See Microsoft's [tenancy model considerations](https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/considerations/tenancy-models).

## Frequently asked questions

### Can a shared database support a large platform?

Yes, but there is no universal store limit. Catalog size, order volume, query design, hardware, and peak concurrency determine practical capacity. Validate the expected workload before committing to a client service level.

### Does every tenant need the same storefront?

No. Sharing a database is a storage decision. Storefront branding, domains, and the features available through subscription plans are separate concerns.

### Can we move to multiple databases later?

Treat this as a migration project. It requires extracting each tenant's records, preserving relationships, moving files, configuring routing, and validating integrations. Do not assume the two variants can be switched with a setting.

For a side-by-side comparison, return to the [architecture overview](/getting-started/introduction#single-database-vs-multiple-databases).
