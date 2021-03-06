---
layout: "docs"
page_title: "Vault Enterprise Entropy Augmentation"
sidebar_title: "Entropy Augmentation"
sidebar_current: "docs-vault-enterprise-entropy-augmentation"
description: |-
  Vault Enterprise features a mechanism to sample entropy from external
  cryptographic modules.
---

# Entropy Augmentation

Vault Enterprise features a mechanism to sample entropy (or randomness for 
cryptographic operations) from external cryptographic modules via the [seals](/docs/configuration/seal/index.html)
interface. While the system entropy used by Vault is more than capable of
operating in most threat models, there are some situations where additional
entropy from hardware-based random number generators is desirable.

To use this feature, you must have an active or trial license for Vault
Enterprise. To start a trial, contact [HashiCorpsales](mailto:sales@hashicorp.com).

# Critical Security Parameters (CSPs)

Entropy augmentation allows Vault Enterprise to supplement its system entropy with
entropy from an external cryptography module. Designed to operate in environments
where alignment with cryptographic regulations like [NIST SP800-90B](https://csrc.nist.gov/publications/detail/sp/800-90b/final)
is required or when augmented entropy from external sources such as hardware true
random number generators (TRNGs) or [quantum computing TRNGs](https://www.hashicorp.com/blog/quantum-security-and-cryptography-in-hashicorp-vault/)
are desirable, augmented entropy replaces system entropy when performing random
number operations on critical security parameters (CSPs). 

These CSPs have been selected from our previous work in [evaluating Vault for conformance with
FIPS 140-2 guidelines for key storage and key transport](https://www.datocms-assets.com/2885/1510600487-vault_compliance_letter_fips_140-2.pdf)
and include the following: 


- Vault’s master key 
- Keyring encryption keys
- Auto Unseal recovery keys
- TLS private keys for inter-node and inter cluster communication (HA leader, raft, and replication)
- Enterprise MFA TOTP token keys
- JWT token wrapping keys
- Root tokens
- DR operation tokens
- [Transit](/docs/secrets/transit/index.html) backend key generation

## Enabling/Disabling

Entropy augmentation is disabled by default. To enable entropy augmentation Vault's
[configuration file][configuration] must include a properly configured [PKCS#11 seal and
entropy stanza](/docs/configuration/entropy-augmentation/index.html).

[configuration]: /docs/configuration/index.html
