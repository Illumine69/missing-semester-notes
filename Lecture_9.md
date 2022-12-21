# Lecture 9: Security and Cryptography

Lec Notes: [https://missing.csail.mit.edu/2020/security/](https://missing.csail.mit.edu/2020/security/)

- A cryptographic hash function maps data of arbitrary size to a fixed size, and has some special properties.
- An example of a hash function is SHA1, which is used in Git.

```bash
$ printf 'hello' | sha1sum
aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d
$ printf 'hello' | sha1sum
aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d
$ printf 'Hello' | sha1sum
f7ff9e8b7bb2e09b70935a5d785e0cc5d9d0abf0
```

## Symmetric cryptography

```bash
keygen() -> key  (this function is randomized)

encrypt(plaintext: array<byte>, key) -> array<byte>  (the ciphertext)
decrypt(ciphertext: array<byte>, key) -> array<byte>  (the plaintext)
```

Correctness property of decrypt function: `decrypt(encrypt(message, key), key) = message`

## Asymmetric cryptography

```shell
keygen() -> (public key, private key)  (this function is randomized)

encrypt(plaintext: array<byte>, public key) -> array<byte>  (the ciphertext)
decrypt(ciphertext: array<byte>, private key) -> array<byte>  (the plaintext)

sign(message: array<byte>, private key) -> array<byte>  (the signature)
verify(message: array<byte>, signature: array<byte>, public key) -> bool  (whether or not the signature is valid)
```

Correctness property of decrypt function: `decrypt(encrypt(message, public key), private key) = message`

Correctness property of verify function: `verify(message,sign(message, private key), public key) = true`
