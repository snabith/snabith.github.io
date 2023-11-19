---
layout: post
title: Network Security Series - 1. Intro
date: 2023-11-18 09:42 -0500
tags: [netsec]
categories: [Network Security]
img_path:
author: Snabith
math: true
---
To know how to attack, you must also know how to defend and vice versa. This Network security series is a summary of concepts I learnt from a class by a fantastic professor [Dr. William Robertson](https://wkr.io). Other posts from this series: (`Post list here`)

## Intro
1. What are the goals of a hacker? - Profit or Espionage(spying)
2. Define security - Policies and mechanisms for enforcing security protection properties over data and resources
3. List the security protection properties - Confidentiality, Integrity, Availability, Authenticity, and Non-repudiation. These properties form an essential framework for thinking about security. 
4. Explain what those properties mean to an attacker. - If you are maintaining:
	1. Confidentiality - An adversary cannot read the data/resources without imitating an authorized principal \[for an expected period] 
	2. Integrity - Cannot modify ..
	3. Availability - Cannot deny access to an otherwise accessible data or resource
	4. Authenticity - Cannot imitate a legitimate user. A basis for making trust decisions. 
	5. Non-repudiation - Cannot deny performing certain actions. \[This property is omitted to maintain anonymity and privacy.]
5. Explain threat models
	A threat model is developed by assuming that the adversary possesses certain capabilities. These assumptions help in developing solutions that can counter (either avoid, defend, or recover from) expected threats. 
6. Why use threat models
	A good use case is the fact that it can give order to a group's problem-solving.
7. A trusted computing base refers to the set of computer system components that must be trusted
	>A given piece of hardware or software is a part of the TCB iff it has been designed to be a part of the mechanism that provides its security to the computer system. In [operating systems](https://en.wikipedia.org/wiki/Operating_system "Operating system"), this typically consists of the kernel (or [microkernel](https://en.wikipedia.org/wiki/Microkernel "Microkernel")) and a select set of system utilities ([wiki](https://en.wikipedia.org/wiki/Trusted_computing_base))
	> Minimize the `trust` and TCB (Trust computing base). Trust gets weird in cloud environments

8. Trust should not be transitive but if you choose to trust an entity B for a functionality F, you should be aware that F may be affected by another entity C that is trusted by B. This can be practiced internally within an organization. A good example would be the Active Directory's transitive trust property while extending trust between forests. 

9. Policies are part of security. So, what should I know about a security policy?
	A security policy defines what it means for an organization to be secure. Usually, each policy has its purpose and limitations. ([Sans infosec policies](https://www.sans.org/information-security-policy/))
	To implement/enforce a security policy, you need to understand the concepts of access control(Defining who can access what), Authentication, and Authorization.
10. Fundamental terminology: 
    Principals - Participants of the policy/system. Ex: User
    Subjects - act on behalf of the principal. Ex: User's program
    Objects - Resources acted upon by subjects. Ex: Files
    Access control classes - Distinguished based on who can define the rules.
    Computer security models - schemes for specifying and enforcing security policies. ([wiki](https://en.wikipedia.org/wiki/Computer_security_model)).
        Access Control Lists - $\{<Subj_0,Obj_0,RW>,<S_1,O_1,RX>...\}$
        Simple policies used in filesystems and networks(traffic control)
        Capability based security([wiki](https://en.wikipedia.org/wiki/Capability-based_security)) - Token based security - revocation control
	
## Principles for Securing Systems

### Economy of Mechanism
- The design and implementation of security mechanisms should be kept simple. 
- Simplicity in implementation and design => Higher chances of implementation

### Defense in depth
- It's great that you have an efficient egress firewall. It would be better if you implement firewalls between your internal networks.
- Think of internal pentests.

### Fail-safe Defaults
- Consider an average network firewall system, it implements a default deny policy. (Say, you are using a digital oceans droplet, you have to explicitly set traffic rules to enable incoming traffic to ports). So, even if the user go with the default settings, the system is relatively secure. That's an example of fail-safe defaults.
- The opposite would be Active Directory. The default policies help to set up the system but not to secure the system. This is for backward compatibility reasons and performance reasons(Ex: SMB signing)

### Complete Mediation
- Say, a logged-in user got RW access to a shared file. Complete mediation dictates that the user's authority must be verified at every new access attempt. So, we must check the user permissions not only when the user opens the file but also when the user tries to save it (Access might have been revoked by the time the user saves it.)

### Open Design
- This is kind of the opposite of "Security through obscurity". (Open design assumes that the design is published)
- A system should be secure even if the design of the system is revealed.

### Separation of privilege
- I consider this as an extension of the principle of least privilege that avoids a single point of failure. 
- Although this requires us to create more users than necessary, it enhances the security posture of an organization.

### Principle of least privilege
- This is my personal favorite when combined with "complete mediation".
- When you implement the principle of least privilege, a compromised principal could only cause the least damage that principal can do. (a pentester's inner voice: "if implemented properly". recon>scan>'movement')

### Work Factor
- This is usually used in cryptography. A higher work factor indicates that more computational power or time is needed for hashing or key derivations. 
- This is a defense against the ideology of "Attacks only get better".
- To maintain security, one should remember that hardware and software to break security is only going to get better. 
- If it is time-consuming or complex to generate hashes, then it can potentially make the attacker's job harder to attack using a dictionary attack.

### Compromise Recording
- Auditing approach to security. It assumes that an attack will occur and assumes the goal of preventing future incidents by recording and learning from it.
- Incident response > Forensics > Timeline > Root cause analysis > Lessons learnt > Documentation.
