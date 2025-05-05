# Experiment 7: AI-Powered Smart Contract for Decentralized Negotiation

# Aim:

To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.

# Algorithm:

Step 1: Item Listing: Seller lists an item with base, minimum, and maximum prices.

Step 2: Offer Submission: Buyer submits an offer along with ETH matching the offer price.

Step 3: Offer Validation: Smart contract checks if the item is still available and the payment is correct.

Step 4: AI Evaluation: dynamicPricing() simulates AI to calculate a counteroffer based on offer vs. price range.

Step 5: Decision Making: If the buyer's offer meets or exceeds the AI’s counteroffer, the sale is confirmed.

Step 6: Transaction Finalization: Seller receives payment, buyer receives confirmation, and item is marked sold.

Step 7: Counteroffer Emission: If the offer is too low, funds are refunded and a counteroffer is emitted as an event.


# Program:

### Developed by: JAGADEESH P
### Register number: 212223230083

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AIPoweredNegotiation {
    struct Item {
        address seller;
        uint256 minPrice;
        uint256 maxPrice;
        uint256 basePrice;
        bool sold;
    }

    mapping(uint256 => Item) public items;
    uint256 public itemCount;

    event ItemListed(uint256 itemId, uint256 basePrice);
    event OfferMade(uint256 itemId, address buyer, uint256 offer);
    event CounterOffer(uint256 itemId, uint256 counterOffer);
    event Sold(uint256 itemId, address buyer, uint256 finalPrice);

    function listItem(uint256 _basePrice, uint256 _minPrice, uint256 _maxPrice) public {
        require(_minPrice <= _basePrice && _basePrice <= _maxPrice, "Invalid price range");
        
        items[itemCount] = Item(msg.sender, _minPrice, _maxPrice, _basePrice, false);
        emit ItemListed(itemCount, _basePrice);
        itemCount++;
    }

    function makeOffer(uint256 _itemId, uint256 _offerPrice) public payable {
        Item storage item = items[_itemId];
        require(!item.sold, "Item already sold");
        require(msg.value == _offerPrice, "Incorrect offer amount");

        emit OfferMade(_itemId, msg.sender, _offerPrice);

        uint256 aiCounterOffer = dynamicPricing(item.basePrice, item.minPrice, item.maxPrice, _offerPrice);

        if (_offerPrice >= aiCounterOffer) {
            item.sold = true;
            payable(item.seller).transfer(_offerPrice);
            emit Sold(_itemId, msg.sender, _offerPrice);
        } else {
            payable(msg.sender).transfer(_offerPrice); // Refund buyer
            emit CounterOffer(_itemId, aiCounterOffer);
        }
    }

    function dynamicPricing(uint256 base, uint256 min, uint256 max, uint256 offer) private pure returns (uint256) {
        if (offer >= max) return max;
        if (offer >= base) return base;
        return (base + offer) / 2; // Simple AI-based counteroffer logic
    }
}

```

# Expected Output:

### Buyers submit offers, and the contract auto-negotiates the price.
![WhatsApp Image 2025-05-05 at 14 04 34_1bf97fa5](https://github.com/user-attachments/assets/7a478f0c-4ab8-40db-b249-aedd8d9dc73e)

![WhatsApp Image 2025-05-05 at 14 04 35_36ff1925](https://github.com/user-attachments/assets/07bc64db-a9dc-49a8-92c1-c10bc8b6cce7)



### If the buyer’s offer is fair, the deal is executed.
![WhatsApp Image 2025-05-05 at 14 04 34_ce813d85](https://github.com/user-attachments/assets/451a9a8c-7e99-4294-a6a9-0e54db8d9d44)


### If the offer is too low, the contract suggests a counteroffer.
![WhatsApp Image 2025-05-05 at 14 04 33_058537cc](https://github.com/user-attachments/assets/d969d69d-2ef0-4f97-9f50-d8d044dbb64b)




# High-Level Overview:

First-of-its-kind AI-powered pricing contract.


Mimics real-world price negotiations using dynamic on-chain pricing.


Can be extended to AI oracles for real-time market data.


Inspired by AI-enhanced commerce and eBay-like decentralized auctions.

# RESULT:

Thus, to create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model is executed successfully.



