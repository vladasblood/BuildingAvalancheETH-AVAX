# BuildingAvalancheETH-AVAX

This ERC20 token contract provides basic functionalities such as token minting, burning, and transferring, while adhering to the ERC20 standard. Moreover, it was deployed in the Avalanche blockchain network. With a simple bullet shop.

## Description

My project focuses on creating a custom ERC20 token, DegenToken, specifically for the Degen Gaming platform on the Avalanche network. This token is designed to enhance the gaming experience by providing a versatile currency that players can earn, spend, and manage within the game. DegenToken allows for the minting of new tokens by the owner, which can be used to reward players and incentivize engagement. Players can transfer tokens to others, supporting a fluid in-game economy. Additionally, the token can be redeemed for "bullets," a virtual currency used for in-game purchases, with the redemption process also involving token burning. Players can check their token balance at any time for transparency and manage their holdings by burning tokens if needed. Built on OpenZeppelin's secure ERC20 and Ownable contracts, DegenToken ensures reliability and security while offering a flexible solution for integrating digital currency into gaming ecosystems.

## Getting Started

### Snowtrace Testnet

To facilitate the deployment and testing of the DegenToken smart contract on the Avalanche network, I utilized Snowtrace Testnet, a valuable tool for managing and verifying transactions on the Avalanche C-Chain. By accessing Snowtrace, I was able to credit AVAX testnet tokens to my account, providing the necessary funds to cover transaction fees and deploy the smart contract effectively. This process ensured that I could perform comprehensive testing of the token's functionalities, including minting, transferring, and redeeming tokens, without incurring real costs. Snowtrace’s interface offered transparency and ease of access, allowing me to monitor and validate each transaction and contract interaction on the Avalanche testnet efficiently.

