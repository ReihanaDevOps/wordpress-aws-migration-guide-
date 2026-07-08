
<h1>The original architecture</h1>

```bash
                    Internet
                        │
                        ▼
                payswift.ae
                        │
                Route53 DNS
                        │
                        ▼
               Elastic IP (Old)
                        │
                        ▼
                  EC2 Instance
                        │
              Apache + WordPress
                        │
          Let's Encrypt SSL Certificate
```
The SSL certificate was installed inside that EC2 instance.

The certificate files were stored on disk.

Example:

/etc/letsencrypt/live/payswift.ae/

contains

fullchain.pem
privkey.pem
cert.pem
chain.pem

Those files belong to that server, not to the domain.
