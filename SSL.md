An SSL (Secure Sockets Layer) certificate is a digital file that authenticates a website's identity and enables an encrypted connection between a browser and a server
<H1>SSL certificate</H1>

<img width="601" height="552" alt="image" src="https://github.com/user-attachments/assets/ec28515f-aa17-4242-bd55-a6abe9ddefba" />
<br>

<h2>Very Important Concept</h2>

Many people think
```bash
"SSL belongs to the domain."
```
Technically...
```bash
❌ Not exactly.
```
The certificate is issued for the domain, but it is installed on the server.

Think of it like this.
```bash
The domain is your house address.

The SSL certificate is the key stored inside the house.

When you build a new house (new EC2),

there is no key inside.

You must install one.
```
During your migration

Old Server
```bash
EC2 A

Apache

SSL

WordPress

New Server

EC2 B

Apache

WordPress

NO SSL
```
After restoring WordPress

Apache was listening only on
```bash
80
```
You verified it
```bash
sudo ss -tulpn | grep ':443'
```
returned

Nothing

Meaning

HTTPS

does not exist.

```bash
sudo apt update
sudo apt install apache2
```
[Visit AWS](https://aws.amazon.com)

| Service | Purpose |
|----------|----------|
| EC2 | Host WordPress |
| Route53 | DNS |
| Elastic IP | Static IP |
| Apache | Web Server |


> [!NOTE]
> WordPress redirects to HTTPS based on Site URL.

> [!TIP]
> Test using the Elastic IP before changing Route53.

> [!IMPORTANT]
> Verify which database WordPress actually uses before creating a backup.

> [!WARNING]
> Keep the old server until the new server is fully validated.

> [!CAUTION]
> Changing DNS before testing may increase downtime.


```mermaid
flowchart TD

A[User]

-->

B[Route53]

-->

C[Elastic IP]

-->

D[EC2]

-->

E[Apache]

-->

F[WordPress]

-->

G[MariaDB]
```


README.md

🚀 AWS WordPress Migration

──────────────────────────────

📖 Overview

🏗 Architecture

💾 Backup

☁ AWS Infrastructure

🌍 Route53

🔒 SSL

🐳 Apache

🗄 MariaDB

🐞 Troubleshooting

📘 Lessons Learned

📚 References
