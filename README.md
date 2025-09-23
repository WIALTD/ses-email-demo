![Node.js](https://img.shields.io/badge/Node.js-18+-green)
![AWS SES](https://img.shields.io/badge/AWS-SES-orange)
![API](https://img.shields.io/badge/API-SES-blue)
![Purpose](https://img.shields.io/badge/Purpose-Educational-yellow)
![Status](https://img.shields.io/badge/Status-Demo-brightgreen)
![License: MIT](https://img.shields.io/badge/License-MIT-lightgrey)

# GGG Email - AWS SES Integration

This repository demonstrates how **Global Guide Group** migrated its transactional email system from **SendGrid** to **Amazon SES (Simple Email Service)**.  
This project mirrors the structure of our earlier SendGrid demo but swaps in AWS SES using the official API route.

## Why the Migration?

- **Cost Efficiency**: SES offers pay-as-you-go pricing, far cheaper than SendGrid's plans for low to medium volume.  
- **Scalability**: SES is enterprise-grade and scales seamlessly with AWS infrastructure.  
- **Security**: IAM policies + Secrets Manager keep credentials safe (no API keys hard-coded).  
- **Future-Proofing**: SES is reusable across projects, reducing reliance on third-party SaaS.  

This repo is a **companion** to [GGG Email - SendGrid](https://github.com/WIALTD/sendgrid-email-demo), showing how the system evolved.

### AWS SES Dashboard (Verified Domain)
![SES Dashboard](assets/ses-dashboard.png)

---

## Tech Stack

- **Backend**: Wix Velo (with Node.js)  
- **Email Service**: Amazon SES API  
- **Secrets Manager**: Wix Secrets Manager for secure credential storage  

### DKIM Verification Success
![DKIM Success](assets/dkim-success.png)

---

## Project Structure

```
ggg-email-ses/
├── .env.example          # Example of required secrets
├── .gitignore
├── Index.js              # Example entry point
├── package.json
├── sesEmail.jsw          # Wix backend email sender (SES API)
└── assets/               # Screenshots of SES + Wix Secrets setup
```

---

## Assets

This repo includes an `/assets` folder that contains supporting screenshots and configuration examples:  

- **ses-dashboard.png** – AWS SES console showing verified domain + sandbox limits.  
- **dkim-success.png** – Confirmation of DKIM verification for domain.  
- **wix-secrets-manager.png** – Wix Secrets Manager setup, demonstrating secure storage of SES keys.  

These are included for **portfolio/visa documentation purposes** to illustrate the real-world configuration steps alongside the code.

## Secrets Manager Setup (Wix)
![Wix Secrets Manager](assets/wix-secrets-manager.png)

AWS SES requires an **Access Key ID** and **Secret Access Key** for API calls.  
For security, these credentials are **never hard-coded** in the project. Instead:  

1. Create three entries in Wix Secrets Manager (`SES_ACCESS_KEY_ID`, `SES_SECRET_ACCESS_KEY`, and`SES_REGION`). 
2. Reference them in backend code using `wix-secrets-backend`.  
3. This mirrors best practices in production: sensitive credentials live in encrypted storage, not in your repo.  

```js
import { getSecret } from 'wix-secrets-backend';

export async function sendSesEmail(to, subject, htmlBody, textBody) {
  const accessKeyId = await getSecret("SES_ACCESS_KEY_ID");
  const secretAccessKey = await getSecret("SES_SECRET_ACCESS_KEY");
  // ... SES client logic
}
```

## Usage

1. Copy `.env.example` → `.env` and fill in your SES keys + region.  
2. Store these values in **Wix Secrets Manager** instead of hardcoding them.  
3. Import and call `sendSesEmail()` from your Wix frontend code.  

Example call:

```js
import { sendSesEmail } from 'backend/sesEmail';

$w.onReady(function () {
  sendSesEmail("hello@globalguidegroup.com", "Test Subject", "<p>Hello World</p>", "Hello World")
    .then(res => console.log(res))
    .catch(err => console.error(err));
});
```

## License

MIT
