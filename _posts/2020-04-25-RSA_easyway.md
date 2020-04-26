---
layout: post
title: "RSA algorithm (asymmetric algorithm)"
date: 2020-04-25 20:10:00 +0300
tags: Asymmetric algorithm, Public key Cryptography
category: Tutorial
author: Poonam Agarwal
---

### What is RSA algorithm?

    RSA(Rivest–Shamir–Adleman) is an algorithm to encrypt and decrypt messages. It is an asymmetric cryptographic algorithm. There is a pair of public and private key, the message is encrypted with private key of sender and decrypted by the shared public key of the sender.

### Example:

    -   EncryptionKey => (5,14)
        Suppose a text to be encrypted is letter 'B' ->  2, then  2^5(mod 14)
        = 32(mod 14) = 4 (cyphertext). Where (e,N) that is 2^e(mod N) => (5,14)
        is encryption key


        DecriptionKey: (11, 14),
        4^11(mod 14) => 2 == B, where (d,N) that is 4^d(mod N) => (11, 14) is
        decription key

    -   How does it work, there is no magic it is just simple mathematics:
        Take two prime numbers (numbers can be hundreds & hundreds digit long),
        I choose small number (for better explanation) (2,7) where p = 2, q = 7
        -   N = p * q => N = 14

        -   choose φ(N) = (p-1)(q-1)

        -   Now choose a number for encryption e where
            { 1 < e < φ(N) and coprime with N and φ(N)}
            1 < e < 6 =  2, 3, 4, 5
            number which is coprime with 14 and 6 is 5
            so our lock is (e,N) = (5,14)

        -   Now for decryption we choose d such that {d.e(mod φ(N))}
            that is -> d.5(mod 6)
            the options could be -> 1.5(mod 6), 2.5(mod 6), 3.5(mod 6),
            4.5(mod 6),5.5(mod 6), 6.5(mod 6), 7.5(mod 6), 8.5(mod 6),
            9.5(mod 6), 10.5(mod 6), 11.5(mod 6), 12.5(mod 6) and so on ...
            I choose 11.5(mod 6) which gives me the value 1.

            So the decription key is (11, 14)
