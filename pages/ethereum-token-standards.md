---
layout: default
title: Ethereum Token Standards
category: software developer
toc: true
---

# ERC 20

* A standard interface for tokens.
* [EIP 20: ERC-20 Token Standard](https://eips.ethereum.org/EIPS/eip-20)

## API

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function name() public view returns (string)` | Returns the name of the token. |   |
   | `function name() public view returns (string)` | Returns the symbol of the token. |   |
   | `function decimals() public view returns (uint8)` | Returns the number of decimals the token uses. |   |
   | `function totalSupply() public view returns (uint256)` | Returns the total token supply. |   |
   | `function balanceOf(address _owner) public view returns (uint256 balance)` | Returns the account balance of another account with address `_owner`. |   |
   | `function transfer(address _to, uint256 _value) public returns (bool success)` | Transfers `_value` amount of tokens to address `_to`, and MUST fire the `Transfer` event. |   |
   | `function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)` | Transfers `_value` amount of tokens from address `_from` to address `_to`, and MUST fire the `Transfer` event. |   |
   | `function approve(address _spender, uint256 _value) public returns (bool success)` | Allows `_spender` to withdraw from your account multiple times, up to the `_value` amount. |   |
   | `function allowance(address _owner, address _spender) public view returns (uint256 remaining)` | Returns the amount which `_spender` is still allowed to withdraw from `_owner`. |   |
   | `event Transfer(address indexed _from, address indexed _to, uint256 _value)` | MUST trigger when tokens are transferred, including zero value transfers. |   |
   | `event Approval(address indexed _owner, address indexed _spender, uint256 _value)` | MUST trigger on any successful call to `approve` function. |   |

# ERC-165

* Creates a standard method to publish and detect what interfaces a smart contract implements.
* [EIP 165: ERC-165 Standard Interface Detection](https://eips.ethereum.org/EIPS/eip-165)

## API

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function supportsInterface(bytes4 interfaceID) external view returns (bool)` | Query if a contract implements an interface. |   |

# ERC-721

* A standard interface for non-fungible tokens, also known as deeds.
* [EIP 721: ERC-721 Non-Fungible Token Standard](https://eips.ethereum.org/EIPS/eip-721)

## API

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function balanceOf(address _owner) external view returns (uint256)` | Count all NFTs assigned to an owner. |   |
   | `function ownerOf(uint256 _tokenId) external view returns (address)` | Find the owner of an NFT. |   |
   | `function safeTransferFrom(address _from, address _to, uint256 _tokenId, bytes data) external payable` | Transfers the ownership of an NFT from one address to another address. |   |
   | `function safeTransferFrom(address _from, address _to, uint256 _tokenId) external payable` | Transfers the ownership of an NFT from one address to another address. |   |
   | `function transferFrom(address _from, address _to, uint256 _tokenId) external payable` | Transfer ownership of an NFT. |   |
   | `function approve(address _approved, uint256 _tokenId) external payable` | Change or reaffirm the approved address for an NFT. |   |
   | `function setApprovalForAll(address _operator, bool _approved) external` | Enable or disable approval for a third party ("operator") to manage  all of `msg.sender`'s assets. |   |
   | `function getApproved(uint256 _tokenId) external view returns (address)` | Get the approved address for a single NFT. |   |
   | `function isApprovedForAll(address _owner, address _operator) external view returns (bool)` | Query if an address is an authorized operator for another address. |   |
   | `event Transfer(address indexed _from, address indexed _to, uint256 indexed _tokenId)` | This emits when ownership of any NFT changes by any mechanism. |   |
   | `event Approval(address indexed _owner, address indexed _approved, uint256 indexed _tokenId)` | This emits when the approved address for an NFT is changed or reaffirmed. |   |
   | `event ApprovalForAll(address indexed _owner, address indexed _operator, bool _approved)` | This emits when an operator is enabled or disabled for an owner. |   |

