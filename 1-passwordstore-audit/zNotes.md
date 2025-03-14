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

Install solidity metrics tintinweb in vscode

Once solidity metrics is installed, right click to for example in "src" directory and run Solidity:metrics and it will give you the solidity report.

Then with ctrl + shift + p + export and we will be able to see in its own file like html




# Time to start looking at the codebase

### About the project in my words

A user should be able to set a password, and retrieve it. Other users should no be able to see my password


### Patrick collins uses this:
// n (for a note)
// q (for a question)
// @audit-i (audit for informational)
// i (for informational finding )

Patrick also works with a cool packed called headers which allows to write really nice headers 

### After studying the protocol

Create a folder "audit-data" and a file called finding_layout.md

```shell
mkdir audit-data && echo > finding_layout.md
```

The content of the finding_layout.md is like this

```shell
### [S-#] TITLE (Root Cause + Impact)

**Description:**

**Impact:**

**Proof of Concept:**

**Recommended Mitigation:**
```

Create another file in audit-data called findings.md

```shell
touch findings.md
```

For proof of concept, we have to run anvil to get a local running blockchain
```shell
anvil
```

Then we need to deploy on this locally running anvil blockchain. So we have to run the deployment script, and luckily for us we just need to type make deploy 
```shell
make deploy
```

Foundry has something call cast (copy and paste the address obtained above)
```
cast storage 0x5FbDB2315678afecb367f032d93F642f64180aa3

And we get this output:
╭------------+---------+------+--------+-------+-------------------------------------------------------------------------------+--------------------------------------------------------------------+-------------------------------------╮
| Name       | Type    | Slot | Offset | Bytes | Value                                                                         | Hex Value                                                          | Contract                            |
+=========================================================================================================================================================================================================================================+
| s_owner    | address | 0    | 0      | 20    | 1390849295786071768276380950238675083608645509734                             | 0x000000000000000000000000f39fd6e51aad88f6f4ce6ab8827279cfffb92266 | src/PasswordStore.sol:PasswordStore |
|------------+---------+------+--------+-------+-------------------------------------------------------------------------------+--------------------------------------------------------------------+-------------------------------------|
| s_password | string  | 1    | 0      | 32    | 49516443757395204518384437876896412918898210405993719258753982441762571943956 | 0x6d7950617373776f726400000000000000000000000000000000000000000014 | src/PasswordStore.sol:PasswordStore |
╰------------+---------+------+--------+-------+-------------------------------------------------------------------------------+--------------------------------------------------------------------+-------------------------------------╯

```

```shell
cast storage 0x5FbDB2315678afecb367f032d93F642f64180aa3 1 --rpc-url http://127.0.0.1:8545
```

"1" is because we are interested in slot 1

And we get this output:

0x6d7950617373776f726400000000000000000000000000000000000000000014

This is the binary representation of the value of the variable.

This means that if we run

```shell
cast parse-bytes32-string 0x6d7950617373776f726400000000000000000000000000000000000000000014
```

we get back: myPassword

### To explain about severity of a bug we will rely on docs.codehawks.com

Go to How to Evaluate a Finding Severity







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
