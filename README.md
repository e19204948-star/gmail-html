# Virtual SIM — Browser-based eSIM & Messaging Demo

A lightweight, **zero-backend** tool for generating mock eSIM profiles, managing virtual SIM contacts, and simulating end-to-end encrypted messaging — all running in the browser with `localStorage`.

> ⚠️ **Demo only.** This tool is for testing, prototyping, and educational purposes. It does not connect to any real SIM, eSIM infrastructure, or mobile network.

## Live Demo

[**Open on GitHub Pages →**](https://your-username.github.io/virtual-sim/)

## Features

| Feature | Description |
|---|---|
| eSIM Generator | Generate mock profiles with ICCID, EID, activation codes, QR codes |
| Contact Manager | Add contacts, assign multiple virtual SIMs per contact |
| E2E Encrypted Chat | ECDH + HKDF → AES-GCM encryption between SIMs, entirely in-browser |
| Import / Export | JSON round-trip for all data (contacts, eSIMs, messages) |
| Drag & Drop | Drop eSIM JSON files directly onto the generator |
| Dark mode | Automatic via `prefers-color-scheme` |

## Pages

```
index.html      → Landing page
chat.html       → Contact list + SIM manager + encrypted chat
esim.html       → eSIM profile generator
list.html       → Contacts & eSIM listing / import-export
```

## Cryptography

Each virtual SIM generates an **ECDH P-256 keypair** on creation (via Web Crypto API). When two SIMs with keys exchange a message:

1. ECDH derives shared bits from sender's private key + recipient's public key
2. HKDF (SHA-256) derives a fresh AES-256-GCM key with a random salt
3. AES-GCM encrypts the plaintext with a random IV
4. Only `{ciphertext, iv, salt}` are stored — no plaintext, no shared secret

Both parties can decrypt because ECDH is symmetric: Alice's private × Bob's public = Bob's private × Alice's public.

## Getting Started

```bash
# Clone
git clone https://github.com/your-username/virtual-sim.git
cd virtual-sim

# Serve locally (any static server works)
npx serve .
# or
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Deploy to GitHub Pages

1. Push to a GitHub repo
2. Go to **Settings → Pages → Source: Deploy from branch → `main` / `root`**
3. Your site will be live at `https://your-username.github.io/virtual-sim/`

## Data Storage

All data is stored in **`localStorage`** under two keys:

| Key | Contents |
|---|---|
| `virtual_sim_comms_v1` | `{ contacts, messages }` |
| `esim_profiles_v1` | `[ ...profiles ]` |

No data is ever sent to any server.

## Browser Support

Requires **Web Crypto API** (available in all modern browsers). Chrome 60+, Firefox 57+, Safari 11+, Edge 79+.

## License

MIT
