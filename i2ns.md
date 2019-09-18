# i2ns

[TOC]

## 18/09/2019

### Security Properties

> These are **principles** of Security!

* **Confidentiality**: Prevent unauthorized disclosure of information
* **Integrity**: Prevent unauthorized modification of information
* **Avaliability**: Prevent unauthorized withholding of information of resources

![https://www.ibm.com/blogs/cloud-computing/wp-content/uploads/2018/01/TRIAD.png](assets/TRIAD.png)

A **qualitative graph:**

```mermaid
graph TD;
CIA==Refinements==>a[Security Policies]
a==Enforcement==>b[Security Mechanisms]
```

Security policies are a **set of rules**.

One security mechanism is a **sandbox**.

> A **sandbox** is an area which everything is controlled if inside. If the executable needs an access to something in particular (like a system call), it needs permissions.

> This is mostly used by **android echosystem.**

Another security mechanism it's **Encryption**

_Which one of the CIA Triad is more important to us?_

> These secuity mechanism are **principally SW.** A **SW** is full of bugs. Known or uknown bugs, they are a problem. There's a **risk** everytime, we need to have the **lowest risk possible.**

### AUTH

> **Authentication**: A Security Mechanism used to prevent unauthorized access to a system, verifying an user with **credentials**(userrname and password)



> **Knowledge Factor** in auth process: Something that should be kept **as a secret**, like **passwords**.

#### Auth Username-Password login

The auth system with USERNAME-PASSWORD works only if we can remember a PASSWORD.

This is a problem when the password is too easy.

##### The **knowledge factor**, in this case, should be a **complex** and **hard-to-remember** password.

Obviously, it's hard to remember this.

##### Guessing

One of the easiest way to `crack` a password is **guessing**.(using **dictionaries**, which are password-Lists)

One of the **main weakness** of the **human being** is not-remembering a difficult password.

##### Phishing

> **Phishing**: Creating a fake login attack, very similar or equal to a service.
> When the victim access to a phishing page, the password is sent to the attacker.

##### Same-Password

Another weakness is **using the same password for more services.** If one of them gets hacked, someone could use that hacked password to enter somewhere else.

##### Fix

A simple fix is using common-words combination instead of hard passwords.



#### AuthN: Bootstrapping

* Password Should be *secret between user and system.*
* A user can get password personally, but the communication could be intercepted.
  * A good fix is forcing the user to change the psw while trying to log in.
  * If he forgot the password, **don't send them 'plaintextly'**, send them an unique link with hours of validity. This link is used to change the password.

#### Storing Passwords

##### Hash Functions

We can store a password plaintext, but this is a **very bad practice.**

A good way to store password is to **store the HASH of the password** 

##### Hash: 1-way functions

* Cryptographic hash functions

* > A 1-way function $f$ is a function that is **relatively easy to compute but hard to reverse.**

  * Given an input $x$ and a 1-way function $f$, $f(x)=y$ . $y$ should follow the [Avalanche Effect](https://en.wikipedia.org/wiki/Avalanche_effect). $y$ is called **digest**.

* This function have a fixed output length.

* A hash function shoud be **cheap to execute.**

> There's a more secure way to do that. An attacker can try to use a **rainbow table**.

a **rainbow table** is a dictionary with a precomputed hash. We can try to search the hash there and check if it exists a `dehashed` password.

If the hash is used in more than one user, **we can access more users with the same password**. We know that it's the same password becouse it's the **same HASH**.



We can fix this with a HASH-SALTED password.

##### HASH SALTING

We can use a '**salt**' variable, which is a random string(generated with a good seed), for each new password stored.

We add this salt to the password.

for example 

---

$\text{hash}(pass)=\text{hash}(pass)$

while

$\text{hash}(pass+salt_1)\ne \text{hash}(pass+salt_2)$ 

obviously $salt_1\ne salt_2$

---

> **WARNING**: Even if this mitigate the problem of rainbow table, we need to use **STRONG** HASH functions.
>
> _Example:_ `MD5` digest is small and crackable, while `sha256` is stronger, like `BCRYPT`.



*Overtime the robustness of an algorithm is usually weakened by stronger and faster computers.*

*What's the current State-Of-The-Art?*

*Salt should generated with **security libraries** which generates **unpredictable sequencies**(or hard-to-predict sequencies).*

#### SINGLE-SIGN-ON (SSO)

> **SSO:** Auth process which allows you to log in with the same set of credential to multiple service provider (asking the Identity Provider, like *google SSO*)



How it works?

Well:

```mermaid
graph TD;
a[Service Provider 1]---b[Identity Provider]
c[Service Provider 2]---b
```

More precisely:

```mermaid
graph LR;
user--Wants to log in-->a
b==Accept the login===c{or}
b==Reject the login===c
c==>a
a[Service Provider 1]==Ask if the login is ok==>b[Identity Provider]
```

As we can see:

1. The service provider **doesn't care about auth**, it just redirect the login to the **Identity Provider**;
2. The **identity provider** check and reply to the Service provider if it's OK or NOT OK;
3. the **Service provider will let the user log in if it's successful**. If it's not, it will reject the user.



Problems:

1. We need to (usually) pay for this service;
2. It should be **frictionless**;
3. We need to trust the **Identity Provider** security mechanisms, if it gets hacked, all the services gets hacked.
4. If the **Identity Provider** goes offline, all the services can't log in.
5. If the comunication between the **SP** and the **IP** is interceptable, it's **very problematic.** We should have a strong **SSL/TLS** connection.



> The standard is **SAML 2.0**

In italy we have **SPID**, even if it's not **mobile-first** designed.



#### Assurance Levels

it's a Graph between **Relative Security Level** and **Solutions**.

1. Something you **know**(password) $\leftarrow$*worst*
2. Something you **have** + something you **know**(keycard+password)
3. Something you **have** + something you **are**(Keycard+ Biometric)
4. Something you **have** + Something you **know** + Something you **are**(Keycard+password+Biometric) $\leftarrow$*best*

> **Possession Factor**: When using something, it's required to set a **trusted device**. When you log in, (**knowledge factor**) you are asked to press a button in a device(*like phone push notification*) or gets a **1TP**(One time Password) randomly generated in your **trusted device**.



#### Contextual or behavioural auth

1. Checks *IP*(Is it the first time you log in from this location?)
2. Checks for *Cookies, geoloc*, ...

> Very effective.