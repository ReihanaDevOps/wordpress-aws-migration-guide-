<h1>You actually ran:</h1>

```bash
sudo tar -xzvf /home/ubuntu/wordpress-files.tar.gz -C /
```
Notice the difference:

```bash
-C /
```
This means:

Extract the archive into the root (/) of the Linux filesystem.

<h4>Why did the website still work?</h4>

It depends on how the backup was created.

If your archive was created like this:
```bash
cd /
tar -czvf wordpress-files.tar.gz var/www/html
```
then the archive contains the full directory structure:
```bash
var/
└── www/
    └── html/
        ├── index.php
        ├── wp-admin/
        ├── wp-content/
        └── ...
```
When you extract it with:
```bash
sudo tar -xzvf /home/ubuntu/wordpress-files.tar.gz -C /
```
Linux recreates the original path:
```bash
/
└── var/
    └── www/
        └── html/
            ├── index.php
            ├── wp-admin/
            ├── wp-content/
            └── wp-config.php
```
So the files are restored directly to:
```bash
/var/www/html
```
That's why Apache immediately found them and the website worked.
