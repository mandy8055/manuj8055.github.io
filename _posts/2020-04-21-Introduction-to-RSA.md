---
title: How RSA algorithm works
tags: [Cryptography, Algorithms, CTF]
style:
color:
description: Understanding the working of RSA algorithm and implementing it to capture the flag involving RSA decryption problems.
comments: true
---
<a class="text-center" href="https://feedburner.google.com/fb/a/mailverify?uri=Mandy8055&amp;loc=en_US" onclick="window.open(this.href, 'subscribe',
    'left=20,top=20,width=500,height=500,toolbar=1,resizable=0'); return false;">Subscribe for New Posts</a>

Cryptography is a method of protecting information and communications through the use of codes, so that only the intended users can read and process it. With the increase in cyber crimes these days,  there is a dire need of more and more secure ways to protect information. One of the successful algorithms which even prevails in today's world and is used almost in every plethora of virtual world is **RSA algorithm** which is named after its creators Ron Rivest, Adi Shamir and Leonard Adleman. It is one of the most secure algorithms and it is believed that only the quantum computers can break this algorithm in a feasible way.

In this blog post, I will discuss the working of this algorithm and how you can use this understanding in order to solve the CTF problems which are based on RSA encryption and decryption.

# How RSA works 
Let us understand this with the help of simple example and a communication medium and then we will try to break down each part of it in detail.

Let us assume Bob wants to send the plain text(P) **B** to Alice.
* **B** will be converted to integer. For simplicity let us assume that **B** is converted to **2**(its place value in letters).
* Alice gives the two numbers to Bob. The numbers are e=5 and N=14(Stay with me, I will explain why we chose e and N). Here e and N are **public key**. Public key is the key which can be shared with the outside world.
* Bob calculates the cipher text(C) using the function **_C = P<sup>e</sup>(mod N)_** i.e. 2<sup>5</sup>(mod 14) = 4 which is equivalent to **D** and sends it to Alice.
* Now Alice got the cipher text message as **D**. So Alice wants to decrypt it in order to get the plain text.
* Alice uses her secret key to decrypt the message. She uses d=11 and N=14. Here d and N are the **private key**. Private key is the key which is generated and kept secretely from the outside world.
* Alice calulates the plain text(P) message using the function **_P = C<sup>d</sup>(mod N)_** i.e. 4<sup>11</sup>(mod 14) = 2 which is equivalent to **B**.

Above are the steps how RSA works. Now, it's the time to unravel the mysteries behind the selection of values for e,N and d.

# Math behind the RSA Algorithm

Let us see step by step how the math behind the algorithm works.

* **STEP 1**: Choose two prime numbers p and q arbitrarily. I chose p = 2 and q = 7. In real-time applications, these prime numbers are humongous(minimum 2048 bits each).
* **STEP 2**: Take the product of p and q. The **result** is our number N i.e. 14.
* **STEP 3**: Calculate Φ(N). Write the numbers from 1 till N i.e. 14 and cross out all the numbers which are the factors of 14(except 1) and multiples of the factors of 14. Sounds confusing:thinking:. Let's get the thing straight. Factors of 14 are 2 and 7. Multiples of 2 and 7 less than or equal to 14 are 4, 6, 8, 10, 12 and 14. Crossing out all these numbers we are left with 1, 3, 5, 9, 11 and 13. These numbers are [**co-prime**](https://en.wikipedia.org/wiki/Coprime_integers) with 14.
    * This method is infeasible. Because, let's assume that if N is very large, then we have to write all the numbers from 1 to N and then cross-out the factors and their multiples. Luckily, we have something called mathemagic(I meant mathematics). One of the greatest mathemagician of all times [Leonhard Euler](https://en.wikipedia.org/wiki/Leonhard_Euler) gave his totient function i.e. Φ(N) = (p - 1)*(q - 1). Math is magic.
* **STEP 4**: Select e such that;
    * 1 < e < Φ(N)
    * e should be co-prime to N and Φ(N). Therefore, **e = 5**.
* **STEP 5**: Calculate d such that; e.d(mod Φ(N)) = 1. WHY and HOW TO CALCULATE???
    * WHY is the essence of the RSA algorithm and HOW TO CALCULATE is by using [Extended Euclid's algorithm](https://brilliant.org/wiki/extended-euclidean-algorithm/)
    * d = 11.

# CTF problems based on RSA algorithm

I have already discussed about CTF's in my very [first post](https://mandy8055.github.io/blog/cipher-combat-beginners-2020). In the problems relating to RSA decryption, you'll be given a text file which contains e, N and C. Your task is to find the flag by decrypting the cipher text C.

For instance we are given; 
* e=3
* N = 245841236512478852752909734912575581815967630033049838269083 and 
* C = 219878849218803628752496734037301843801487889344508611639028

Now, Let us use our understanding in order to figure out the solution. We need to calculate p and q in order to calulate Φ(N). There is an [online platform](http://factordb.com/) which is very handy in figuring out primes p and q for us. Put N in there and factorize it in order to get p and q.

{% include elements/figure.html image="https://mandy8055.github.io/assets/2020-04-22-factor-db.png" caption="Factor db result" %}

Now, that we have p, q, e, n and C. Let us code it out to get **d** and our plain text **P**. Python's **crypto** and **gympy2** libraries do the task easier for us by obfuscating the complexity. However, if you wish you can implement the algorithm using above steps from scratch.

```python
from Crypto.PublicKey import RSA
import gmpy2

# For converting the integer result to its text form
def int2Text(number, size):
    text = "".join([chr((number >> j) & 0xff) for j in reversed(range(0, size << 3, 8))])
    return text.lstrip("\x00")

N=245841236512478852752909734912575581815967630033049838269083
C=219878849218803628752496734037301843801487889344508611639028
p=416064700201658306196320137931
q=590872612825179551336102196593
phi_n=(p-1)*(q-1)
e=3L
# Calculation of the private key
d = long(gmpy2.divm(1, e, phi_n))
# Constructing the RSA 
rsa = RSA.construct((N,e,d,p,q))
# Decrypting the cipher text using RSA object
plaintext = rsa.decrypt(C)

print plaintext  # returns 142327357830311032682897538242625561166817486779261
print int2Text(plaintext,100) #returns abctf{rs4_is_aw3s0m3}
```
Bingo; we captured the flag.

# Conclusion
The compexity of RSA algorithm lies in the fact that the **prime factorization of large number is hard**. This is the only reason why RSA is still prevelant and is used in every area across virtual world. However, RSA algorithm is not impenetrable. Quantum computers can crack it very efficiently. You can watch [this](https://www.youtube.com/watch?v=6H_9l9N3IXU) amazing youtube video on further elaboration.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}