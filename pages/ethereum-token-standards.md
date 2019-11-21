---
layout: default
title: Ethereum Token Standards
category: software developer
toc: true
---

# Ethereum Token Standards

## ERC 20

* A standard interface for tokens.
* [EIP 20: ERC-20 Token Standard](https://eips.ethereum.org/EIPS/eip-20)

### API

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

## ERC-165

* Creates a standard method to publish and detect what interfaces a smart contract implements.
* [EIP 165: ERC-165 Standard Interface Detection](https://eips.ethereum.org/EIPS/eip-165)

### API

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function supportsInterface(bytes4 interfaceID) external view returns (bool)` | Query if a contract implements an interface. |   |

## ERC-721

* A standard interface for non-fungible tokens, also known as deeds.
* [EIP 721: ERC-721 Non-Fungible Token Standard](https://eips.ethereum.org/EIPS/eip-721)

### API

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

## ERC-777

* standard interfaces and behaviors for token contracts.
* [EIP 777: ERC777 Token Standard](https://eips.ethereum.org/EIPS/eip-777)

### API

#### ERC777Token (Token Contract)

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function name() external view returns (string memory)` | Get the name of the token. |   |
   | `function symbol() external view returns (string memory)` | Get the symbol of the token. |   |
   | `function totalSupply() external view returns (uint256)` | Get the total number of minted tokens. |   |
   | `function balanceOf(address holder) external view returns (uint256)` | Get the balance of the account with address `holder`. |   |
   | `function granularity() external view returns (uint256)` | Get the smallest part of the token that’s not divisible. |   |
   | `function defaultOperators() external view returns (address[] memory)` | Get the list of default operators as defined by the token contract. |   |
   | `function authorizeOperator(address operator) external` | Set a third party `operator` address as an operator of `msg.sender` to send and burn tokens on its behalf. |   |
   | `function revokeOperator(address operator) external` | Remove the right of the `operator` address to be an operator for `msg.sender` and to send and burn tokens on its behalf. |   |
   | `function isOperatorFor(address operator, address holder) external view returns (bool)` | Indicate whether the `operator` address is an operator of the `holder` address. |   |
   | `event AuthorizedOperator(address indexed operator, address indexed holder)` | Indicates the authorization of `operator` as an operator for `holder`. |   |
   | `event RevokedOperator(address indexed operator, address indexed holder)` | Indicates the revocation of operator as an operator for holder. |   |
   | `function send(address to, uint256 amount, bytes calldata data) external` | Send the `amount` of tokens from the address `msg.sender` to the address to. |   |
   | `function operatorSend(address from, address to, uint256 amount, bytes calldata data, bytes calldata operatorData) external` | Send the `amount` of tokens on behalf of the address `from` to the address `to`. |   |
   | `event Minted(address indexed operator, address indexed to, uint256 amount, bytes data, bytes operatorData)` | Indicate the minting of `amount` of tokens to the `to` address by the `operator` address. |   |
   | `function burn(uint256 amount, bytes calldata data) external` | Burn the `amount` of tokens from the address `msg.sender`. |   |
   | `function operatorBurn(address from, uint256 amount, bytes calldata data, bytes calldata operatorData) external` | Burn the `amount` of tokens on behalf of the address `from`. |   |
   | `event Burned(ddress indexed operator, address indexed from, uint256 amount, bytes data, bytes operatorData)` | Indicate the burning of `amount` of tokens from the `from` address by the `operator` address. |   |

#### ERC777TokensSender

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function tokensToSend(address operator, address from, address to, uint256 amount, bytes calldata data, bytes calldata operatorData) external` | Notify a request to send or burn (if `to` is `0x0`) an `amount` tokens from the `from` address to the `to` address by the `operator` address. |   |

#### ERC777TokensRecipient

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function tokensReceived(address operator, address from, address to, uint256 amount, bytes calldata data, bytes calldata operatorData) external` | Notify a send or mint (if `from` is `0x0`) of `amount` tokens from the `from` address to the `to` address by the operator address. |   |

## ERC-1400

* Represents a library of standards for security tokens on Ethereum.
* [ERC 1400: Security Token Standard](https://github.com/ethereum/EIPs/issues/1411)

### Features

* Document management
* Error signalling
* Gate keeper (operator) access control
* Off-chain data injection
* Issuance / redemption semantics

### Components

   | Component | Title | Description | Remarks |
   |-----------|-------|-------------|---------|
   | [ERC-1594](https://github.com/ethereum/EIPs/issues/1594) | Core Security Token Standard | n-chain restriction checking with error signalling, off-chain data injection for transfer restrictions and issuance / redemption semantics |   |
   | [ERC-1410](https://github.com/ethereum/EIPs/issues/1410) | Partially Fungible Tokens | differentiated ownership / transparent restrictions |

### API

#### ERC-1594

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function canTransfer(address _to, uint256 _value, bytes _data) external view returns (byte, bytes32)` |   |   |
   | `function canTransferFrom(address _from, address _to, uint256 _value, bytes _data) external view returns (byte, bytes32);` |   |   |
   | `function transferWithData(address _to, uint256 _value, bytes _data) external` |   |   |
   | `function transferFromWithData(address _from, address _to, uint256 _value, bytes _data) external` |   |   |
   | `function isIssuable() external view returns (bool)` |   |   |
   | `function issue(address _tokenHolder, uint256 _value, bytes _data) external` |   |   |
   | `function redeem(uint256 _value, bytes _data) external` |   |   |
   | `function redeemFrom(address _tokenHolder, uint256 _value, bytes _data) external` |   |   |
   | `event Issued(address indexed _operator, address indexed _to, uint256 _value, bytes _data)` |   |   |
   | `event Redeemed(address indexed _operator, address indexed _from, uint256 _value, bytes _data)` |   |   |

#### ERC-1410

   | Function/Event | Description | Remarks |
   |----------------|-------------|---------|
   | `function balanceOf(address _tokenHolder) external view returns (uint256)` |   |   |
   | `function balanceOfByPartition(bytes32 _partition, address _tokenHolder) external view returns (uint256)` |   |   |


## Misc

### Required Feature/API

* Limit the maximum transfer amount at once per holder

* Limit the maximum transfer amount for a specific period per holder

* Surppress the entire transfer until the specified, finite, future date.


