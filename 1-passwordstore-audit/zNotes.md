# Steps for a security reasearch

## The Rekt Test

Code maturity” is important!

```
1) Do you have all actors, roles, and privileges documented?
2) Do you keep documentation of all the external services, contracts, and oracles you rely on?
3) Do you have a written and tested incident response plan?
4) Do you document the best ways to attack your system?
5) Do you perform identity verification and background checks on all employees?
6) Do you have a team member with security defined in their role?
7) Do you require hardware security keys for production systems?
8) Does your key management system require multiple humans and physical steps?
9) Do you define key invariants for your system and test them on every commit?
10) Do you use the best automated tools to discover security issues in your code?
11) Do you undergo external audits and maintain a vulnerability disclosure or bug bounty program?
12) Have you considered and mitigated avenues for abusing users of your system?

https://blog.trailofbits.com/2023/08/14/can-you-pass-the-rekt-test/
```

## Other important tools for getting information about the protocol to start a security review.
```
passwordStore/minimal-onboarding-questions.md
minimal-onboarding-filled.md
```

## Clone the repository and check out the commit hash

After clonning the true repository we can checkout that the exact commit hash is part of the audit scope, so we will copy the commit hash "7d55682ddc4301a7b13ae9413095feffd9924566". It is suppose to be located in the minimal-onboarding-filled.md 
```shell
git branch
git checkout 7d55682ddc4301a7b13ae9413095feffd9924566
```

it is suppose that we should see the "onboarded" branch, if not don't care.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

```shell
git switch -c passwordstore-audit (git switch -c <new-branch-name>)
```

and if you run
```shell
git branch
```

we can see we are on the branch passwordstore-audit

Note: If they don't give you a commit, maybe you would do a ton of work on some protocol and they go "oh no we didn't want you to test that, we wanted you to test this other commit" and you wasted a ton of time, so this will save you making sure you have all this information and just by you asking the minimal questions, you are going to teach the protocol so much about security and so much about just some of the requirements are there.

## Stats

There is a tool a lot of people use called cloc or count of lines of code, this will work for any code base you work, it simply counts the lines of code. You can install globally.

```shell
npm install -g cloc
cloc --help
```

If we want to apply cloc to the src directory

```shell
cloc ./src/
```



## Time to start looking at the codebase





## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

-   **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
-   **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
-   **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
-   **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Init
```shell
$ forge init <project-name>
```

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
