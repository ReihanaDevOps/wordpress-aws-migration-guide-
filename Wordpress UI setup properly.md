<h1>UI Refresh Element and Permalink Settings</h1>

<img width="1616" height="936" alt="image" src="https://github.com/user-attachments/assets/d23707c2-7086-4901-96aa-a2e156237272" />
<img width="967" height="867" alt="image" src="https://github.com/user-attachments/assets/b8a23f55-a692-4c1b-bead-9453fec9ee8d" />

<h2>Senario </h2>

* ✅ WordPress is running.
* ✅ SSL is working.
* ✅ Apache is serving the site.
* ❌ Only the **pretty URLs** (like `/products/`) are returning 404.

This is a **very common issue after migrating WordPress** and is usually caused by missing or stale **permalink rewrite rules**.

## Step 1 (Most likely fix): Flush the permalinks

Log in to WordPress Admin:

```text
https://pay.org/wp-admin
```

Then go to:

**Settings → Permalinks**

**Do not change anything.**

Just click:

**Save Changes**

This regenerates the rewrite rules and often recreates the correct `.htaccess` configuration. ([One.com Support][1])

---

## Step 2: Check the `.htaccess` file

Run:

```bash
cat /var/www/html/.htaccess
```

It should contain something similar to:

```apache
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```

If it's missing or empty, WordPress won't be able to resolve URLs like `/products/`. ([One.com Support][1])

---

## Step 3: Make sure Apache's rewrite module is enabled

Run:

```bash
sudo a2enmod rewrite
```

Then:

```bash
sudo systemctl restart apache2
```

---

## Step 4: Verify the Apache configuration

Run:

```bash
sudo apache2ctl -M | grep rewrite
```

You should see:

```text
rewrite_module (shared)
```

---

## Step 5: Allow `.htaccess` overrides

Run:

```bash
sudo grep -n "AllowOverride" /etc/apache2/apache2.conf
```

Look for the block:

```apache
<Directory /var/www/>
```

It should be:

```apache
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

If it's set to:

```apache
AllowOverride None
```

then WordPress rewrite rules will be ignored.


