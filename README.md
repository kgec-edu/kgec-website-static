# Kalyani Government Engineering College — Static Maintenance Hub

This repository hosts the official static maintenance and institutional directory portal for **Kalyani Government Engineering College (KGEC)** on **GitHub Pages**.

- **Target Custom Domains**: `kgec.edu.in` & `www.kgec.edu.in`
- **GitHub Repository**: [kgec-edu/kgec-website-static](https://github.com/kgec-edu/kgec-website-static)
- **GitHub Pages Origin**: `kgec-edu.github.io`

---

## 🌐 DNS Setup Guide for `kgec.edu.in`

To route public traffic from `kgec.edu.in` and `www.kgec.edu.in` to this GitHub Pages site, update the DNS records in your DNS provider control panel (e.g., **ClouDNS**, Cloudflare, or Registrar DNS).

### 1. Apex Domain Records (`kgec.edu.in`)
Delete/replace any existing `A` records pointing to previous web server IPs (e.g. `31.97.201.185`) with **GitHub's 4 Anycast `A` records**:

| Host / Name | Record Type | Value / Destination IP | TTL |
| :--- | :---: | :--- | :---: |
| `@` *(or `kgec.edu.in`)* | **`A`** | `185.199.108.153` | `300` (or Auto) |
| `@` *(or `kgec.edu.in`)* | **`A`** | `185.199.109.153` | `300` (or Auto) |
| `@` *(or `kgec.edu.in`)* | **`A`** | `185.199.110.153` | `300` (or Auto) |
| `@` *(or `kgec.edu.in`)* | **`A`** | `185.199.111.153` | `300` (or Auto) |

#### *(Optional but Recommended) IPv6 `AAAA` Records:*
| Host / Name | Record Type | Value / Destination | TTL |
| :--- | :---: | :--- | :---: |
| `@` | **`AAAA`** | `2606:50c0:8000::153` | `300` |
| `@` | **`AAAA`** | `2606:50c0:8001::153` | `300` |
| `@` | **`AAAA`** | `2606:50c0:8002::153` | `300` |
| `@` | **`AAAA`** | `2606:50c0:8003::153` | `300` |

---

### 2. Subdomain Record (`www.kgec.edu.in`)
Add or update the `CNAME` record for `www`:

| Host / Name | Record Type | Target / Value | TTL |
| :--- | :---: | :--- | :---: |
| `www` | **`CNAME`** | `kgec-edu.github.io.` | `300` (or Auto) |

> **Note:** Include the trailing dot (`.`) after `kgec-edu.github.io.` if required by your DNS manager.

---

### 3. How to Verify DNS Propagation

Run the following commands in your terminal to verify that the DNS records have propagated globally:

```bash
# Check Apex domain A records (should return the four 185.199.x.153 IPs)
dig kgec.edu.in +noall +answer

# Check www subdomain CNAME record
dig www.kgec.edu.in CNAME +noall +answer

# Test live HTTP response
curl -ILs https://kgec.edu.in | head -n 20
```

---

### 4. GitHub Pages Custom Domain & SSL Verification

1. The repository includes a [`CNAME`](./CNAME) file with `kgec.edu.in`.
2. Under **GitHub Repository Settings > Pages**:
   - **Source**: Deploy from a branch (`main` / `/ (root)`).
   - **Custom domain**: `kgec.edu.in` (Status will show *DNS check successful* once records propagate).
   - **Enforce HTTPS**: Check this box once the Let's Encrypt certificate is issued (takes ~5–15 minutes after DNS propagation).

---

### 5. 🔄 Rollback Procedure (When Primary Server is Restored)

When the main institutional web portal is ready to go back online:
1. Open your DNS management console (e.g., ClouDNS).
2. Change the `@` `A` records back to your primary server IP (e.g. `31.97.201.185` or new production IP).
3. Update or re-point the `www` record as appropriate.
4. Traffic will automatically divert back to the production server as DNS TTL expires.

---

### 📁 Project Structure

```text
├── CNAME           # GitHub Pages custom domain binding (kgec.edu.in)
├── .nojekyll       # Disables Jekyll processing for raw static file serving
├── index.html      # Main responsive institutional maintenance portal
├── 404.html        # Fallback router ensuring all paths render the maintenance page
├── README.md       # Repository documentation and DNS configuration instructions
├── notices/        # Temporary notice PDFs linked from the homepage "Notices & Announcements" section
│   └── combined-merit-list-btech-2nd-year-ay-2026-27-branch-change.pdf
└── images/         # Self-contained SVG and high-resolution institutional branding assets
    ├── kgec-logo.svg
    ├── kgec_logo.png
    ├── indian-emblem.svg
    └── indian-emblem.webp
```

#### Adding or removing a notice

1. Drop the PDF into `notices/` using a lowercase, hyphenated filename (no spaces).
2. Copy an existing `<li class="notice-item">` block in `index.html` and update the date, title, and `href`.
3. Remove the whole `<!-- 0. Temporary Notice Board -->` section once the primary portal is restored.

---

### Maintained By
**Department of Computer Science & Engineering**  
Kalyani Government Engineering College (KGEC)  
*A Government of West Bengal Institute • Estd. 1995*
