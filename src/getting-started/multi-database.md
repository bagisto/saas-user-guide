---
title: Multiple Database SaaS Architecture
description: Explore Bagisto database-per-tenant SaaS architecture, with landlord data, template provisioning, tenant routing, reporting, backups, and upgrade considerations.
---

# Multiple Databases

The **multiple databases** variant gives each merchant a separate database for its operational store data. One Bagisto application serves the stores, while a central **landlord database** manages the platform's tenants, subscriptions, and administrative records.

This model creates a clear database boundary between stores and makes individual store data easier to manage as a unit. The platform still shares application services, and separate databases can run on the same database server.

::: info What you'll learn
- Where platform records and store records live
- How Bagisto provisions a database and routes requests to a tenant
- How reporting, files, jobs, and sessions work across tenants
- What the platform team must plan for recovery, upgrades, and growth
:::

## How the architecture works

<svg viewBox="0 0 688 344" role="img" aria-label="A separate database for each tenant" style="width:100%;height:auto;font-family:var(--vp-font-family-base)">
<title>A separate database for each tenant</title><defs><marker id="arrow-multi" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" style="fill:var(--vp-c-text-3)"/></marker></defs>
<rect x="16" y="16" width="152" height="48" rx="8" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-divider);stroke-width:1"/>
<text x="92" y="45" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Super admin panel</text>
<rect x="184" y="16" width="152" height="48" rx="8" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-divider);stroke-width:1"/>
<text x="260" y="45" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant A store</text>
<rect x="352" y="16" width="152" height="48" rx="8" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-divider);stroke-width:1"/>
<text x="428" y="45" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant B store</text>
<rect x="520" y="16" width="152" height="48" rx="8" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-divider);stroke-width:1"/>
<text x="596" y="45" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant C store</text>
<line x1="92" y1="64" x2="92" y2="100" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-multi)"/>
<line x1="260" y1="64" x2="260" y2="100" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-multi)"/>
<line x1="428" y1="64" x2="428" y2="100" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-multi)"/>
<line x1="596" y1="64" x2="596" y2="100" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-multi)"/>
<rect x="16" y="104" width="656" height="56" rx="8" style="fill:var(--vp-c-brand-soft);stroke:var(--vp-c-brand-1);stroke-width:1"/>
<text x="344" y="128" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:13px;font-weight:600">Bagisto SaaS platform</text>
<text x="344" y="147" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">detects the tenant from the domain and switches the database connection</text>
<line x1="92" y1="160" x2="92" y2="194" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-multi)"/>
<line x1="260" y1="160" x2="260" y2="194" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-multi)"/>
<line x1="428" y1="160" x2="428" y2="194" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-multi)"/>
<line x1="596" y1="160" x2="596" y2="194" style="stroke:var(--vp-c-text-3);stroke-width:1.2" marker-end="url(#arrow-multi)"/>
<path d="M 16 214 L 16 312 A 76.0 13 0 0 0 168 312 L 168 214" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/><ellipse cx="92.0" cy="214" rx="76.0" ry="13" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/>
<text x="92" y="248" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Central database</text>
<text x="92" y="268" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">tenants · plans</text>
<text x="92" y="284" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">billing · reports</text>
<path d="M 184 214 L 184 312 A 76.0 13 0 0 0 336 312 L 336 214" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/><ellipse cx="260.0" cy="214" rx="76.0" ry="13" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/>
<text x="260" y="248" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant A database</text>
<text x="260" y="268" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">products · orders</text>
<text x="260" y="284" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">customers</text>
<path d="M 352 214 L 352 312 A 76.0 13 0 0 0 504 312 L 504 214" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/><ellipse cx="428.0" cy="214" rx="76.0" ry="13" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/>
<text x="428" y="248" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant B database</text>
<text x="428" y="268" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">products · orders</text>
<text x="428" y="284" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">customers</text>
<path d="M 520 214 L 520 312 A 76.0 13 0 0 0 672 312 L 672 214" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/><ellipse cx="596.0" cy="214" rx="76.0" ry="13" style="fill:var(--vp-c-bg-soft);stroke:var(--vp-c-brand-1);stroke-width:1.5"/>
<text x="596" y="248" text-anchor="middle" style="fill:var(--vp-c-text-1);font-size:12px;font-weight:600">Tenant C database</text>
<text x="596" y="268" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">products · orders</text>
<text x="596" y="284" text-anchor="middle" style="fill:var(--vp-c-text-2);font-size:11px">customers</text>
</svg>

The diagram shows the main request paths. Selected store data is also copied to the landlord for platform reporting, as explained below.

### Three database roles

The Bagisto Multi DB SaaS implementation separates these responsibilities:

| Database | Purpose | Examples |
| --- | --- | --- |
| Landlord | Platform administration and tenant directory | Companies, domain mappings, subscription plans, billing records, database metadata, and reporting copies |
| Tenant | A merchant's operational store | Catalog, categories, customers, orders, channels, and store configuration |
| Template | Starting point for new stores | Migrated and seeded Bagisto tables copied during provisioning |

Tenant store tables use Bagisto's own data model without requiring a `company_id` column to separate merchants inside those tables. A product ID or order ID can therefore repeat in different tenant databases. Platform reporting copies include the tenant identifier so those repeated IDs remain distinguishable.

### Domain-based request routing

The platform resolves a request using the tenant's registered domain, custom domain mapping, or channel hostname. It selects that tenant's database connection before store operations run. Platform administration continues to use the landlord connection.

For example, a request to Merchant A's store reads Merchant A's catalog database. A request to Merchant B's store uses Merchant B's database, even when both stores contain a product with the same numeric ID. Domain mapping selects the store; authentication and permissions still control what a visitor or administrator can do.

