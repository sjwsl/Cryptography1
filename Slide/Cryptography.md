<!-- vscode-markdown-toc -->
* 1. [Overview](#Overview)
	* 1.1. [**Crypto core**](#Cryptocore)
	* 1.2. [**Three steps in cryptography**](#Threestepsincryptography)
* 2. [History](#History)
	* 2.1. [**Substitution Cipher**](#SubstitutionCipher)
	* 2.2. [**Vigener Cipher**](#VigenerCipher)
	* 2.3. [**Roter Machines**](#RoterMachines)
	* 2.4. [**Data Encryption Standard**](#DataEncryptionStandard)
* 3. [Randomized algorithm](#Randomizedalgorithm)
* 4. [An important property of XOR](#AnimportantpropertyofXOR)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

This is my note of ***Cryptography 1*** by ***Dan Boneh***

##  1. <a name='Overview'></a>Overview

---

###  1.1. <a name='Cryptocore'></a>**Crypto core**

* Secret ket establishment
  
* Secure communication
  * confidentially
  * integrity
  
* Digital signature
  
* Anonymous communication
  
* Anonymous **digital** cash
  * Can I spend a **digital coin** without anyone knowing who I am?
  * How to prevent double spending?

* Secure multi-party computation
  * ***Thm:*** anything that can done with trusted authority can also be done without trusted authority

* Privately outsourcing computation
  * Get search result from Google while Google doesn't know what you search for

* Zero knowledge
  * one party (the prover) can prove to another party (the verifier) that they know a value x, **without conveying any information** apart from the fact that they know the value x

###  1.2. <a name='Threestepsincryptography'></a>**Three steps in cryptography**

1. Precisely specify threat model
    * What an attacker can do
    * What an attacker's goal is

2. Propose a construction

3. Prove that breaking construction under threat mode will solve an underlying hard problem

##  2. <a name='History'></a>History

---

###  2.1. <a name='SubstitutionCipher'></a>**Substitution Cipher**

* Use frequency of English letters and pairs of letters to easily break it

###  2.2. <a name='VigenerCipher'></a>**Vigener Cipher**

* The key is a word
* Just break it as substitution cipher
 
###  2.3. <a name='RoterMachines'></a>**Roter Machines**

* Early example: the Hebern machine (single rotor)
* Most famous: the Enigma (3-5 rotors)
  * Designed to defend frequency attack (statistic attack)
  * keys = $26^4$ = $2^{18}$
  * Still can't defend the ciphertext only attack

###  2.4. <a name='DataEncryptionStandard'></a>**Data Encryption Standard**

* DES: #keys = $2^{56}$ , block size = 64 bits
* Today: AES(2001), Salsa20(2008) (and many others)

##  3. <a name='Randomizedalgorithm'></a>Randomized algorithm

---

* Deterministic algorithm: $y \leftarrow A(m)$
  * output is a deterministic value
* Randomized algorithm: $y \leftarrow A(m;r)$ where $r \stackrel{R}{\leftarrow} \{0,1\}^n$
  * output is a random variable $y \stackrel{R}{\leftarrow} A(m)$

##  4. <a name='AnimportantpropertyofXOR'></a>An important property of XOR

---

***Thm:*** Y a rand. var. over $\{0,1\}^n$, X an indep. **uniform** var. on$\{0,1\}^n$, Then $Z = Y \bigoplus X$ is a a **uniform** var. on $\{0,1\}^n$

***Proof:*** Just consider n = 1

y | 


