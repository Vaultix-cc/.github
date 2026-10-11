<div align="center">

<img src="https://vaultix.cc/logo.png" alt="Vaultix" width="96" />

# Vaultix

### Privacy-first licensing, delivery and support for desktop software

**Privacy by design. Protection by principle.**

[![Website](https://img.shields.io/badge/Website-vaultix.cc-6366f1?style=for-the-badge)](https://vaultix.cc)
[![Docs](https://img.shields.io/badge/Docs-public-22c55e?style=for-the-badge)](https://vaultix.cc/docs)
[![End-to-end encrypted](https://img.shields.io/badge/Files-end--to--end%20encrypted-a78bfa?style=for-the-badge)](https://vaultix.cc/docs/encryption)
[![Hosted in Germany](https://img.shields.io/badge/Hosted%20in-Germany-000000?style=for-the-badge)](https://vaultix.cc/features/privacy-first-licensing)

</div>

---

## What Vaultix does

Vaultix gives software developers **license keys or user accounts, hardware ID binding, end-to-end encrypted file and folder delivery, runtime variables, signed API responses, approved-build checks and Discord support tickets**, managed from one web dashboard and used through a **C++ SDK** (Windows, Linux) or the **REST API**. There is a free plan without payment details.

| Feature | What you get |
|---|---|
| **Licensing** | Keys with your own prefix (`YOURAPP-XXXX-XXXX-XXXX-XXXX`) or username/password accounts per app, subscriptions per product, lifetime licenses, pause and resume, bulk edits |
| **HWID lock** | One license, one PC. Resets with limits you set, or one click in Discord |
| **Zero-knowledge delivery** | Builds, folders and config encrypted and signed in your browser, delivered only to licensed users. One-click rollback after a bad update |
| **Tamper protection** | Approved-build list (executable or its SHA-256), signed requests and responses, randomized integrity heartbeat |
| **Discord support** | Tickets in your own server, verified against the customer's license, on-call routing for your team |
| **Teams** | Permission groups, per-product access, an audit log of who changed what |

---

## Zero-knowledge files

New apps are end-to-end encrypted from the start; existing apps turn it on in App Settings. Vaultix stores and delivers your content but holds no key that opens it.

```text
 Your browser                      Vaultix                          Your customer's app (C++ SDK)
 ────────────                      ───────                          ─────────────────────────────
 encrypt (AES-256-GCM, 1 MiB       stores ciphertext only           one-time X25519 key exchange
 chunks, random key per file)      wraps the sealed key again       verify uploader's Ed25519 signature
 seal key to the app (X25519)  ──▶ (layer 2, server key outside ──▶ check size + SHA-256
 sign the upload (Ed25519)         the database and backups)        decrypt in memory, nothing on disk
                                   re-encrypts every download
                                   with a fresh key (layer 3)
```

| Lock | Who holds the key | What it stops |
|---|---|---|
| **1. End-to-end** | You, the team members you approve, your app | Vaultix, a database or backup leak, or a legal demand reading or changing your content |
| **2. At rest** | Vaultix, kept outside the database and backups | A database dump plus a key pulled from your app being enough |
| **3. Per download** | Nobody: a new one-time key per request | Captured or replayed downloads, and downloads without a live, licensed session |

- **Nothing gets swapped.** Every upload is signed by an approved team member; the SDK refuses any file your team didn't sign, even one served by Vaultix. Rollbacks are signed again in the browser, so the server can't roll a file back on its own.
- **No plain checksums.** Vaultix stores only `HMAC-SHA256(hashKey, sha256(file))`; `IsValidFileHash` compares that keyed hash.
- **Honest limits.** Vaultix still sees file names, sizes and who downloads what. Someone with a valid license who takes apart the running app can read what it decrypts, as with any delivery system.

Full specification: [How end-to-end encryption works](https://vaultix.cc/docs/encryption)

---

## Accounts and keys

| Part | How |
|---|---|
| **Dashboard login** | The password never leaves the browser: `PBKDF2-SHA256` (600,000 rounds) → HKDF into an `authHash` (sent) and a `wrapKey` (never sent). The server stores only a PBKDF2 hash of `authHash` |
| **Personal keys** | X25519 + Ed25519 per account, wrapped with `wrapKey`, so Vaultix stores them but can't open them |
| **App keys** | Derived from a 24-word BIP39 recovery phrase only the app owner holds; your app gets a `vxk_…` key that opens downloads and nothing else |
| **Team access** | Each member has their own key. The owner compares a fingerprint (`5781-8166-D1C1-6717`) and gives access; Vaultix never grants access by itself. When someone leaves, the owner's browser switches the team to a new key |
| **Sessions** | RS256 JWTs; TOTP or email-code 2FA, TOTP secrets encrypted at rest |
| **API responses** | Signed with Ed25519 and verified by the SDK, with a bounded clock-skew window |

---

## C++ SDK

```cpp
#include "VaultixClient.h"

Vaultix::VaultixClient client("https://vaultix.cc/api");
client.SetApiKey("ap_your_app_key");
client.SetContentKey("vxk_your_sdk_key");          // App Settings → End-to-end encryption

if (client.AuthenticateWithKey(licenseKey)) {       // with HWID lock on: tied to this PC
    // signature, size and SHA-256 are checked before you get a single byte
    std::vector<uint8_t> dll = client.DownloadFileToMemory(productId, "game.dll");
    auto variables = client.GetProductVariables(productId);
    uint64_t signedAt = client.GetLastSignedAt();   // refuse versions older than one you've seen
}
```

`AuthenticateWithKey` · `AuthenticateWithUsername` · `DownloadFile` · `DownloadFileToMemory` · `StreamFile` · `DownloadFolder` · `DownloadFolderToMemory` · `IsValidFileHash` · `GetProductVariables` · `GetProductStatus` · `GetProductNews` · `GetAccountInfo` · `HasProductAccess`

C++17, OpenSSL and libcurl. Reference: [C++ client docs](https://vaultix.cc/docs/cpp-client) · [REST API](https://vaultix.cc/docs/api-overview)

---

## Privacy: we keep very little

- **Shortened IPs only** in server logs (IPv4 /24, IPv6 /32), no browser string, no query parameters.
- **No analytics, trackers, ads, third-party cookies or AI services.** Data is never sold.
- **No payment provider.** You pay from your own wallet (USDC or USDT on Solana); Vaultix never sees card or bank details.
- **No names in the books.** Accounting records carry a customer number.

| What | How long |
|---|---|
| Server logs (shortened IP, path, status) | 7 days |
| IP address and browser in your app's audit log (each can be switched off per app) | 7 days |
| Audit log entries | 12 months |
| Your end users' data | your choice per app: 30 days, 90 days, 1 year, or until you delete it |
| Database backups | up to 30 days |
| Accounting records (German law, § 147 AO) | 8 years, customer number only |

**Our stance:** no arbitrary takedowns, no voluntary disclosure of customer data over alleged IP claims, and every legal demand is checked for its legal basis, jurisdiction, authenticity and scope. A person reviews every report. [How reports are handled](https://vaultix.cc/legal/aup) · [Privacy policy](https://vaultix.cc/legal/privacy)

---

## Built with

- **Dashboard:** React 19 · Tailwind CSS · Radix UI · WebCrypto
- **Backend:** FastAPI (Python 3.11) · Pydantic 2 · MongoDB · `cryptography`
- **SDK:** C++17 · OpenSSL · libcurl
- **Hosting:** servers in Germany

---

<div align="center">

[Website](https://vaultix.cc) · [Features](https://vaultix.cc/features) · [Docs](https://vaultix.cc/docs) · [Changelog](https://vaultix.cc/changelog) · [Status](https://vaultix.cc/status) · [Discord](https://discord.gg/acxzwhjc2j)

support@vaultix.cc · privacy@vaultix.cc

</div>
