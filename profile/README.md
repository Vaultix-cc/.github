<div align="center">

# Vaultix

### The Complete Software Licensing & Customer Management Platform

**Licensing. Authentication. File distribution. Subscriptions. Support.**
Everything you need to ship secure software, in one place.

[![Website](https://img.shields.io/badge/Website-vaultix.cc-6366f1?style=for-the-badge)](https://vaultix.cc)
[![Docs](https://img.shields.io/badge/Read-Docs-22c55e?style=for-the-badge)](https://vaultix.cc)
[![Made in Germany](https://img.shields.io/badge/Made%20in-Germany%20%F0%9F%87%A9%F0%9F%87%AA-000000?style=for-the-badge)](https://vaultix.cc)
[![GDPR](https://img.shields.io/badge/GDPR-Compliant-0ea5e9?style=for-the-badge)](https://vaultix.cc)

</div>

---

## What is Vaultix?

Vaultix is an all-in-one SaaS platform for **software licensing, subscription management, and customer support**, built specifically for developers and businesses selling desktop applications, tools, games, and digital products.

Stop juggling a third-party licensing service, a separate auth system, generic file hosting, spreadsheets for customers, and a disconnected support tool. Vaultix combines all of it into a single, cohesive platform where every piece works together.

> Take control of your software distribution. Protect your revenue. Delight your customers.

---

## Core Features

| Feature | Description |
|---|---|
| **License Key Management** | Generate cryptographically secure keys with custom prefixes (`YOURAPP-XXXX-XXXX-XXXX-XXXX`). You control generation, validation, and revocation. |
| **HWID Binding** | Lock licenses to specific machines to stop piracy and key sharing. Configurable reset limits keep legitimate upgrades smooth. |
| **Flexible Authentication** | Choose key-based auth (no passwords) or username/password accounts, per app. Both support persistent auto-login tokens. |
| **Subscription Management** | Create, pause/resume, extend, and bulk-edit subscriptions. Lifetime licenses and download tracking included. |
| **Secure File Distribution** | Upload encrypted files or whole folders with SHA256 verification, version control, and download analytics. |
| **Discord Integration** | Built-in ticket system with round-robin assignment, staff on-call status, and live subscription tools right inside Discord. |
| **Team & Permissions** | Granular, per-product and app-wide role-based access control with full audit logging. |
| **Session Security** | Track sessions, IPs, and user agents. Instant revocation, 7-day auto-login, and TOTP / email-code 2FA. |

---

## Security First

- **AES-256-GCM encryption** with PBKDF2 key derivation for all file transfers and sensitive data
- **HWID binding** for anti-piracy protection
- **JWT-based authentication** with secure token management
- **Certificate pinning** and **request signing** for advanced hardening
- **Comprehensive session tracking** with instant revocation
- We **never store your files** — encrypted streaming keeps your data private

---

## Who It's For

- **Desktop App Developers** — ship Windows, macOS, and Linux apps with professional licensing built in
- **Game Developers** — secure launchers, anti-cheat HWID binding, automatic updates, and Discord player support
- **SaaS Businesses** — subscription billing, customer tracking, and team collaboration
- **Discord Communities** — manage everything from the hub your users already live in

---

## Quick Start (SDK)

The **Vaultix SDK** is available for multiple languages, so you can integrate in minutes.öö

| Language | Status | Package Manager |
|---|---|---|
| C++ | Stable | CMake |
| C# | Stable | NuGet |
| Python | Stable | pip |
| Java | Stable | Maven |

```python
from vaultix import VaultixClient

async def main():
    client = VaultixClient("https://vaultix.cc/api")
    client.set_api_key("ap_your_app_api_key")

    if await client.authenticate_with_username("username", "password"):
        print("Authenticated!")
        await client.download_file("product_id", "file.txt", "./file.txt")
        client.end_session()
```

```csharp
using Vaultix.SDK;

var client = new VaultixClient("https://vaultix.cc/api");
client.SetApiKey("ap_your_app_api_key");

if (await client.AuthenticateWithUsernameAsync("username", "password"))
{
    await client.DownloadFileAsync("product_id", "file.txt", "./file.txt");
    client.EndSession();
}
```

---

## Tech Stack

**Frontend:** React 19 · TailwindCSS · Radix UI
**Backend:** FastAPI (Python) · Pydantic
**Database:** MongoDB
**Auth:** JWT with secure token handling

---

## Get in Touch

| Purpose | Contact |
|---|---|
| Website | [vaultix.cc](https://vaultix.cc) |
| Support | support@vaultix.cc |
| Privacy | privacy@vaultix.cc |
| Legal | legal@vaultix.cc |
| Abuse | abuse@vaultix.cc |

---

<div align="center">

**Vaultix** — Build your application, secured.

Hosted in Germany / EU · GDPR/DSGVO Compliant

© 2026 Vaultix. All rights reserved.

</div>
