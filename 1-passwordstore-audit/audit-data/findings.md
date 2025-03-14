## [Severity-High] Variable store in storage on-chain are visible to anyone, no matter the solidity visibility keyword, meaning the password is not actually a private password.

The variable `PasswordStore::s_password` (string private s_password) is declared as a private. But all variables stored in storage on-chain are visible to anyone, not matter if the visibility keyword is "private". This means the password is not actually a private password.

**Description:**

[`PasswordStore::s_password`] All data stored on-chain is visible to anyone and can be read directly from the blockchain. The `PasswordStore::s_password` variable is intended to be private variable and only accessed throught getPassword function, which is intended only called by the owner contract.

We show one such method of reading any data off chain bellow 

**Impact:**

Anyone can read the private password, severely breaking the functionality of the protocol.

**Proof of Concept:** (Proof of code)

The below test case shows how anyone can read the password directly from the blockchain.

1. Create a locally running block chain
```shell
make deploy
```

2. Deploy the contract to the chain
```shell
make deploy
```

3. Run the storage tool.
We use `1` because thats the storage slot of `s_password` in the contract.

```shell
cast storage <ADDRESS_HERE> 1 --rpc-url http://127.0.0.1:8545
```

You can then parse that hex to a string with:

```shell
cast parse-bytes32-string <BINARY_VALUE>
```

And get an output of 
```shell
myPassword
```

**Recommended Mitigation:**

Due to this, the overall architecture of the contract should be rethought. One could encrypt the password off-chain, and then store the encrypted password on-chain. This would require the user to remember another password off-chain to decrypt the password. However you would also likely want to remove the view function as you wouln't one the user to accidentally send a transaction with the password that decrypts your password.


**Likelihood & Impact
- Impact HIGH
- Likelihood: HIGH
=> Severity: HIGH



## [Severity-High] `PasswordStore::setPassword` has not access controls, meaning that a non owner could change the password

**Description:**
The `PasswordStore::setPassword` is set to be an `external` function, however the nacspec of the function and overall purpose of the smart contract is that `This function allows only the owner to set a new password.`

```javascript
    function setPassword(string memory newPassword) external {
@>      // @audit - There are not access control
        s_password = newPassword;
        emit SetNetPassword();
    }
```

**Impact:**

Anyone can set/change the password of the contract, severely breaking the contract intended functionality.

**Proof of Concept:**

Add the following to the `PasswordStore.t.sol`
<details>
<summary>Code</summary>

```javascript
    function test_anyone_can_set_password(address randomAddress) public {
        vm.assume(randomAddress != owner);
        vm.prank(randomAddress);
        string memory expectedPassword = "myNewPassword";
        passwordStore.setPassword(expectedPassword);

        vm.prank(owner);
        string memory actualPassword = passwordStore.getPassword();
        assertEq(actualPassword, expectedPassword);
    }

```

</details>


To run the previous test:

```
forge test --mt test_anyone_can_set_password
```

**Recommended Mitigation:**

Add an access control conditional to the `PasswordStore:setPassword` function

```javascript
if(msg.sender != s_owner){
   revert PasswordStore__NotOwner();
}
```

**Likelihood & Impact
- Impact HIGH
- Likelihood: HIGH
=> Severity: HIGH

## [Severity Informational] The `PasswordStore::getPassword()` natspec indicates a parameter that doesn't exist causing the natspec to be incorrect 

**Description:**
```javascript
    /*
     * @notice This allows only the owner to retrieve the password.
@>   * @param newPassword The new password to set.
     */
    function getPassword() external view returns (string memory)
```

The `PasswordStore::getPassword()` function signature is `getPassword()` while the natspec says it should be `getPassword(string)`

**Impact:**

The natspec is incorrect 

<!-- **Proof of Concept:** becuase there is not proof of code--> 

**Recommended Mitigation:**

Remove the incorrect natspec line.

```diff
-     * @param newPassword The new password to set.
```

**Likelihood & Impact
- Impact NODE 
- Likelihood: HIGH
=> Severity: Informational/gas/Non-crits

Informational: Hey, this isn't a bug, but should know