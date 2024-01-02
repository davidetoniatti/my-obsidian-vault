### Hash functions
a function $H$ that take an arbitrary lenght message $X$ and return a fixed size digest $Y$.
a cryptohf has random output and should satisfy collition resistan property: no way for an attacker to either create or purposedly modify initial text so as to obtain original digest.
if I do a little modification, the digest completely change.
Use hash functions for integrity.
Formally, three property of incremental importance:
- proprierty 1: one wayness or preimage resistance: given the digest $Y=$ result of a hash function, it's hard to find ANY $X$ such as $H(X)=Y$
One way in crypto is much more than non-invertible (quantum computing not a problem for hash functions).
- property 2: second preimage resistance: given Xm its hard to find another X' such that H(X) = H(X')
- property 3 - collision resistance: it's hard to find two generic X1 and X2 such that H(X1)=H(X2)

### MAC using hash function
golden rule: dont use crypto alg for things different from there purpose
Merkle-Damgard.
initialization vector must be constant, because the result Y for X as to be the same every time i compute H(X).

## HMAC (Hash Message Auth Code)
Security in MAC
(m_1,t_1),...,(m_k,t_k) number of possible pairs to start
if i cannot forge a message m' != m such that (m',t')
Problem solved in 1996 
$$HMAC_K(M) = H(K^+ \oplus \text{ opad} || H(K^+ \oplus \text{ ipad} || M))$$
- requires 2 keys (nested mac)
- take a key and expand to size block (i.e. from 128bit to 512 bit) by adding sequence of zero;
- xor with ipad = 0x5c repeted as needed (opad = 0x36) this values flip nearly half of the bits 
- encrypt the IV

## User auth
Prove you are really the one you claim to be, dont confuse with identification, which is show your digital identity. Auth is prove that your digital identity is controlled by you.
Actually, not so simplez

Mitigation for chAP: explicit salt
attacker model: have access to db
send challenge + salt, user salt the passwd locally
if db attacked, just change the salt.

# Mutual authentication with CHAP

TAKE HOME: DON'T USE CHAP
I recieve the challenge, i can reply with the same challenge, and now you recieve the answer to the challenge.3

in single mutual auth, i recieve c2 
problem: using challenges independently.
problem : decouple the challenges.

question: how cbc work

# Wireless cellular system security (authentication)

2/3G mobile switching center, visiter location register (entity to do auth)
4G: sation: eNB/MME (for auth)
auth 2/3G

auth vect: xres (expected result, same as sres gsm), rand (same as gsm rand), ck (cipher key, same as gsm kc).
whats new: integrity key, derived by mobile station from cipher key
super new: network auth token = guarantees that network is authentic.
mutual auth with two messages!
nonce: time?
if time reference guaranteed, then i can send hash of timestamp and key, where timestamp is the challenge, not more selected by sender but guaranteed

idea:  use sequence number as "implicit" nonce.


## Handling Remote Access: RADIUS.
radius: AAA protocol
authentication, Authorization, Accounting
Authorization: have permissions to access a service?
Accounting: what are u currently doing/using/paying: taking record of what u do with the service.

radius: c/s protocol that support AAA (the first one); centralized 
sec features:
crypto-authenticated reply: shared-secret approach

