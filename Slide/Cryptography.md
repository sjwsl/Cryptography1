<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

# Cryptography

This is my note of **_Cryptography 1_** by **_Dan Boneh_**

## Overview

### Crypto core

- Secret ket establishment

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

- Zero knowledge
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

#### Data Encryption Standard

- DES: #keys = $2^{56}$ , block size = 64 bits
- Today: AES(2001), Salsa20(2008) (and many others)

### Randomized algorithm

- Deterministic algorithm: $y \leftarrow A(m)$
  - output is a deterministic value
- Randomized algorithm: $y \leftarrow A(m;r)$ where $r \stackrel{R}{\leftarrow} \{0,1\}^n$
  - output is a random variable $y \stackrel{R}{\leftarrow} A(m)$

### An important property of XOR

**_Thm:_** Y a rand. var. on $\{0,1\}^n$, X an indep. **uniform** var. on $\{0,1\}^n$, Then $Z = Y \bigoplus X$ is a a **uniform** var. on $\{0,1\}^n$ s

**_Proof:_** Just consider n = 1
| Y | Pr. |  
| - | - |
| 0 | $p_0$ |
| 1 | $p_1$ |

| X   | Pr.           |
| --- | ------------- |
| 0   | $\frac{1}{2}$ |
| 1   | $\frac{1}{2}$ |

| X   | Y   | Pr.             |
| --- | --- | --------------- |
| 0   | 0   | $\frac{p_0}{2}$ |
| 0   | 1   | $\frac{p_1}{2}$ |
| 1   | 0   | $\frac{p_0}{2}$ |
| 1   | 1   | $\frac{p_1}{2}$ |

$$
\begin{aligned}

P\{Z=0\} &= P\{\ (x,y)=(0,0) \cup (x,y)=(1,1)\ \} \\&= p_0/2+p_1/2 \\&=1/2

\end{aligned}
$$

### The birthday paradox

Let $r_1,\cdots,r_n \in U$ be indep. identically distributed random vars.

**_Thm:_** when $n= 1.2 \times |U|^{1/2}$ then

$$
P\{\exist\ i \neq j: r_i = r_j \} \geq 1/2
$$

## Stream cipher

---