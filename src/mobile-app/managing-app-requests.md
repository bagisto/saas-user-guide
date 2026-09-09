# Managing App Requests

Every tenant's app request arrives in one place for the super admin to review, send to the mobile team, and close out with a download link. This page covers that side of the workflow.

::: info What you'll learn
- How to review a tenant's app request
- How to raise a build ticket with the mobile team
- How to deliver the finished app, request a correction, or reject
:::

## Viewing requests

Go to **Mobile App Requests → All Requests** to see every request across all tenants, with the tenant name and domain alongside each one. You can **search**, **filter**, and **paginate** to find a request quickly.

<ImagePopup src="/images/managing-app-requests/super-app-requests.png" alt="All app requests" />

Click the **View** (eye) icon on a row to open the full request.

<ImagePopup src="/images/managing-app-requests/super-request-detail.png" alt="App request detail" />

The detail screen shows the app name, platform, application IDs, every generated image, and the credential groups the tenant supplied. Credentials are stored encrypted and shown as **Stored** rather than in plain text.

## Downloading the images

Select **Download images (ZIP)** to get everything in one archive, foldered by platform.

- **android/** — a `splash/` folder with 7 sizes, named by their exact pixels, and a `logo/` folder.
- **ios/** — a `splash/` folder with 8 sizes, named the same way, and a `logo/` folder.
- **README.txt** — the app name, request reference, tenant, and what each folder holds.

Each platform folder carries its own copy of the logo, so a build team working on one platform has everything in one place.

## Step 1 — Raise the build ticket

With the request open, select **Raise Ticket**. Add an optional priority and comment, then confirm.

<ImagePopup src="/images/managing-app-requests/super-raise-ticket.png" alt="Raise ticket panel" />

Confirming creates a ticket in the helpdesk and posts the full build brief to it, with the image archive attached. The panel returns immediately — the ticket is raised in the background, so refresh after a moment to see the number.

The request moves to **Processing**, and a Helpdesk Ticket card appears on it with a link straight to the ticket and its replies.

::: warning
The build details — application IDs, and the Apple, Google Play and Firebase credentials — are posted as an **internal note**. Agents can read it on the ticket, but it is never included in the confirmation e-mail sent to the ticket's contact address.
:::

## Step 2 — Deliver the app

When the team returns the finished build, paste its URL into **Mark as Completed**, with an optional version.

A request for one platform asks for one URL. A request covering **both** asks for two, because the builds live at different addresses — an `.apk` or `.aab` for Android, an App Store link for iOS.

<ImagePopup src="/images/managing-app-requests/super-mark-completed.png" alt="Mark as Completed form" />

Saving records the links, moves the request to **Completed**, and e-mails the tenant. Issuing new links disables any earlier ones.

## Requesting a correction

If something the tenant submitted is wrong — a logo that reads badly on white, the wrong bundle ID — you do not have to reject the whole request.

Use **Request Correction** on the field in question and give a reason. The request **stays Pending** and only that one field reopens for the tenant to edit; everything else remains locked. The tenant is e-mailed with your reason.

<ImagePopup src="/images/managing-app-requests/super-request-correction.png" alt="Request correction on a field" />

## Rejecting a request

If the request cannot go ahead at all, use **Reject** and give a reason. The tenant is e-mailed, and the reason is shown on their copy of the request.

Rejecting also frees the platform, so the tenant can raise a fresh request for it.

## Configuration

Helpdesk settings live under **Configure → Mobile App Req → UVdesk**.

<ImagePopup src="/images/managing-app-requests/super-configuration.png" alt="Mobile App Request configuration" />

- **Enable** — turns ticket raising on. With it off, **Raise Ticket** reports that the helpdesk is disabled.
- **Helpdesk URL** — the instance root only, for example `https://help.uvdesk.com`.
- **Access Token** — generated from **UVdesk → Dashboard → Profile → Access Tokens**.
- **Ticket Type** — the numeric ticket type id to file under.
- **From Name** / **From Email** — who the ticket appears to come from.

::: tip Set a From Email
Leave **From Email** blank and tickets are raised from whichever super admin pressed the button, so helpdesk replies land in that person's personal inbox. Set it to a shared platform address instead — the ticket sender stays consistent no matter who raises it.
:::

## Statuses

- **Pending** — the tenant has submitted. The form is locked, and you can request a correction on individual fields.
- **Processing** — the helpdesk ticket has been raised.
- **Completed** — the download links are recorded and the tenant has been e-mailed.
- **Rejected** — you declined it, with a reason the tenant can read.

::: tip
Every request is scoped to the tenant that raised it. Tenants see only their own requests; the super admin sees them all in one list.
:::
