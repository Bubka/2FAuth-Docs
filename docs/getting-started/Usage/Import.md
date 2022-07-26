# Import

2FAuth can import data from some 2FA apps to ease the migration.

At this time, only Google Authenticator export is supported, but others will come soon. Note that not all 2FA apps enable data export so 2FAuth will never cover all possible migrations.

## Exporting the data

### Google Authenticator

Open Google Authenticator and use the `⋅⋅⋅` icon to enter the Export feature.

:::mobile-screen
![Start the export feature](/static/gauth_export_accounts.png)
:::

Next choose one or more accounts to export then click the [!button variant="dark" icon="upload" corners="pill" text="Export"] button.

:::mobile-screen
![Export selected accounts](/static/gauth_export.png)
:::

!!!success
Google Authenticator now displays a QR code to import into 2FAuth. Keep it on screen or save a screen capture for later use or backup.
!!!

:::mobile-screen
![Exported QR code](/static/gauth_qrcode.png)
:::

!!!info Multiple QR codes
If you choose to export more than 10 2FA accounts, Google Authenticator will generate several QR codes (one by tens).

:icon-arrow-right: __The Import into 2FAuth process should be executed for each G-Auth QR code.__
!!!

---

## Importing into 2FAuth

The import process consists of 2 steps: The preloading (with validation) of the exported data and the recording of all or part of the valid data.

### Preloading

Proceed exactly as if you were adding a new account in 2FAuth:

1. Click the [!button corners="pill" size="xs" text="New"] button
2. Click the [!button corners="pill" size="xs" text="Scan a QR code"] or [!button size="xs" corners="pill" text="Upload a QR code"] button (depending on wether the G-Auth QR code is on screen or has been captured)
3. Flash/Select the QR code

!!!success
2FAuth now lists all accounts found in the QR code.
!!!

:::mobile-screen
![Preloaded data ready to be recorded](/static/gauth_import_preload.png)
:::

!!!info Duplicates
2FAuth checks for possible duplicates by comparing preloaded accounts with existing accounts in its database. Duplicates are simply flagged, they can still be imported.
!!!

!!!warning Validation
Accounts that do not respect OTP specification are automatically skipped
!!!

### Recording

This step lets you save the preloaded accounts to the 2FAuth database.

- You are free to record or discard the accounts of your choice.
- Click on account titles to get a fresh OTP if you want to check the integrity of any account.
- Nothing is added to 2FAuth until you click on an [!button size="xs" text="Import"] button or the [!button corners="pill" size="s" text="Import all"] button.

:::mobile-screen
![Some recorded accounts](/static/gauth_import_recorded.png)
:::
