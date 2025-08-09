
| Acronym    | Stands for                                     | Short description                                                                                                                         |
| ---------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **CNG**    | Cryptography Next Generation                   | The Windows cryptographic API framework introduced in Vista, used for modern crypto algorithms and providers (replaces legacy CryptoAPI). |
| **KSP**    | Key Storage Provider                           | A CNG component that stores keys; the **Microsoft Platform Crypto Provider** is a KSP that stores keys in the TPM.                        |
| **NCrypt** | (literal for "Encrypt")                        | The C API prefix for functions in `ncrypt.dll` that manage keys and providers under CNG (e.g., `NCryptEnumKeys`).                         |
| **PCR**    | Platform Configuration Register                | Special TPM registers that store measurements (hashes) of firmware, boot loaders, etc., used for attestation and policy enforcement.      |
| **HSM**    | Hardware Security Module                       | A dedicated secure device for key management; TPM is a form of HSM but smaller in scope.                                                  |
| **ACL**    | Access Control List                            | A list of permissions associated with an object (e.g., keys), defining who can access or modify it.                                       |
| **ADCS**   | Active Directory Certificate Services          | Microsoft’s certificate authority system, which can work with TPM key attestation for cert issuance.                                      |
| **RSA**    | Rivest–Shamir–Adleman                          | An asymmetric encryption/signature algorithm commonly supported by TPMs.                                                                  |
| **ECDSA**  | Elliptic Curve Digital Signature Algorithm     | A digital signature algorithm using elliptic curves, supported by TPMs for efficient, strong signing.                                     |
| **AES**    | Advanced Encryption Standard                   | Symmetric encryption algorithm widely used for securing data; standardized by NIST.                                                       |
| **CBC**    | Cipher Block Chaining                          | A block cipher mode that chains each block’s encryption to the previous block’s ciphertext for added security.                            |
| **CTR**    | Counter Mode                                   | A block cipher mode that turns a block cipher into a stream cipher using incrementing counters.                                           |
| **CFB**    | Cipher Feedback Mode                           | A block cipher mode that feeds back previous ciphertext to encrypt the next block; allows partial block processing.                       |
| **GCM**    | Galois/Counter Mode                            | A block cipher mode that provides both encryption and message authentication (AEAD).                                                      |
| **MAC**    | Message Authentication Code                    | A short piece of information that verifies data integrity and authenticity using a shared secret.                                         |
| **HMAC**   | Hash-based Message Authentication Code         | A MAC that uses a cryptographic hash function and a shared secret key.                                                                    |
| **IV**     | Initialization Vector                          | A non-secret, unique value used to randomize encryption to prevent pattern analysis.                                                      |
| **PKI**    | Public Key Infrastructure                      | A framework for managing public-key encryption, certificates, and trust chains.                                                           |
| **CA**     | Certificate Authority                          | An entity that issues and manages digital certificates to bind public keys to identities.                                                 |
| **CSR**    | Certificate Signing Request                    | A request sent to a CA containing a public key and identifying information for certificate issuance.                                      |
| **OCSP**   | Online Certificate Status Protocol             | A protocol for checking the revocation status of an X.509 digital certificate in real time.                                               |
| **CRL**    | Certificate Revocation List                    | A list published by a CA that contains revoked digital certificates.                                                                      |
| **TLS**    | Transport Layer Security                       | The protocol securing data in transit over the internet (successor to SSL).                                                               |
| **SSL**    | Secure Sockets Layer                           | Predecessor to TLS; now considered obsolete but still used as a generic term for HTTPS security.                                          |
| **FIPS**   | Federal Information Processing Standards       | US government standards for cryptography and IT security, e.g., FIPS 140-2 for crypto modules.                                            |
| **NIST**   | National Institute of Standards and Technology | US federal agency that develops and maintains security and cryptography standards.                                                        |
| **SHA**    | Secure Hash Algorithm                          | A family of cryptographic hash functions (e.g., SHA-256, SHA-3) used in signatures and integrity checks.                                  |
| **PBKDF2** | Password-Based Key Derivation Function 2       | A key stretching algorithm that uses salting and repeated hashing to derive strong keys from passwords.                                   |

## JWT specific acronyms

| Acronym  | Stands for       | Short description                                                                                      |
| -------- | ---------------- | ------------------------------------------------------------------------------------------------------ |
| **JWT**  | JSON Web Token   | A compact, signed token format for securely transmitting claims in JSON between parties.               |
| **JWK**  | JSON Web Key     | A JSON format for representing cryptographic keys, often used in JWKS endpoints for JWT validation.    |
| **JWKS** | JSON Web Key Set | A set (array) of JWKs, usually served via HTTPS so clients can fetch public keys for JWT verification. |

