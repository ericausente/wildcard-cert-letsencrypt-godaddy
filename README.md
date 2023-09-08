# wildcard-cert-letsencrypt-godaddy

Step-by-step guide to  s obtain a wildcard SSL/TLS certificate for my *.kushikimi.xyz domain using Certbot and the GoDaddy DNS plugin:

## Step-by-Step Guide: Obtaining a Wildcard SSL/TLS Certificate with Certbot and GoDaddy

This guide will walk you through the process of obtaining a wildcard SSL/TLS certificate for your domain using Certbot and the GoDaddy DNS plugin. A wildcard certificate secures your domain and all its subdomains.

## Prerequisites:

- A GoDaddy account with your domain registered.
- A server or hosting environment where you can run Certbot.
- Python 3 and pip installed on your server.


### Step 1: Install Certbot

Install Certbot on your server using your package manager. For example, on Ubuntu, you can use:

```
sudo apt-get update
sudo apt-get install certbot
```

### Step 2: Install the Certbot GoDaddy DNS Plugin

Install the Certbot DNS plugin for GoDaddy by running:

```
sudo apt-get install python3-pip
sudo pip3 install certbot-dns-godaddy
```

### Step 3: Configure GoDaddy API Credentials

Create a godaddy.ini file to store your GoDaddy API credentials. Replace your_api_key and your_api_secret with your actual API key and secret obtained from GoDaddy.

```
sudo nano /etc/letsencrypt/godaddy.ini
```

Add the following content to the godaddy.ini file:

```
dns_godaddy_key = your_api_key
dns_godaddy_secret = your_api_secret
```

Save and close the file.

### Step 4: Request the Wildcard Certificate Using Certbot's Manual Mode:

Run Certbot in manual mode with the --preferred-challenges dns option to specify DNS challenges:
```
sudo certbot certonly --manual --preferred-challenges dns -d kushikimi.xyz -d *.kushikimi.xyz
```

Certbot will provide you with the DNS-01 challenge information:
```
Please deploy a DNS TXT record under the name:

_acme-challenge.kushikimi.xyz.

with the following value:

WNU1K3Fv_Mf9LUEz5wqY4fd9koexquuarEsq19XLq_I

Press Enter to Continue
```

Create DNS TXT Records Manually:

Follow the instructions provided by Certbot to create DNS TXT records manually in your GoDaddy DNS settings:
- Log in to your GoDaddy account.
- Navigate to the DNS management section for your domain.
- Create a new DNS TXT record with the name _acme-challenge.kushikimi.xyz and the value provided by Certbot.

Press Enter to Continue:
After you've created the DNS TXT records, return to the terminal where Certbot is running, and press Enter to continue.

Certbot Verification:
Certbot will verify that the DNS TXT records match the values it expects. If the verification is successful, Certbot will issue the wildcard certificate.



Sample output: 

```
ubuntu@ip-172-31-17-131:/etc/letsencrypt$ sudo certbot certonly --manual --preferred-challenges dns -d kushikimi.xyz -d *.kushikimi.xyz
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Requesting a certificate for kushikimi.xyz and *.kushikimi.xyz

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name:

_acme-challenge.kushikimi.xyz.

with the following value:

WNU1K3Fv_Mf9LUEz5wqY4fd9koexquuarEsq19XLq_I

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Press Enter to Continue

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name:

_acme-challenge.kushikimi.xyz.

with the following value:

V3zUXm_vLgC1ZGNSubDj6R1saMD4PhNviuksqpg2Iv0

(This must be set up in addition to the previous challenges; do not remove,
replace, or undo the previous challenge tasks yet. Note that you might be
asked to create multiple distinct TXT records with the same name. This is
permitted by DNS standards.)

Before continuing, verify the TXT record has been deployed. Depending on the DNS
provider, this may take some time, from a few seconds to multiple minutes. You can
check if it has finished deploying with aid of online tools, such as the Google
Admin Toolbox: https://toolbox.googleapps.com/apps/dig/#TXT/_acme-challenge.kushikimi.xyz.
Look for one or more bolded line(s) below the line ';ANSWER'. It should show the
value(s) you've just added.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Press Enter to Continue

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/kushikimi.xyz/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/kushikimi.xyz/privkey.pem
This certificate expires on 2023-12-07.
These files will be updated when the certificate renews.

NEXT STEPS:
- This certificate will not be renewed automatically. Autorenewal of --manual certificates requires the use of an authentication hook script (--manual-auth-hook) but one was not provided. To renew this certificate, repeat this same certbot command before the certificate's expiry date.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

### Step 5: Certificate Renewal

Certbot will automatically handle certificate renewals. You don't need to worry about manually renewing the certificate.

### Step 6: Configure Your Web Server

Configure your web server (e.g., Apache or Nginx) to use the obtained SSL/TLS certificate for your domain and subdomains. The exact configuration steps depend on your web server software.

Step 7: Schedule Automatic Renewal (Optional)

To automate the certificate renewal process, you can set up a cron job to run Certbot's renewal process periodically. Create a cron job like this:

```
sudo crontab -e
```

Add the following line to run Certbot every day:

```
0 0 * * * /usr/bin/certbot renew --quiet
```

Save and exit the editor. Certbot will automatically renew the certificate if it's within 30 days of expiration.

That's it! You've successfully obtained a wildcard SSL/TLS certificate for your domain using Certbot and GoDaddy DNS. Your website and subdomains are now secured with SSL/TLS encryption.
