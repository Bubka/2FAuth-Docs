# Import

2FAuth can import data in various formats to ease you migrate from another 2FA app or to restore a backup.
Note that not all 2FA apps enable data export so 2FAuth will never covers all possible migrations.

## Exporting the data

First step is to export data from the 2FA source app in order to get a migration resource, like a file or a QR code. Follow one of the dedicated section below then jump to the [Importing into 2FAuth](#importing-into-2fauth) step.

### Google Authenticator

Open Google Authenticator and use the [!button variant="ghost" size="s" corners="round" text="•••"] icon to enter the _Export_ feature.

:::mobile-screen
![Start the export feature](/static/import/gauth_export_accounts.png)
:::

Next choose one or more accounts to export then click the [!button variant="dark" icon="upload" corners="pill" text="Export"] button.

:::mobile-screen
![Export selected accounts](/static/import/gauth_export.png)
:::

!!!success
Google Authenticator now displays a QR code to import into 2FAuth. Keep it on screen or save a screen capture for later use or backup.
!!!

:::mobile-screen
![Exported QR code](/static/import/gauth_qrcode.png)
:::

!!!info Multiple QR codes
If you choose to export more than 10 2FA accounts, Google Authenticator will generate several QR codes (one by tens).

:icon-arrow-right: __The Import into 2FAuth process should be executed for each G-Auth QR code.__
!!!

### 2FAS Authenticator

Open 2FAS Authenticator and enter the :icon-gear: _Settings_ section. Then enter the _2FAS Backup_ feature.

:::mobile-screen
![2FAS Settings](/static/import/2FAS_settings.png)
:::

Enter the _Export_ tool.

:::mobile-screen
![2FAS Backup](/static/import/2FAS_backup.png)
:::

__Uncheck the password option__ and click the [!button variant="danger" corners="round" text="Export to file"] button.

:::mobile-screen
![2FAS Export](/static/import/2FAS_export.png)
:::

!!!success
Save (and secure) the .2fas migration file, you are ready to import your accounts into 2FAuth
!!!

### Aegis Authenticator

:::has-vertical-button
Open Aegis Authenticator and use the [!button variant="primary" size="s" corners="round" text="•••"] icon to enter the _Settings_ section.
:::

:::mobile-screen
![Aegis](/static/import/aegis_home.png)
:::

Enter the _Import & Export_ feature.

:::mobile-screen
![Aegis Settings](/static/import/Aegis_settings.png)
:::

Open the _Export_ tool.

:::mobile-screen
![Aegis Import & Export](/static/import/Aegis_import_export.png)
:::

Whatever the export format (both are supported by 2FAuth), __uncheck the encryption option__ and click the [!badge variant="danger" corners="square" text="OK" size="l"] button.

:::mobile-screen
![Aegis Export](/static/import/Aegis_export.png)
:::

!!!success
Save (and secure) the migration file, you are ready to import your accounts into 2FAuth
!!!

---

## Importing into 2FAuth

The import process consists of 2 steps: The preloading (with validation) of the exported data and the recording of all or part of the valid data.

### Preloading

From the 2FAuth main view:

1. Click the [!button corners="pill" size="xs" text="New"] button, just as if you had to add a new account
2. Click the [!button corners="pill" size="xs" text="Import"] button in the alternative methods
3. Submit your migration resource: [!button corners="pill" size="xs" text="Livescan" icon="device-camera"] a QR code or [!button corners="pill" size="xs" text="Upload"] a file/QR code

!!!success
2FAuth now lists all accounts found in the migration resource.
!!!

:::mobile-screen
![Preloaded data ready to be recorded](/static/import/import_preload.png)
:::

!!!info Duplicates
:::mobile-screen
![](/static/import/import_duplicates.png)
:::
2FAuth checks for possible duplicates by comparing preloaded accounts with existing accounts in its database.  
Duplicates are simply flagged, they can still be imported.
!!!

!!!warning Validation
Accounts that do not respect OTP specification are automatically skipped
!!!

### Recording

This final step lets you save the preloaded accounts to the 2FAuth database.

- You are free to record or discard the accounts of your choice.
- Click on account titles to get a fresh OTP if you want to check the integrity of any account.
- Nothing is added to 2FAuth until you click on an [!button size="xs" text="Import"] button or the [!button corners="pill" size="s" text="Import all"] button.

:::mobile-screen
![5 accounts registered, 3 remaining](/static/import/import_recorded.png)
:::
