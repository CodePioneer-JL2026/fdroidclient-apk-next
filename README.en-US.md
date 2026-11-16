# fdroidclient-apk-next

🇺🇸 **English (US)** ｜ 🇨🇳 [简体中文](README.zh-CN.md)

F-Droid stopped publishing client APKs on GitHub. This repo keeps the door open.

I'm a middle school student in China, and I want to say this honestly: my technical skills are still limited. I can't patch F-Droid's code, I can't add features, and there are plenty of things in this world of free software that are way above my head. But I've fallen in love with what open source stands for — the idea that software can be free, transparent, and built by strangers helping strangers, just because it's the right thing to do. That idea made me want to give something back, in whatever small way I could.

Here's the part I can play: F-Droid's home is on GitLab, and they no longer publish new releases on GitHub. When the official site or GitLab is unreachable — which happens a lot from where I am — people simply have no way to get the latest client. If you can't even install the store, you never get to meet the thousands of FOSS apps behind it. That felt wrong to me, especially for people like me: students, curious kids, anyone just starting out and trying to learn. So this repo is my answer. I mirror every new official signed F-Droid Android client here, exactly as F-Droid builds and signs it — no modifications, no repackaging, ever. Think of it as an extra door: one more place for the people who can't get through the front one.

## What's in every release

Every time a new official client drops, I upload **two core files, every single time** — plus one official key, kept alongside them for deeper verification:

- `F-Droid_<version>.apk` — the official client, untouched
- `F-Droid_<version>.apk.sha256.txt` — a plain text file with the **file hash** of that exact APK
- `f-droid.org-signing-key.gpg` — the official GPG public key, for advanced release-chain checks (see below)

That `.sha256.txt` is just a text file. Open it on any phone and you'll see one line: the SHA-256 of this build. I keep it as `.txt` on purpose — no fancy no-extension tricks, so it opens on any device and beginners can actually read it.

## About that `.sha256.txt` file (please read this part)

**The hash inside the `.txt` is the APK file's own hash. It is NOT the signing certificate fingerprint. Two completely different things.**

- Every APK build is different, so **this file hash changes with every version.** That's normal — a new version means a new hash.
- It only proves the download wasn't corrupted in transit. **It does not prove who signed the package.**
- To prove it's really F-Droid's official package, you check the certificate separately (see the next section).

Quick check on most computers:

```sh
sha256sum -c F-Droid_*.apk.sha256.txt
```

No `sha256sum` on your phone? Just open the `.txt` and compare the hash inside it with what's shown on this releases page. If they match, the download is intact.

## Verify the signature too (don't skip this)

There are three layers here. For everyday installs, the first two are all you need. The third is the "paranoid's back pocket."

| | File hash (the `.txt`) | Certificate fingerprint (the one that matters) | GPG public key (advanced) |
|---|---|---|---|
| What it proves | Download is intact | Package is really from F-Droid | Release chain wasn't tampered with |
| Changes? | **Every version** | **Never** (valid until 2037) | Key fingerprint stays the same (valid until 2037) |
| How to check | `sha256sum -c` or by eye | `keytool -printcert -jarfile` | `gpg --verify` (needs the `.asc` file) |

The certificate fingerprint to look for is **always** this one — this is what Android checks on install and update:

```
43238d512c1e5eb2d6569f4a3afbf5523418b82e0a3ed1552770abb9a9c9ccab
```

(F-Droid documents this as their long-lived signing certificate, valid until 2037.)

- **Different APK hash?** Normal. Every release has its own.
- **Different certificate?** Stop. **Do not install**, no matter what the filename or file hash says.
- On Linux/macOS:

  ```sh
  keytool -printcert -jarfile F-Droid_*.apk
  ```

  should print that same certificate value. Android itself will also refuse any future update whose signature doesn't match, so getting the very first install right keeps every later update safe.

### Advanced: GPG release signature (optional, but worth knowing about)

The `f-droid.org-signing-key.gpg` shipped in this repo is the official public key. It is **a different key from the Android signing certificate** above — don't mix them up:

- **Android certificate** (middle column above): what Android checks when installing. It stops impersonation. Before 2037, its fingerprint is always `43238d…ccab`.
- **GPG public key** (this section): for verifying things on a computer. It checks the official `.asc` signature file and proves the package came out of F-Droid's own release chain.

**How to use the `.gpg` file:**

1. Import the key once:

   ```sh
   gpg --import f-droid.org-signing-key.gpg
   ```

   The official primary key fingerprint is:

   ```
   37D2 C987 89D8 3119 4839  4E3E 41E7 044E 1DBA 2E89
   ```

   Compare it when you import. Same fingerprint = same key, just renewed.

2. Verify the release signature (you need the official `.asc` file that sits next to the APK):

   ```sh
   gpg --verify F-Droid_<version>.apk.asc F-Droid_<version>.apk
   ```

   `Good signature from "F-Droid ..."` means the release chain is intact.

**⏳ Heads-up about the 2037 expiry:**

This GPG key is valid until **2037**. After that, verification may print `key expired`. That's **normal expiry, not tampering** — the official team renews the key, and an unchanged fingerprint means it's still the same key. For everyday phone installs, the **Android certificate fingerprint** above plus a matching `.sha256.txt` is all you need. GPG is the back pocket, and the key is already here whenever you want it.

## I'll keep this going

This isn't a one-time upload. I'm going to maintain it for as long as I can, through flaky networks and busy school days. I can't promise I'll always be fast, but I can promise I'll keep showing up. New official version out? I'll be here with the APK, its `.sha256.txt`, and that `.gpg` key — same files, same rules.

## One honest note

**I'm not affiliated with or endorsed by F-Droid.** I'm just a kid who got lucky enough to find open source, and who wants more people to find it too. I may be small, my ability may be limited, but this mirror is every bit of effort I've got. If it helps even one person discover free software — one kid who gets curious, one person who learns something new — then every stuck connection and late-night sync was worth it. My dream is just that: to help a few more people get through the door. If you've read this far, maybe you'll pay it forward too, in your own small way. That's how all of us grow. 🤝
