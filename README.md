# OpenSocialNetwork Protocol (OSNP)

**Status:** Draft RFC
**Version:** 0.1.0-alpha
**License:** MIT

```
You own your email. You own your domain.
Why don't you own your social network?
```

## TL;DR

OpenSocialNetwork Protocol (OSNP) is an open protocol for decentralized social networking.

- **Your domain = Your identity** (like email, but for social)
- **Your server = Your rules** (no algorithmic feed, no shadow bans, no corporate moderation)
- **Cryptographic signatures** = Proof of authorship, proof of date
- **REST over HTTPS** = Dead simple implementation
- **TextBundle format** = Export your data anytime, read it anywhere

## Why?

We've built the internet on open standards: TCP/IP, HTTP, SMTP, DNS.

Then we handed our social lives to corporations that:

- Decide what you can say
- Decide who sees your content
- Sell your data to advertisers
- Change the rules whenever they want
- Can delete your 10-year archive with one click

**Mastodon** tried to fix this, but:

- Instance admins can still ban you
- Moving between instances is painful
- Your identity is `user@instance.social`, not yours

**What if your social identity was `yourname.com`?**

Just like email. You can run your own server, or use a provider. You can switch providers. Your address stays the same.

## Protocol Overview

### Identity

```
alice.com       → Alice's page
blog.bob.net    → Bob's page
```

Your domain is your identity. You control DNS. You own it.

### Endpoints (DRAFT)

```http
GET  /osp/v1/profile         # Who am I
GET  /osp/v1/posts           # My posts (paginated)
GET  /osp/v1/posts/{id}      # Single post
POST /osp/v1/subscribe       # Request subscription
GET  /osp/v1/feed            # My feed (authenticated)
```

Standard REST. Standard HTTPS. Works with `curl`.

### Content Format

Posts use [TextBundle](https://textbundle.org/) — an open standard:

```
post-2024-01-15.textbundle/
├── text.md           # Markdown content
├── assets/           # Images, files
└── info.json         # Metadata + signature
```

Every post is a portable document. Export it. Archive it. Read it in 50 years.

### Cryptographic Signatures

```json
{
  "content_hash": "sha256:a1b2c3...",
  "timestamp": "2024-01-15T12:34:56Z",
  "timestamp_signature": "ed25519:9f8e7d...",
  "timestamp_provider": "timeproof.example.com",
  "signature": "ed25519:3c4d5e...",
  "public_key": "ed25519:04a1b2..."
}
```

- **Proof of authorship** — only you can sign with your key
- **Proof of date** — timestamp inside signature
- **Tamper-proof** — can't modify after publishing

## Components

### Server: OpenSocialProfileBox

Reference implementation in **Rust**.

- Full protocol implementation
- SQLite storage (or PostgreSQL)
- Admin web interface
- Runs on a $5 VPS, Raspberry Pi, or your laptop

### Client: Mobile App

Reference implementation in **Flutter**.

- iOS + Android
- Offline-first (preload content, read without internet)
- Key management built-in
- No tracking, no analytics

### Providers (future)

For non-technical users — hosted OpenSocialPage with one-click setup.

Like email: you can run your own server, or use Gmail/Proton/Fastmail.

## What This Is NOT

- **Not a blockchain project** — No mining, no tokens, no gas fees
- **Not a startup** — No VC money, no monetization plan, no exit strategy
- **Not a platform** — It's a _protocol_. Anyone can build clients, servers, and services
- **Not ActivityPub** — Different design choices (domain-centric identity, mandatory signatures)

## Current Status

This is a **draft specification**.

- [ ] Protocol spec v1.0
- [ ] Reference server (Rust)
- [ ] Reference client (Flutter)
- [ ] Developer documentation

We're looking for:

- Protocol reviewers (point out the flaws)
- Rust developers (server implementation)
- Flutter developers (mobile client)
- Security researchers (break our crypto)
- Technical writers (documentation)

## Design Decisions

### Why REST, not ActivityPub?

ActivityPub is great, but:

1. Complex to implement correctly
2. Instance-centric identity (`@user@instance`)
3. No built-in content signing

OSNP is intentionally simpler. A weekend project can implement a basic client.

### Why domains, not DID/blockchain?

Domains are:

- Already decentralized (multiple registrars, multiple TLDs)
- Human-readable
- Legally protected property
- Work with existing infrastructure

Yes, ICANN can revoke TLDs. But your government can also turn off your internet. We optimize for the common case.

### Why mandatory signatures?

In an era of AI-generated content and deepfakes, cryptographic proof of authorship matters.

Every post is signed. Every comment is signed. You can prove you wrote something on a specific date.

## FAQ

**Q: Who pays for hosting?**
A: You. Like email. Or use a free provider (will emerge).

**Q: How do I find people?**
A: Share links. Use directories. Same as the early web.

**Q: What about moderation?**
A: Your page, your rules. Other people's pages, their rules. Block anyone locally. No global moderation.

**Q: What if someone posts illegal content?**
A: They're responsible under their jurisdiction's laws. Just like hosting a website.

**Q: Is this just RSS with extra steps?**
A: RSS doesn't have identity, subscriptions, comments, likes, or signatures. OSNP does.

## Get Involved

This is day one. Everything is up for discussion.

- Read the [whitepaper](./whitepaper.md)
- Open an issue to discuss the protocol
- Submit a PR to improve the spec

No Discord. No Telegram. Just GitHub issues.

## Quick Start

### For Developers

```bash
# Clone the monorepo
git clone https://github.com/yourusername/opensocialnetwork
cd opensocialnetwork

# Build the server
cd server
cargo build --release

# Run the server
cargo run -- init --domain yourdomain.com
cargo run -- serve

# Build the mobile app
cd ../mobile
flutter pub get
flutter run
```

### For Users

1. **Option A: Self-host** — Run your own server (requires technical skills)
2. **Option B: Use a provider** — Sign up with a hosting provider (coming soon)
3. **Download the app** — iOS/Android client to connect to any OSNP server

## Philosophy

The web was built by people who shared protocols, not platforms.

Email works because SMTP is open.
The web works because HTTP is open.
Social networking should work the same way.

**Own your domain. Own your data. Own your voice.**

_OpenSocialNetwork Protocol (OSNP) — MIT License_

_This is not a product. This is a protocol. Build on it._
