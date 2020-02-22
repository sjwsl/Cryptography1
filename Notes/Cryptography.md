# Cryptography

This is my note of [_Cryptography 1_](https://www.coursera.org/learn/crypto) by **_Dan Boneh_**

[TOC]

## Overview

### Crypto core

- Secret key establishment

- Secure communication

  - confidentially
  - integrity

- Digital signature

- Anonymous communication

- Anonymous **digital** cash

  - Can I spend a **digital coin** without anyone knowing who I am?
  - How to prevent double spending?

- Secure multi-party computation

  - **_Thm:_** anything that can done with trusted authority can also be done without trusted authority

- Privately outsourcing computation

  - Get search result from Google while Google doesn't know what you search for

- [Zero knowledge](https://en.wikipedia.org/wiki/Zero-knowledge_proof)
  - one party (the prover) can prove to another party (the verifier) that they know a value x, **without conveying any information** apart from the fact that they know the value x

### Three steps in cryptography

1. Precisely specify threat model

   - What an attacker can do
   - What an attacker's goal is

2. Propose a construction

3. Prove that breaking construction under threat mode will solve an underlying hard problem

### History

#### Substitution Cipher

- Use frequency of English letters and pairs of letters to easily break it

#### Vigener Cipher

- The key is a word
- Just break it as substitution cipher

#### Roter Machines

- Early example: the Hebern machine (single rotor)
- Most famous: the Enigma (3-5 rotors)
  - Designed to defend frequency attack (statistic attack)
  - keys = $26^4$ = $2^{18}$
  - Still can't defend the ciphertext only attack

#### [Data Encryption Standard](https://en.wikipedia.org/wiki/Data_Encryption_Standard)

- DES: #keys = $2^{56}$ , block size = 64 bits
- Today: AES(2001), Salsa20(2008) (and many others)

### [Randomized algorithm](https://en.wikipedia.org/wiki/Randomized_algorithm)

- Deterministic algorithm: $y \leftarrow A(m)$
  - output is a deterministic value
- Randomized algorithm: $y \leftarrow A(m;r)$ where $r \stackrel{R}{\leftarrow} \{0,1\}^n$
  - output is a random variable $y \stackrel{R}{\leftarrow} A(m)$

### An important property of XOR

**_Thm:_** Y a rand. var. on $\{0,1\}^n$, X an indep. **uniform** var. on $\{0,1\}^n$, Then $Z = Y \bigoplus X$ is a a **uniform** var. on $\{0,1\}^n$ s

**_Proof:_** Just consider n = 1

|  Y  |  Pr.  |
| :-: | :---: |
|  0  | $p_0$ |
|  1  | $p_1$ |

|  X  |      Pr.      |
| :-: | :-----------: |
|  0  | $\frac{1}{2}$ |
|  1  | $\frac{1}{2}$ |

|  X  |  Y  |       Pr.       |
| :-: | :-: | :-------------: |
|  0  |  0  | $\frac{p_0}{2}$ |
|  0  |  1  | $\frac{p_1}{2}$ |
|  1  |  0  | $\frac{p_0}{2}$ |
|  1  |  1  | $\frac{p_1}{2}$ |

$$
\begin{aligned}
P\{Z=0\} &= P\{\ (x,y)=(0,0) \cup (x,y)=(1,1)\ \} \\&= p_0/2+p_1/2 \\&=1/2
\end{aligned}
$$

### [The birthday paradox](https://en.wikipedia.org/wiki/Birthday_problem)

Let $r_1,\cdots,r_n \in U$ be indep. identically distributed random vars.

**_Thm:_** when $n= 1.2 \times |U|^{1/2}$ then

$$
P\{\exist\ i \neq j: r_i = r_j \} \geq 1/2
$$

## Stream Cipher

### [The One Time Pad](https://en.wikipedia.org/wiki/One-time_pad)

- First example of a **secure** cipher

- key = ( random bit string as long the message )

$$
\begin{aligned}
E(k,m) &= k \bigoplus m
\\D(k,c) &= k \bigoplus c
\end{aligned}
$$

- Very fast enc/dec
- but long keys as long as PT

### [Information Theoretic Security](https://en.wikipedia.org/wiki/Information-theoretic_security)

- Shannon 1949

- **_Def:_** A cipher $(E,D)$ over $(K,M,C)$ has **_perfect secrecy_** if

$$
\forall m_0,m_1 \in M\ (|m_0|=|m_1|),\forall c \in C
$$

$$
P\{E(k,m_0)=c\}=P\{E(k,m_1)=c\}
$$

- **No CT only attack**

- **_Lemma:_** OTP has **_perfect secrecy_**

  - **_Proof:_** $\forall m\in M,c\in C$, There is exactly one key $(m\bigoplus c)$ maps $m$ to $c$

- **_Thm:_** perfect secrecy $\Rightarrow |K| \ge |M|$
- Hard to use in practice!

### [Pseudorandom Generators](https://en.wikipedia.org/wiki/Pseudorandom_generator)

- **_Stream Ciphers:_** making OTP practical
  - Idea: replace **random** key by "pseudorandom" key
  - Goal: decrease the length of key
- Stream ciphers **cannot** have perfect secrecy
  - Need a different definition of security
  - Security will depend on specific PRG
- **WEAK** PRG: glibc random():

$$
r[i]\leftarrow(r[i-3]+r[i-31])\%2^{32}
$$

$$
return\ r[i]>>1
$$

- PRG **MUST** be unpredictable
- We say that $G:K\rightarrow \{0,1\}^n$ is **predictable** if:

$$
\exist\ alg.\ A, \exist\ 0 \le i \le n-1
$$

$$
P\{A(G(k))|_{1,\cdots,i}=G(k)|_{i+1}\}>1/2+\varepsilon
$$

- **_Def:_** PRG is **unpredictable** if it is not predictable  
  $\forall i$, no **efficient** alg. can predict $bit_{i+1}$ for **non-negligible** $\varepsilon$

- Negligible

  - In practice: $\varepsilon$ is a scalar
    - non-neg: $\varepsilon\ge 1/2^{30}$ (likely to happen over 1 GB of data)
    - negligible: $\varepsilon\le 1/2^{80}$ (won't happen over life of key)
  - In theory: $\varepsilon$ is a function $\mathbb{Z}^{\ge 0} \rightarrow \mathbb{R}^{\ge 0}$
    - non-neg: $\exist d:\varepsilon(\lambda)>=1/\lambda^d$ inf. often
    - negligible: $\forall d,\lambda \ge \lambda_d: \varepsilon(\lambda)<1/\lambda^d$

### Attacks on OTP and stream ciphers

#### Attack 1: two time pad is insecure

Never use stream cipher key more than once

$$
c_1\leftarrow m_1 \bigoplus PRG(k)
$$

$$
c_2\leftarrow m_2 \bigoplus PRG(k)
$$

Then Adv. does:

$$
c_1 \bigoplus c_2 \rightarrow m_1 \bigoplus m_2
$$

Enough redundancy in English and ASCII encoding that:

$$
m_1 \bigoplus m_2 \rightarrow m_1,m_2
$$

- 802.11b WEP

$$
c=m \bigoplus PRG(IV||k)
$$

1. $IV$ increases by one every frame, but length of $IV$ is only 24 bits. After $2^{24}\approx$ 16M frames it repeats.

1. keys are related(only 24 of 1048 bits are different), And PRG used in WEP(RC4) is not secure when you use related keys.
   **A better construction**
   Also use PRG(k) to generate new keys so that each frame has a pseudorandom key.

- Disk encryption
  When the file changes, it's easy to tell where the change occurred instantly. That leaks information that attackers shouldn't actually know. Essentially it's another example of two time pad.
  **Typically do not use a stream cipher in disk encryption**

#### Attack 2: no integrity

OTP is malleable. Modifications to CT are undetected and have **predictable** impact on PT.

$$
D(E(m,k)\bigoplus p,k)=m\bigoplus p
$$

Attackers can choose $p$ to modify PT.

### Real-world stream ciphers

#### [RC4](https://en.wikipedia.org/wiki/RCv4)

Software cipher

$$
k(128\ bits)\rightarrow k'(2048\ bits) \looparrowright 1\ byte\ per\ round
$$

- used in HTTPS and WEP
- Weaknesses:
  - Bias in initial output: $P\{2^{nd}byte=0\}=2/256>1/256$
  - $P\{(0,0)\}=1/256^2+1/256^3>1/256^2$
  - Related key attacks

#### [CSS](https://en.wikipedia.org/wiki/Content_Scramble_System)

Hardware cipher **(badly broken)**

Using [Linear-feedback shift register (LFSR)](https://en.wikipedia.org/wiki/Linear-feedback_shift_register)

#### [eStream](https://en.wikipedia.org/wiki/ESTREAM)

a new kind of $PRG$: $\{0,1\}^s\times Nonce \rightarrow \{0,1\}^n$

$Nonce$: a non-repeating value for a given key.

$$
E(k,m;r)=m\bigoplus PRG(k;r)
$$

The pair (k,r) is never used more than once.

$Nonce$ is designed to reuse the key more than once.

A famous and successful example: [Salsa 20](https://en.wikipedia.org/wiki/Salsa20)

### PRG Security Defs

Let $G$:$K\rightarrow \{0,1\}^n$ be a $PRG$

**_Goal:_** define what it means that

$$
k\stackrel{R}{\leftarrow} K,output\ G(k)
$$

is **indistinguishable** from

$$
r\stackrel{R}{\leftarrow} R,output\ r
$$

#### Statistical Tests

**_Def_**: an alg. $A$ s.t. $A(x)$ outputs "0" or "1"

#### Advantage

Let $G$:$K\rightarrow \{0,1\}^n$ be a $PRG$, $\{0,1\}^n\rightarrow r$ and $A$ a **statistical test** on $\{0,1\}^n$

**_Def:_**

$$
Adv_{PRG}[A,G]=|Pr[A(G(k))=1]-Pr[A(r)]=1| \in [0,1]
$$

$Adv$ close to 1 $\Rightarrow$ A can dist. $G$ from random

$Adv$ close to 0 $\Rightarrow$ A cannot dist. $G$ from random

**_Def:_** We say that $G:K\rightarrow \{0,1\}^n$ is a **secure** $PRG$ if $\forall$ "eff" stat. tests $A$: $Adv_{PRG}[A,G]$ is **negligible**.

**_Thm:_** a secure $PRG$ is unpredictable.  
**_Proof:_** Just show a predictable is insecure.

**_Thm:_** an unpredictable $PRG$ is secure.
**_Proof:_** <https://en.wikipedia.org/wiki/Yao%27s_test>

More generally, let $P_1$ and $P_2$ be two distributions over $\{0,1\}^n$  
**_Def:_** We say that $P_1$ and $P_2$ are **computationally indistinguishable** if no **eff.** stat. tests that can distinguish $P_1$ and $P_2$ (denoted $P_1\approx_p P_2$)

### [Semantic Security](https://en.wikipedia.org/wiki/Semantic_security)

Adv. $A$ gives Chal. two message $m_0$ and $m_1$, Chal. returns c=$E(k,m_0)$ or c=$E(k,m_1)$

**_Def:_** $E$ is **semantically secure** if for all efficient $A$:

$$
Adv_{SS}[A,E]=|Pr[c=E(k,m_0)]-Pr[c=E(k,m_1)]|<negligible
$$

$\Rightarrow$ for all explicit $m_0,m_1\in M,k\leftarrow K:\{E(k,m_0)\}\approx_p\{E(k,m_1)\}$

**_Thm:_** $G:K\rightarrow \{0,1\}^n$ is a secure $PRG \Rightarrow$ stream cipher $E$ derived from G is semantically secure

**_Proof:_** Just to prove: $\forall$ sem. sec. adversary $A$, $\exist$ a $PRG$ adversary \$B s.t.

$$
Adv_{SS}[A,E]\leq 2Adv_{PRG}[B,G]
$$

## Block Cipher

### PRFs and PRPs

**_Def:_** Pseudo Random Function **(PRF)** defined over $(K,X,Y)$:

$$
F:K\times X \rightarrow Y
$$

such that exists **efficient** algorithm to evaluate $F(k,x)$

**_Def:_** Pseudo Random Permutation **(PRP)** defined over $(K,X)$

$$
E:K\times X \rightarrow X
$$

such that:

1. Exist **efficient** algorithm to evaluate $E(k,x)$
2. The function $E(k,\cdot)$ is one-to-one
3. Exists **efficient** inversion algorithm $D(k,y)$

Block cipher is actually a PRP

AES: $K\times X\rightarrow X$ where $K=X=\{0,1\}^{128}$

3DES: $K\times X\rightarrow$ where $X=\{0,1\}^{64},K=\{0,1\}^{168}$

### Secure PRFs and PRPs

Let $F:K\times X\rightarrow Y$ be a PRF

- $S_U$: the set of all functions from $|X|$ to $|Y|$, $|S_U|=|Y|^{|X|}$
- $S_F=\{F(k,\cdot)\ , k\in K\} \subseteq S_U$, $|S_F|=|K|$

**_Def:_** a PRF is **secure** if a random function in $S_U$ is **indistinguishable** from a random function in $S_F$

If we replace $S_U$ with the set of all **one to one** functions from $X$ to $X$, then we get **def.** of secure PRP, and actually we also get secure block cipher.

### An easy application: PRF $\Rightarrow$ PRG

Let $F:K\times \{0,1\}^n\rightarrow \{0,1\}^n$ be a secure PRF.

Then following $G:K\rightarrow \{0,1\}^{nt}$ is a secure PRG:

$$
G(k)=F(k,0)||F(k,1)||\cdots||F(k,n-1)
$$

It's security is easy to prove from the security of PRF.

### DES: 16 round Feistel Network

#### Core idea : [Feistel Network](https://en.wikipedia.org/wiki/Feistel_cipher)

Given functions $f_1,\cdots,f_d:\{0,1\}^n\rightarrow\{0,1\}^n$

Goal: build **invertible** function $F:\{0,1\}^{2n}\rightarrow\{0,1\}^{2n}$

Feistel Network uses $f$ as round functions. For each round $i=0,1,\cdots d$

1. Split the text block into two equal pieces $L_i,R_i$
2. compute

$$
\begin{cases}
R_i=f_i(R_{i-1})\bigoplus L_{i-1}\\
L_i=R_{i-1}
\end{cases}
$$

Then $L_d||R_d$ is the ciphertext.

Decryption is just to replace the second step with

$$
\begin{cases}
L_{i-1}=f_i(L_i)\bigoplus R_i\\
R_{i-1}=L_{i}
\end{cases}
$$

Then $L_0||R_0$ is the plaintext.

**_Thm(Luby-Rackoff '85):_** If $f:K\times \{0,1\}^n\rightarrow \{0,1\}^n$ is a secure PRF, Then 3-round Feistel $F:K^3\times \{0,1\}^{2n}$ is a secure PRP.

Now we have a method to build a secure PRP from a secure PRF. Essentially we have an efficient method to construct invertible functions from normal functions.

The Feistel structure has the advantage that encryption and decryption operations are very similar, even identical in some cases, requiring only a reversal of the key schedule. Therefore, the size of the code or circuitry required to implement such a cipher is nearly halved.

#### Details about DES

DES is the archetypal block cipher—an algorithm that takes a fixed-length string of plaintext bits and transforms it through a series of complicated operations into another ciphertext bitstring of the same length. In the case of DES, the block size is 64 bits. The key has 64 bits but only 56 bits are effective.

Since DES is based on Feistel Network, we can focus on the function $f$ used in F-Network.

$f_i$ is a function $F(k_i,x)$ and $k_i$ is from the DES key $k$.

The F-function, operates on half a block (32 bits) at a time and consists of four stages:

1. **Expansion:** the 32-bit half-block is expanded to 48 bits using the expansion permutation.
2. **Key mixing:** the 48-bit result is combined with $k_i$ using an XOR operation.
3. **Substitution:** after mixing in the $k_i$, the block is divided into eight 6-bit pieces before processing by the S-boxes, or substitution boxes. Each of the eight S-boxes replaces its 6 input bits with 4 output bits according to a non-linear transformation, provided in the form of a lookup table.
4. **Permutation:** finally, the 32 outputs from the S-boxes are rearranged according to a fixed permutation, the P-box. This is designed so that, after permutation, the bits from the output of each S-box in this round are spread across four different S-boxes in the next round.

**More details see:** <https://en.wikipedia.org/wiki/Data_Encryption_Standard>

#### The S-boxes

$$
S_i:\{0,1\}^6\rightarrow \{0,1\}^4
$$

If S-boxes are linear or almost linear, formally

$$
S_i(\vec{x})\equiv A_i\cdot \vec{x}\ (mod\ 2)
$$

We say that $S_i$ is a linear function. Then the entire DES would be linear so that the entire encryption function is actually matrix multiplication in modulo 2. DES would be so easy to break.

DONOT choose S-boxes at random.

#### Strengthening DES against exhaustive search

Because of the 56-bit key, the naive DES can now be cracked by anyone using a exhaustive search. As a result, many methods have emerged to strengthen DES.

##### Method 1: Triple-DES

Let $E:K\times M\rightarrow M$ be a block cipher

***Def:*** $3E((k_1,k_2,k_3),m)=E(k_1,D(k_2,E(k_3,m)))$

For 3DES: $|K|=2^{56*3}=2^{168}$

but easy attack in time $2^{118}$ (still secure)

- Why not $E(k_1,E(k_2,E(k_3,m)))$?
If $k_1=k_2=k_3$, then $E(k_1,D(k_2,E(k_3,m)))=E(k_1,m)$. We can implement naive DES with circuits that implement 3DES.

- Why not Doubyle-DES?
With 112-bits key, Double-DES has easy attack in $2^{57}$ that is just 2 times slower than DES attack. I will cover this attack in detail soon.

##### Method 2: DESX

Let $E:K\times M\rightarrow M$ be a block cipher

***Def:*** $EX((k_1,k_2,k_3),m)=k_1\bigoplus E(k_2,m\bigoplus k_3)$

for DESX: $|K|=2^{64+56+64}=2^{184}$

but easy attack in time $2^{64+56}=2^{120}$(still secure)

Note: $k_1\bigoplus E(k_2,m)$ or $E(k_2,m\bigoplus k_1)$ does NOTHING !!

#### More attcks on block ciphers