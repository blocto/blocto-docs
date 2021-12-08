---
description: Blocto uses mixed-custodial model to provide both convenience and security
---

# Key Management

Blocto uses **mixed-custodial key management model** to provide both convenience and security, since both custodial and non-custodial models have their flaws:

## Non-Custodial

In non-custodial systems, user takes full responsibility in their key management. This is aligned with some of core-values of blockchain: decentralization and autonomous.

### Advantages

1. Doesn't have to trust any organizations.
2. Users have full control over their accounts.

### Disadvantages

1. Complex and intimidating, especially for beginners.
2. If user loses the key, nobody can help recover the account.
3. If the key gets leaked, someone else will have full control over the user's account.

## Custodial

In custodial systems, some 3rd party helps users manage their keys and provide an account/password or OAuth interface for managing the access.

### Advantages

1. Much simpler user experience. Similar to centralized systems.
2. If user forgets the password, there's always a way to recover user's access.
3. Custodial service operators usually have better security measures to manage the keys than non-tech-savvy users.

### Disadvantages

1. The custodial service operator can be malicious. They may steal users' assets.
2. The custodial service poses an appealing target for hackers. Any centralized system has the potential to be hacked.

## Mixed-Custodial

Blocto incorporates a mixed-custodial key management model to take the advantages from both sides:

1. **Custodial at first**\
   When users just signed up, the key is stored in Blocto's custodial service. This is for providing an easy on boarding experience to the users and avoid bombarding them with complexity. The keys stored in our services are managed by HSMs to provide superior security.
2. **Non-custodial later**\
   Users have the option to upgrade to non-custodial mode by generating a new key on their mobile device and replace the ownership of the account. By then the keys stored in Blocto's custodial service will be voided and won’t be able to access the account anymore.

The idea is that when user just started, they don't care that much about security, since they don't have anything in their wallet yet. At this stage, **custodial model** is a better choice. As time goes by, user have accumulated more assets in their wallets and more knowledge about how blockchain works. This is the appropriate time for advanced users to switch to **non-custodial model** for more security.