Snowtrace Address Used: [Snowtrace](https://testnet.snowtrace.io/address/0xcA44aCF69576E7F5F8C717C5f525F737D7451Cd7)

AVAX Credits for Testnet: [CoreApp](https://core.app/tools/testnet-faucet/?subnet=c&token=c&fbclid=IwZXh0bgNhZW0CMTEAAR3Fzah7CG-wC25zStrJnL345nlLVHWOpHaFiuBFAQKWHEUqit_ytHNOPc8_aem_N23vqrgOfdXKUGj2I7RLHw)

### Installing

To run this program, one can utilize the provided IDE for Solidity which is Remix. To access the website, please direct to this site [Remix IDE](https://remix.ethereum.org/)

Once you are on the website, create a new file on the "+" icon in the left-handed sidebar. Save the file with a `.sol` extension (e.g., `DegenToken.sol`). Copy and paste the following code into the file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.23;

/*
Challenge
Your task is to create a ERC20 token and deploy it on the Avalanche network for Degen Gaming. The smart contract should have the following functionality:

1. Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
2. Transferring tokens: Players should be able to transfer their tokens to others.
3. Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
4. Checking token balance: Players should be able to check their token balance at any time.
5. Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.
*/

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenToken is ERC20, Ownable {
    
    uint256 public constant GUNPOWDER = 100;

    mapping(address => uint256) public bulletsOwned;

    constructor() ERC20("Degen", "DGN") Ownable(msg.sender) {
        _mint(msg.sender, 10 * (10 ** uint256(decimals())));
    }

    function redeemBullets(uint256 quantity) public {
        uint256 cost = GUNPOWDER * quantity;
        require(balanceOf(msg.sender) >= cost, "Insufficient Tokens for Bullets");

        bulletsOwned[msg.sender] += quantity;
        _burn(msg.sender, cost);
    }

    function verifyBullets(address user) public view returns (uint256) {
        return bulletsOwned[user];
    }

    function mintTokens(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    function checkBalance(address account) public view returns (uint256) {
        return balanceOf(account);
    }

    function burnTokens(uint256 amount) public {
        require(balanceOf(msg.sender) >= amount, "Not enough tokens to burn");
        _burn(msg.sender, amount);
    }

    function transferTokens(address to, uint256 amount) public {
        require(to != address(0), "Invalid address");
        require(balanceOf(msg.sender) >= amount, "Not enough tokens to transfer");
        _transfer(msg.sender, to, amount);
    }
}
```

To compile, click on the `Solidity Compiler` tab in the left-handed sidebar. Make sure the Compiler option is set to `0.8.23` (or another compatible version), and then click on the `Compile DegenToken.sol` button.

Once the compiler does not return any errors, you can deploy the contract by clicking on the `Deploy & Run Transactions` tab in the left-hand sidebar. Select the `DegenToken` contract (e.g., `DegenToken - DegenToken.sol`) from the dropdown menu, and then click on the `Deploy` button. 

### Executing the Program

After successfully deploying the DegenToken smart contract, you can proceed with a demonstration of its functionalities.

#### Imports
- Ensure that the import statements correctly point to the version of OpenZeppelin contracts (v4.3.0, in this case) that you are using. This includes the `ERC20` and `Ownable` contracts necessary for the token implementation and access control.

#### Inherited Functions
- Functions such as `_mint`, `_burn`, and `_transfer` are inherited from the `ERC20` contract. These provide the core functionalities needed for an ERC20 token, including minting new tokens, burning existing tokens, and transferring tokens between addresses.

#### `Constructor (constructor() ERC20("Degen", "DGN") Ownable(msg.sender))`
- **Purpose**: Initializes the contract with a predefined initial supply of tokens minted to the address that deploys the contract (msg.sender).
- **Details**: This constructor calls the constructor of the ERC20 contract with the token name "Degen" and symbol "DGN". It also uses the `Ownable` constructor to set the deployer as the owner of the contract. The `_mint` function is called to mint 10 tokens to the deployer, initializing their balance.

#### `mintTokens(address to, uint256 amount) public onlyOwner`
- **Purpose**: Allows the contract owner to mint new tokens and assign them to a specified address (`to`).
- **Details**: The `onlyOwner` modifier ensures that only the owner (deployer of the contract) can call this function. It calls the `_mint` function from ERC20 to create `amount` tokens and assign them to the address specified by `to`.

#### `burnTokens(uint256 amount) public`
- **Purpose**: Allows any token holder to burn (destroy) their own tokens, reducing the total supply.
- **Details**: The caller (`msg.sender`) specifies the amount of tokens they want to burn. The function calls `_burn` from ERC20 to decrease the balance of `msg.sender` by the specified amount.

#### `transferTokens(address to, uint256 amount) public`
- **Purpose**: Allows any token holder to transfer tokens from their own address to another address (`to`).
- **Details**: This function transfers `amount` tokens from `msg.sender` to `to`. It calls `_transfer` from ERC20 to handle the transfer logic and emit necessary events. The function does not need to explicitly return a value because it overrides the default behavior of ERC20’s transfer function and handles success implicitly.

#### `redeemBullets(uint256 quantity) public`
- **Purpose**: Allows players to redeem their tokens for "bullets" in the game.
- **Details**: The function calculates the cost in tokens for the specified quantity of bullets, checks if the sender has sufficient tokens, and if so, updates the bullets owned by the sender and burns the corresponding amount of tokens.

#### `verifyBullets(address user) public view returns (uint256)`
- **Purpose**: Allows checking the number of bullets owned by a specific address.
- **Details**: The function returns the number of bullets owned by the specified user address, allowing for transparency in the game's bullet economy.
  
## Help

If it somehow happens that `mint` does not perform, please review the address. You can only mint tokens as the smart contract owner.

## Authors

Vladimir Al Guerrero
[@Linkedin](https://www.linkedin.com/in/vladimir-al-guerrero-178b6a24b/)

## License

This project is licensed under the MIT License - see the `LICENSE.md` file for details