## TPM specific acronyms

| Acronym             | Stands for                             | Short description                                                                                                                                          |
| ------------------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **TPM**             | Trusted Platform Module                | A hardware chip that securely stores cryptographic keys, measures system integrity (via PCRs), and performs secure operations like signing and encryption. |
| **TPM2**            | Trusted Platform Module 2.0            | The second-generation TPM specification, supporting modern algorithms and policies.                                                                        |
| **EK**              | Endorsement Key                        | A unique asymmetric key burned into the TPM at manufacturing, used for identity and attestation.                                                           |
| **AIK**             | Attestation Identity Key               | A key pair created inside the TPM for attestation purposes, certified by the EK or a CA.                                                                   |
| **SRK**             | Storage Root Key                       | The root of the TPM’s storage hierarchy; protects other keys and data stored in the TPM.                                                                   |
| **NV**              | Non-Volatile (storage)                 | Persistent storage inside the TPM for keys, counters, and policies.                                                                                        |
| **PCR**             | Platform Configuration Register        | Special TPM registers holding measurements (hashes) of system state for integrity verification.                                                            |
| **TBS**             | TPM Base Services                      | Windows service and API layer that allows applications to talk to the TPM.                                                                                 |
| **AK**              | Attestation Key                        | Often used interchangeably with AIK; in TPM 2.0 terminology, keys for signing attestation data.                                                            |
| **RK**              | Restricted Key                         | A TPM key whose use is limited to specific operations (e.g., signing only).                                                                                |
| **NV Index**        | Non-Volatile Index                     | An addressable slot in TPM NV storage for data, counters, or authorization policies.                                                                       |
| **TSS**             | TPM Software Stack                     | API/library layer that provides high-level TPM commands for applications.                                                                                  |
| **PCR Bank**        | PCR algorithm set                      | A group of PCRs associated with a specific hash algorithm (e.g., SHA-1 bank, SHA-256 bank).                                                                |
| **PolicyPCR**       | Policy Platform Configuration Register | A TPM policy command that ties key or object use to specific PCR values.                                                                                   |
| **PolicyOR**        | Policy OR                              | A TPM policy command that allows key use if any of several policies match.                                                                                 |
| **PolicyAuthorize** | Policy Authorize                       | A TPM policy command that lets an external signer authorize a policy update.                                                                               |
| **Quote**           | TPM Quote                              | A signed report from the TPM proving the current PCR values for attestation.                                                                               |
| **Seal/Unseal**     | Seal data                              | Encrypting (“sealing”) data to TPM policy/PCR state so it can be “unsealed” only in matching conditions.                                                   |
## NFC & smart card–specific acronyms

|Acronym|Stands for|Short description|
|---|---|---|
|**NFC**|Near Field Communication|A short-range wireless communication standard (13.56 MHz) used for contactless cards and devices.|
|**APDU**|Application Protocol Data Unit|The command/response structure used to communicate with smart cards and NFC secure elements.|
|**SAM**|Secure Access Module|A secure microcontroller (often in card form) used to securely store keys and perform cryptographic operations.|
|**UID**|Unique Identifier|The hardware serial number of an NFC card or tag.|
|**PICC**|Proximity Integrated Circuit Card|ISO/IEC term for a contactless card or tag.|
|**PCD**|Proximity Coupling Device|ISO/IEC term for the NFC reader or terminal.|
|**MIFARE**|(Brand, not an acronym)|A family of contactless cards by NXP, often used in access control and transport.|
|**DESFire**|Data Encryption Standard + Fire|NXP’s high-security contactless card platform supporting AES/3DES, mutual authentication, and file-based storage.|
|**ISO14443**|ISO/IEC 14443|Standard for proximity cards (13.56 MHz) defining communication between PICCs and PCDs.|
|**ISO7816**|ISO/IEC 7816|Standard for contact and contactless smart card commands (includes APDU format).|
|**PIV**|Personal Identity Verification|US federal smart card standard for secure identity credentials.|
|**EMV**|Europay, MasterCard, Visa|Global standard for chip-based payment cards and terminals (supports NFC/contactless).|
|**SE**|Secure Element|A tamper-resistant chip (can be embedded, SIM-based, or card-based) that stores keys and executes secure transactions.|
