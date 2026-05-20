# 📌 Title

Fix SSH Compatibility Issues Using Linux Crypto Policy (LEGACY Mode)

---

## 🎯 Objective

Restore SSH connectivity from a modern Linux system to legacy network devices that use outdated cryptographic algorithms.

---

## 🧱 Environment / Lab Setup

### Devices

* Linux Host (RHEL / Fedora / Ubuntu-based)
* Legacy Network Device (Cisco router/switch, older firmware)

### Tools

* SSH client (OpenSSH)
* System crypto policies (`update-crypto-policies`)

---

## ⚠️ Problem Description

Unable to SSH into legacy devices from a modern Linux system.

### 🔴 Symptoms

* SSH connection fails
* Errors such as:

```text
ssh_dispatch_run_fatal: error in libcrypto
no matching key exchange method found
```

---

## 🔍 Root Cause

Modern Linux distributions disable weak cryptographic algorithms by default, including:

* SHA-1
* Older Diffie-Hellman groups
* RSA < 2048 bits

Legacy devices (especially older Cisco gear) still rely on these.

---

## 🛠️ Troubleshooting Steps

### 🔹 Attempt SSH Connection

```bash
ssh user@device-ip
```

### 🔹 Try Manual Algorithm Override

```bash
ssh -o KexAlgorithms=+diffie-hellman-group14-sha1 user@device-ip
```

If still failing → system crypto policy is blocking it.

---

## ✅ Solution / Fix

### 🔹 Step 1 — Enable LEGACY Crypto Policy

```bash
sudo update-crypto-policies --set LEGACY
```

---

### 🔹 Step 2 — Verify Policy

```bash
update-crypto-policies --show
```

**Expected Output:**

```text
LEGACY
```

---

### 🔹 Step 3 — Restart Services

```bash
sudo systemctl restart sshd
```

---

## 🔐 What the LEGACY Policy Allows

| Setting          | DEFAULT   | LEGACY      |
| ---------------- | --------- | ----------- |
| TLS 1.0          | Disabled  | Enabled     |
| TLS 1.1          | Disabled  | Enabled     |
| SHA-1            | Disabled  | Allowed     |
| Minimum RSA Key  | 2048 bits | 1024 bits   |
| Minimum DH Param | 2048 bits | 1024 bits   |
| 3DES             | Disabled  | Enabled     |
| CBC Mode         | Allowed   | Allowed     |
| DSA Keys         | Disabled  | Enabled     |
| RC4              | Disabled  | Limited use |

---

## 🧪 Verification

* SSH into device successfully:

```bash
ssh user@device-ip
```

* Connection no longer fails
* Able to authenticate and access CLI

---

## ⚠️ Security Warning

LEGACY mode lowers system security.

👉 Only use in:

* Lab environments
* Temporary troubleshooting

---

## 🔄 Improvements / Next Steps

* Upgrade device firmware to support modern crypto
* Configure stronger SSH settings on network devices
* Revert policy after use:

```bash
sudo update-crypto-policies --set DEFAULT
```

---

## 📚 Key Takeaways

* Modern Linux blocks insecure crypto by default
* Legacy devices often require weaker algorithms
* System-wide crypto policies can override SSH behavior
* Always prefer upgrading devices over weakening security

---

## 🧩 Tags

`#ssh #linux #security #cisco #troubleshooting #crypto`

---

