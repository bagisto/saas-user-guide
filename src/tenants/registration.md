# Registration & Sign-in

Before a merchant can sell, they join the platform and get in. Registration takes three short steps, after which the storefront and admin panel are provisioned automatically.

::: info What you'll learn
- How a merchant registers a store in three steps
- How the store is provisioned, and where the merchant lands afterwards
- How an existing merchant signs in
:::

## Registering a store

Open the platform address and go to the registration page. Registration runs in three steps, shown as **01 Your Sign**, **02 Account**, and **03 You**, with a **Back** button to return to an earlier step. The store's address is reserved at the first step.

**Step 1 — Your Sign.** Enter the **Store name** and a **Username**. The subdomain your store will live at is previewed as you type — it is built from the username, or from the store name while the username is empty. Both the store name and the username must be unused on the platform. Select **Reserve & continue**.

<ImagePopup src="/images/registration/tenant-register-1.png" alt="Registration step 1 — store name and username" />

**Step 2 — Account.** Enter the **Email**, **Password**, and **Confirm password** the store will sign in with. The email must not already belong to another store. Select **Continue**.

<ImagePopup src="/images/registration/tenant-register-2.png" alt="Registration step 2 — account details" />

**Step 3 — You.** Enter the owner's **First name**, **Last name**, and **Phone**. The phone number must be numeric and unique on the platform. Select **Open your store** to finish.

<ImagePopup src="/images/registration/tenant-register-3.png" alt="Registration step 3 — personal details" />

### Registering with a plan

A visitor who picks a plan on the platform's pricing section arrives at registration with that plan already chosen. A **Selected plan** badge shows the plan's name and price — or **Free** — at the top of the form.

## What happens next

Once registration finishes, the store is set up automatically: the storefront, admin panel, a **Default Channel** on the **Default** theme, and ready-made theme content. There is nothing to install. The merchant is also sent a welcome email, **New Company Registered Successfully**, with a **Go to your store admin** button.

Where the merchant lands depends on how they registered:

- **Without a plan** — they arrive on their new **storefront**, with the message *Store Created Successfully*. They sign in to the admin panel from there (see below).
- **With a free or trial plan** — they are signed in to the admin panel automatically and land on **Billing → Overview**.
- **With a paid plan** — they are signed in automatically and taken to checkout to pay for the plan. See [Tenant Billing](/subscriptions/tenant-billing).

If setup fails, the merchant sees *Something went wrong, please try again later.* The store is left ready to retry.

::: info The store is set up for you
The tenant can sign in and begin adding products right away. The [Store Setup checklist](/tenants/store-management#the-store-setup-checklist) on the dashboard guides them through the rest. See [Store & Admin Panel](/tenants/store-management).
:::

## Signing in

A returning merchant selects **Sign in** ("Already a tenant?") on the platform. On the **Find your store** page they enter their **Email** and select **Find my store**. The platform looks up their store and redirects them to its admin login, with the email already filled in.

<ImagePopup src="/images/registration/tenant-sign-in.png" alt="Find your store sign-in" />

If the email does not belong to an active store, the page shows *This email is not registered with us. Please register to continue.*

They can also go straight to their store's admin login at **their-domain/admin/login**.

<ImagePopup src="/images/registration/tenant-admin-login.png" alt="Tenant admin login" />

::: tip
Each store signs in at its own address, not a shared one. If a merchant isn't sure of theirs, the **Find your store** page finds it from their email and sends them to the right door.
:::
