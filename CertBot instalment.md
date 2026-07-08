<h1>home/ubuntu# sudo ss -tulpn | grep ':443'</h1>

The command returned nothing, which means:

Apache is not listening on port 443 (HTTPS).

At the same time, WordPress is forcing:
```bash
http://pay.ae
        ↓
https://pay.ae
```
Since nothing is serving HTTPS, the browser can't connect.
```bash
Option 1: Install a new SSL certificate (recommended)
```
If your DNS already points to this new server, install Certbot:
```bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y
```
Then request a certificate:
```bash
sudo certbot --apache -d pay.ae -d www.pay.ae
```
When prompted:

Enter your email.
Agree to the terms.
Choose the option to redirect HTTP to HTTPS

My recommendation

Unless you specifically need www, simply run:
```bash
sudo certbot --apache -d pay.ae
```
That will install HTTPS for the main domain and is the quickest way to get the site online.

After it finishes, run:
```bash
sudo ss -tulpn | grep ':443'
```
You should then see Apache listening on port 443, and https://pay.ae should load correctly.
