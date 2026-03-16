# 🔐 crypto-algorithms-python

> *From classical ciphers to modern cryptography — a complete Python implementation*

A personal collection of **cryptographic algorithms implemented from scratch in Python**, covering classical ciphers, symmetric encryption, asymmetric encryption, hash functions, key exchange protocols, digital signatures, and a real-world secure client-server application.

---

## ⚠️ Disclaimer

```
All implementations are developed strictly for educational and research purposes.
Do NOT use these implementations in production environments.
For production use, rely on battle-tested libraries (cryptography, PyCryptodome).
```

---

## 👩‍💻 Author

**Ghribi Ahlam** — Cybersecurity Engineer · USTHB, Département Sécurité Informatique  
📧 ahlamghribi77@gmail.com · 🐙 [github.com/Ahlamghribi](https://github.com/Ahlamghribi)

---

## 📁 Repository Structure

```
crypto-algorithms-python/
│
├── algorithms/
│   │
│   ├── classical_ciphers.py              # Classical ciphers suite (was: chap1.py)
│   │
│   ├── aes/
│   │   ├── aes.py                        # AES from scratch (S-box, MixColumns, KeySchedule)
│   │   ├── test_aes.py                   # Unit tests
│   │   └── tests.py                      # Additional tests
│   │
│   ├── des/
│   │   ├── DES.py                        # DES main class
│   │   ├── feistel.py                    # Feistel network
│   │   ├── Round.py                      # DES round function
│   │   ├── Mixer.py                      # XOR mixer
│   │   ├── PBox.py                       # Permutation boxes (PC1, PC2, IP, FP)
│   │   ├── SBox.py                       # 8 substitution boxes
│   │   ├── Swapper.py                    # Half-block swapper
│   │   ├── NoneSwapper.py                # No-op swapper (last round)
│   │   ├── utils.py                      # Binary conversion utilities
│   │   └── test_des.py                   # DES tests
│   │
│   ├── triple_des.py                     # 3DES (EDE mode)
│   ├── rc4.py                            # RC4 stream cipher (verbose)
│   ├── feistel.py                        # Generic Feistel cipher (ECB/CBC, file I/O)
│   │
│   ├── rsa.py                            # RSA from scratch (keygen, encrypt, decrypt)
│   ├── ecc_cryptography.py               # ECC over finite field ℤ/17ℤ
│   ├── diffie.py                         # Diffie-Hellman key exchange
│   │
│   ├── dsa_scratch.py                    # DSA from scratch (Miller-Rabin)  (was: dsa.py)
│   ├── dsa_signature.py                  # DSA with PyCryptodome (FIPS 186-4)
│   ├── rsa_digital_signature.py          # RSA signatures (PKCS#1 + SHA-256)
│   │
│   ├── sha256_hash.py                    # SHA-256
│   ├── md5_hash.py                       # MD5
│   ├── blake3_hash.py                    # BLAKE3
│   ├── md5_internals.py                  # MD5 step-by-step walkthrough (was: algorithme de fonctionnement md5.py)
│   │
│   ├── secure-chat/
│   │   ├── server.py                     # Multithreaded server (RSA + AES-GCM)
│   │   └── client.py                     # Client (RSA handshake + AES-GCM messaging)
│   │
│   └── files/
│       ├── test.txt                      # Sample plaintext for Feistel cipher
│       └── encrypted.txt                 # Encrypted output sample
│
├── report-advanced-crypto.pdf            # Advanced cryptography report
├── report-crypto-general.pdf             # General cryptography report
└── README.md
```

---

## 🗂️ Algorithms Overview

### 1. Classical Ciphers — `classical_ciphers.py`

From historical ciphers to cryptanalysis techniques, all implemented from scratch:

| Algorithm | Description |
|-----------|-------------|
| **Caesar Cipher** | Shift cipher with configurable shift, encrypt/decrypt |
| **Random Substitution** | Full alphabet substitution with random key generation |
| **Affine Cipher** | y = (ax + b) mod 26, modular inverse for decryption |
| **Hill Cipher** | Matrix-based cipher using numpy (2×2 key matrix) |
| **Playfair Cipher** | Digraph substitution using 5×5 keyword matrix |
| **Vigenère Cipher** | Polyalphabetic cipher with repeating keyword |
| **One-Time Pad (OTP)** | Theoretically unbreakable with truly random key |
| **Frequency Analysis** | Statistical letter frequency counting |
| **Kasiski Test** | Repeated sequence detection for Vigenère key length |
| **Probable Word Method** | Known-plaintext cryptanalysis |
| **Index of Coincidence** | Language detection metric for cipher identification |

---

### 2. Symmetric Encryption

#### AES — `aes/aes.py`
Full AES implementation **from scratch**, without any crypto library:

```
Key sizes    : 128 / 192 / 256 bits
Block size   : 128 bits
Components   : S-Box, Inverse S-Box, Key Schedule,
               SubBytes, ShiftRows, MixColumns, AddRoundKey
Rounds       : 10 / 12 / 14 (depending on key size)
```

#### DES — `des/`
Modular DES implementation with fully separated components:

```
Key size     : 56 bits (64-bit input with parity)
Block size   : 64 bits
Rounds       : 16 Feistel rounds
Components   : PBox (PC1, PC2, IP, FP), SBox (8 boxes),
               Mixer (XOR + expand), Round, Feistel network
Methods      : encrypt/decrypt number, encrypt/decrypt message
```

#### Triple DES — `triple_des.py`
3DES in EDE (Encrypt-Decrypt-Encrypt) mode with 3 independent keys.

#### RC4 — `rc4.py`
Stream cipher with verbose mode showing per-byte KSA and PRGA steps:

```
Components   : KSA (Key Scheduling Algorithm)
               PRGA (Pseudo-Random Generation Algorithm)
Output       : Detailed per-character XOR table (index | P | K | C)
```

#### Feistel Cipher — `feistel.py`
Generic Feistel-based file encryption with mode selection:

```
Rounds       : 8
Block size   : 64 bits
Modes        : ECB, CBC
I/O          : File-based (encrypt/decrypt text files)
CLI          : -e/-d -m <mode> -t <file> -k <key> -o <outfile>
```

---

### 3. Asymmetric Encryption

#### RSA — `rsa.py`
Complete RSA implementation from scratch:

```python
# Key generation pipeline
p, q  → random primes (100–500)
n     = p × q
φ(n)  = (p-1)(q-1)
e     → coprime with φ(n)
d     → modular inverse of e (Extended Euclidean algorithm)

# Encryption / Decryption
C = M^e mod n
M = C^d mod n
```

#### ECC — `ecc_cryptography.py`
Elliptic Curve Cryptography over finite field **ℤ/17ℤ**:

```
Curve        : y² ≡ x³ + ax + b (mod 17)
Operations   : Point addition, Point doubling, Scalar multiplication
Validation   : Curve validity (4a³ + 27b² ≠ 0), Point on curve check
Special case : Point at Infinity (identity element)
Modular inv  : Fermat's little theorem
```

#### Diffie-Hellman — `diffie.py`
Classic DH key exchange with message encryption:

```
Public params : g = 197, p = 151
Parties       : Alice (a = 199), Bob (b = 157)
Shared secret : K = g^(ab) mod p
Encryption    : Caesar-style XOR with shared key
```

---

### 4. Digital Signatures

#### DSA from scratch — `dsa_scratch.py`
Full DSA implementation with Miller-Rabin primality test:

```
Key sizes    : L = 1024 bits (p), N = 160 bits (q)
Primality    : Miller-Rabin probabilistic test
Parameters   : p, q, g per FIPS 186 structure
Hash         : SHA-1 (via hashlib)
Operations   : Key generation, Sign, Verify
```

#### DSA (Secure) — `dsa_signature.py`
Production-grade DSA using PyCryptodome:

```
Key sizes    : L = 2048 bits, N = 256 bits (FIPS 186-4)
Library      : PyCryptodome (getPrime, inverse)
Domain params: p, q, g generated with cryptographic security
```

#### RSA Digital Signature — `rsa_digital_signature.py`

```
Key size     : 2048 bits
Scheme       : PKCS#1 v1.5
Hash         : SHA-256
Operations   : generate_keys, create_signature, verify_signature
```

---

### 5. Hash Functions

| File | Algorithm | Output |
|------|-----------|--------|
| `sha256_hash.py` | SHA-256 | 256-bit hex digest |
| `md5_hash.py` | MD5 | 128-bit hex digest |
| `blake3_hash.py` | BLAKE3 | 256-bit hex digest |
| `md5_internals.py` | MD5 step-by-step | Internal walkthrough |

---

### 6. Secure Client-Server Chat — `secure-chat/`

A real-world secure communication application combining RSA and AES:

```
Protocol     : Hybrid encryption (RSA for key exchange, AES-GCM for messages)
Transport    : TCP sockets (port 5555)
Concurrency  : Multithreaded server (one thread per client)
```

**Handshake & Encryption Flow:**

```
[1] Server generates RSA-2048 key pair
[2] Server → Client  : RSA public key (PEM)
[3] Client → Server  : RSA public key (PEM)
[4] Client generates AES-256 session key
[5] Client → Server  : AES key encrypted with server RSA public key (OAEP/SHA-256)
[6] Server decrypts session key with RSA private key
────────────────────────────────────────────────────────────
[7] All subsequent messages: AES-256-GCM
    Packet format: nonce (16B) | tag (16B) | ciphertext
    New nonce generated per message
```

---

## 🔄 Renaming Reference

| Original name | Renamed to | Reason |
|---|---|---|
| `LES ALGORITHMES/` | `algorithms/` | Lowercase, no spaces — GitHub convention |
| `client-server application for...` | `secure-chat/` | Too long for a folder name |
| `Files/` | `files/` | Lowercase convention |
| `des/des/` (nested) | `des/` | Redundant double nesting |
| `chap1.py` | `classical_ciphers.py` | Descriptive name |
| `dsa.py` | `dsa_scratch.py` | Distinguishes from `dsa_signature.py` |
| `algorithme de fonctionnement md5.py` | `md5_internals.py` | No spaces, meaningful name |
| `RAPPORT_DU_PROJET_ADV_CRYPTO.pdf` | `report-advanced-crypto.pdf` | Lowercase, hyphenated |
| `RAPPORT_PROJECT_CRYPTO_GENERAL.pdf` | `report-crypto-general.pdf` | Lowercase, hyphenated |

---

## 🧪 Environment & Dependencies

```
Language : Python 3.12
```

```bash
pip install pycryptodome cryptography blake3 numpy
```

| Library | Used by |
|---------|---------|
| `pycryptodome` | `dsa_signature.py`, `rsa_digital_signature.py` |
| `cryptography` | `secure-chat/server.py`, `secure-chat/client.py` |
| `blake3` | `blake3_hash.py` |
| `numpy` | `rsa.py`, `classical_ciphers.py` (Hill cipher) |
| `hashlib` | `sha256_hash.py`, `md5_hash.py`, `dsa_scratch.py` |

---

## 🚀 Usage Examples

```bash
# Classical ciphers
python algorithms/classical_ciphers.py

# AES
python algorithms/aes/aes.py

# DES
python algorithms/des/test_des.py

# RC4 (verbose)
python algorithms/rc4.py

# Feistel cipher (file encryption)
python algorithms/feistel.py -e -m cbc -t files/test.txt -k mysecretkey -o files/encrypted.txt

# RSA from scratch
python algorithms/rsa.py

# Diffie-Hellman
python algorithms/diffie.py

# ECC
python algorithms/ecc_cryptography.py

# DSA
python algorithms/dsa_scratch.py

# RSA digital signature
python algorithms/rsa_digital_signature.py

# Secure chat (two terminals)
python algorithms/secure-chat/server.py
python algorithms/secure-chat/client.py

# Hash functions
python algorithms/sha256_hash.py
python algorithms/md5_hash.py
python algorithms/blake3_hash.py
```

---

## 🔗 References

- [FIPS 197 — AES Standard](https://csrc.nist.gov/publications/detail/fips/197/final)
- [FIPS 46-3 — DES Standard](https://csrc.nist.gov/publications/detail/fips/46/3/final)
- [FIPS 186-4 — DSA Standard](https://csrc.nist.gov/publications/detail/fips/186/4/final)
- [RFC 3447 — RSA PKCS#1](https://datatracker.ietf.org/doc/html/rfc3447)
- [RFC 7748 — Elliptic Curves](https://datatracker.ietf.org/doc/html/rfc7748)
- [Diffie-Hellman — Original Paper](https://ee.stanford.edu/~hellman/publications/24.pdf)
- [BLAKE3 Specification](https://github.com/BLAKE3-team/BLAKE3-specs)
- [PyCryptodome Docs](https://pycryptodome.readthedocs.io/)
- [Python cryptography Docs](https://cryptography.io/en/latest/)
