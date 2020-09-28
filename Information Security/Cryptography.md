# Cryptography

## What is Cryptography?

- Cryptography provides for secure communication in the presence of malicious third-parties—known as adversaries. Encryption uses an algorithm and a key to transform an input (i.e., plaintext) into an encrypted output (i.e., ciphertext). A given algorithm will always transform the same plaintext into the same ciphertext if the same key is used.

- Algorithms are considered secure if an attacker cannot determine any properties of the plaintext or key, given the ciphertext. An attacker should not be able to determine anything about a key given a large number of plaintext/ciphertext combinations which used the key.

## What is the difference between symmetric and asymmetric cryptography?

- With symmetric cryptography, the same key is used for both encryption and decryption. A sender and a recipient must already have a shared key that is known to both. Key distribution is a tricky problem and was the impetus for developing asymmetric cryptography.

- With asymmetric crypto, two different keys are used for encryption and decryption. Every user in an asymmetric cryptosystem has both a public key and a private key. The private key is kept secret at all times, but the public key may be freely distributed.

- Data encrypted with a public key may only be decrypted with the corresponding private key. So, sending a message to John requires encrypting that message with John’s public key. Only John can decrypt the message, as only John has his private key. Any data encrypted with a private key can only be decrypted with the corresponding public key. Similarly, Jane could digitally sign a message with her private key, and anyone with Jane’s public key could decrypt the signed message and verify that it was in fact Jane who sent it.

- Symmetric is generally very fast and ideal for encrypting large amounts of data (e.g., an entire disk partition or database). Asymmetric is much slower and can only encrypt pieces of data that are smaller than the key size (typically 2048 bits or smaller). Thus, asymmetric crypto is generally used to encrypt symmetric encryption keys which are then used to encrypt much larger blocks of data. For digital signatures, asymmetric crypto is generally used to encrypt the hashes of messages rather than entire messages.

- A cryptosystem provides for managing cryptographic keys including generation, exchange, storage, use, revocation, and replacement of the keys.

## What problems does cryptography solve?

- A secure system should provide several assurances such as confidentiality, integrity, and availability of data as well as authenticity and non-repudiation. When used correctly, crypto helps to provide these assurances. Cryptography can ensure the confidentiality and integrity of both data in transit as well as data at rest. It can also authenticate senders and recipients to one another and protect against repudiation.

- Software systems often have multiple endpoints, typically multiple clients, and one or more back-end servers. These client/server communications take place over networks that cannot be trusted. Communication occurs over open, public networks such as the Internet, or private networks which may be compromised by external attackers or malicious insiders.

- It can protect communications that traverse untrusted networks. There are two main types of attacks that an adversary may attempt to carry out on a network. Passive attacks involve an attacker simply listening on a network segment and attempting to read sensitive information as it travels. Passive attacks may be online (in which an attacker reads traffic in real-time) or offline (in which an attacker simply captures traffic in real-time and views it later—perhaps after spending some time decrypting it). Active attacks involve an attacker impersonating a client or server, intercepting communications in transit, and viewing and/or modifying the contents before passing them on to their intended destination (or dropping them entirely).

- The confidentiality and integrity protections offered by cryptographic protocols such as SSL/TLS can protect communications from malicious eavesdropping and tampering. Authenticity protections provide assurance that users are actually communicating with the systems as intended. For example, are you sending your online banking password to your bank or someone else?

- It can also be used to protect data at rest. Data on a removable disk or in a database can be encrypted to prevent disclosure of sensitive data should the physical media be lost or stolen. In addition, it can also provide integrity protection of data at rest to detect malicious tampering.

## What are the principles?

- The most important principle to keep in mind is that you should never attempt to design your own cryptosystem. The world’s most brilliant cryptographers (including Phil Zimmerman and Ron Rivest) routinely create cryptosystems with serious security flaws in them. In order for a cryptosystem to be deemed “secure,” it must face intense scrutiny from the security community. Never rely on security through obscurity, or the fact that attackers may not have knowledge of your system. Remember that malicious insiders and determined attackers will attempt to attack your system.

- The only things that should be “secret” when it comes to a secure cryptosystem are the keys themselves. Be sure to take appropriate steps to protect any keys that your systems use. Never store encryption keys in clear text along with the data that they protect. This is akin to locking your front door and placing the key under the doormat. It is the first place an attacker will look. Here are three common methods for protecting keys (from least secure to most secure):

  1. Store keys in a filesystem and protect them with strong access control lists (ACLs). Remember to adhere to the principal of least privilege.
  2. Encrypt your data encryption keys (DEKs) with a second key encrypting key (KEK). The KEK should be generated using password-based encryption (PBE). A password known to a minimal number of administrators can be used to generate a key using an algorithm such as bcrypt, scrypt, or PBKDF2 and used to bootstrap the cryptosystem. This removes the need to ever store the key unencrypted anywhere.
  3. A hardware security module (HSM) is a tamper-resistant hardware appliance that can be used to store keys securely. Code can make API calls to an HSM to provide keys when needed or to perform decryption of data on the HSM itself.

- Make sure that you only use algorithms, key strengths, and modes of operation that conform to industry best practices. Advanced encryption standard (AES) (with 128, 192, or 256-bit keys) is the standard for symmetric encryption. RSA and elliptical curve cryptography (ECC) with at least 2048-bit keys are the standard for asymmetric encryption. Be sure to avoid insecure modes of operation such as AES in Electronic Codebook (ECB) mode or RSA with no padding.