See [Custom Domain Mapping](/domains/cname-mapping) for the domain setup workflow.

## What happens when a merchant registers

Provisioning has a lifecycle rather than a guaranteed completion time:

1. **Register the tenant.** The platform records the company and a pending database entry in the landlord.
2. **Dispatch provisioning.** A provisioning job is submitted through Laravel's configured queue connection. Production deployments need workers for asynchronous processing.
3. **Prepare the store.** The provisioner ensures the template is available, creates a database from it, and copies the starting store assets.
4. **Apply tenant details.** It initializes the merchant's channel, owner account, and store settings.
5. **Mark the database ready.** Store access becomes available after successful provisioning. Until then, visitors see the provisioning state.

If provisioning fails, the database is marked **failed** with an error recorded for investigation. An authorized operator can retry after addressing the cause. Queue capacity, database permissions, template readiness, and storage performance all affect onboarding time.

The default naming convention uses the `bagisto_tenant_` prefix and tenant username, with a suffix when needed to avoid a collision. Database naming and template settings are configurable.

## Isolation beyond the database

Separate store databases are only one part of this implementation's tenant handling:

| Resource | How the implementation separates it |
| --- | --- |
| Cache | Tenant-specific prefixes and file-cache directories |
| Store files | Separate tenant directories on configured storage disks |
| Sessions | A tenant check rejects a session used in a different tenant context |
| Elasticsearch | Tenant-specific index prefixes |
| Queued store jobs | The dispatching tenant's ID travels with the job; processing restores its context |
| Scheduled store commands | Configured store commands are run for each active tenant |

These boundaries matter because two stores can have identical record IDs. An image path, cached product, or queued order task needs the tenant context as well as the record ID.

Custom extensions and integrations must preserve this context. A separate database does not automatically make external storage, third-party services, or custom jobs tenant-aware.

## How platform-wide reporting works

The super admin can review Products, Orders, and Customers across stores through **reporting copies in the landlord database**. The implementation synchronizes selected records after store changes and provides a scheduled rebuild to reconcile reporting data.

These views support platform oversight without making each reporting page query every tenant database. If synchronization fails, a reporting copy may lag behind the store until reconciliation succeeds. The tenant database remains the source for operational store records.

This also means that selected product, order, and customer information exists centrally. When documenting data locations or planning retention, include reporting copies alongside tenant databases, backups, logs, and files. See [Tenant Data](/super-admin/tenant-data).

## Managing tenant databases

In this variant, **Tenants > Tenant Databases** gives authorized super-admin users database metadata such as status, server, size, table count, and pending migrations. Available operations include refreshing statistics, applying pending migrations, and retrying failed provisioning.

Archiving a tenant retains its database and files while stopping normal store access. Permanent deletion is a separate operation. An archive is retained live data, so it should not be treated as an independent backup.

### Backups and recovery

A separate database gives a store a clearer backup and restore boundary. Backup schedules, retention, encryption, and restore procedures still need to be configured by the hosting or operations team; database separation does not create an automatic backup service.

A complete store recovery includes its files and the relevant landlord records. After restoration, reconcile reporting copies, search indexes, pending jobs, and external transactions so the recovered store and platform agree.

### Upgrades and migrations

Updates can involve landlord migrations, migrations for existing tenant databases, and refreshing the template for future registrations. Updating the template alone does not update existing stores.

The implementation includes tenant migration tooling and records pending migrations. Operators should track failures and schema versions across the tenant fleet. All stores still use shared application code, so separate databases do not imply independently installed application versions or unrestricted tenant-specific schemas.

### Scaling and shared resources

The tenant connection supports server and credential overrides, giving operators a basis for placing a tenant database on different infrastructure. Moving a live store still needs a planned data transfer, connection update, validation, and recovery procedure; it is not an automatic scaling action.

Databases on the same server continue to share its capacity. Application servers, queue workers, storage, and the landlord can also remain shared dependencies. Microsoft's [database tenancy guidance](https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/service/sql-database) explains the distinction between database isolation and resource allocation; its Azure-specific services are not Bagisto requirements.

## When to choose multiple databases

Consider this model when a separate store database is valuable to your service offering, or when clients need individual recovery and database placement to be planned explicitly. For example, a platform serving established retailers may prioritize restoring one merchant's store without extracting its rows from a shared store database.

Budget for provisioning workers, database monitoring, backups, and migrations across the full tenant fleet. A large number of tenants is possible, but it increases the need for operational automation.

Separate databases can support a broader isolation strategy. Dedicated hardware, regional placement, and contractual data-handling requirements must still be addressed in the deployment design. Microsoft's [storage isolation guidance](https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/approaches/storage-data) describes these as separate architectural choices.

## Frequently asked questions

### Does every merchant get a dedicated server?

No. Each merchant gets a separate store database. Several databases may run on one server, and application services can remain shared.

### Will an issue in one database leave every other store unaffected?

A store-specific data problem can have a smaller scope, but shared server, application, storage, queue, or landlord failures can affect multiple stores. Availability depends on the deployment as a whole.

### Can the platform owner access tenant data?

The platform includes authorized cross-tenant reporting and tenant administration capabilities. Database separation protects the boundary between merchants; it does not remove the platform operator's administrative responsibilities or access.

### Is multiple database always the better choice?

No. Choose it when the database boundary justifies the operational work. If shared tables meet your service requirements and simpler administration is a priority, review [Single Database](/getting-started/single-database) and the [architecture comparison](/getting-started/introduction#single-database-vs-multiple-databases).
