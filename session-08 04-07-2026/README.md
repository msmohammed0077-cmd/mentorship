# Add / Modify Cart — Design Package

This package documents the **Add and Modify Cart** use case for a restaurant ordering system.

## Actors

- **Customer**: Uses the online ordering interface.
- **Restaurant Staff**: Uses the restaurant ordering interface for an in-store customer.

The application validates products, updates the cart, persists data, and returns the result.

## Included Files

| No. | File | Purpose |
|---|---|---|
| 01 | `01- FlowChart - Add-Modify-Cart.png` | High-level add and modify cart process. |
| 02A | `02- SequeneceDiagram - Add-Cart-UseCase.png` | Add-item interaction between the actor, UI, Cart, Menu, and DB. |
| 02B | `02- SequeneceDiagram - Modify-Cart-UseCase.png` | Modify-item interaction and unavailable-item failure flow. |
| 03 | `03- ERD - Add-Modify-Cart.png` | Cart entities and their relationships. |
| 04 | `04- PseudoCode - Add-Modify-Cart.md` | Application and persistence pseudocode. |

## Supported Operations

- Browse the menu and select a product.
- Add a product to the cart.
- Add another product to the same cart.
- Change quantity.
- Remove an item.
- Add or update notes.
- Validate branch and product availability.
- Recalculate subtotal and total.
- Persist and return the updated cart.
- Return an error when a product is unavailable.

## Main Entities

- `CUSTOMER`
- `BRANCH`
- `CART`
- `CART_ITEM`
- `PRODUCT`

## Main Relationships

- A customer can have zero or more carts.
- A branch can have zero or more carts.
- A cart can have zero or more cart items.
- A product can appear in zero or more cart items.

## Cart Ownership

### Online order

- `CART.customer_id` identifies the customer.
- `CART.source` is `ONLINE`.
- `created_by_user_id` can be null.

### Restaurant order

- `CART.created_by_user_id` identifies the restaurant staff user.
- `CART.customer_id` can be null.
- `CART.source` is `RESTAURANT`.

## Audit Columns

The following generic fields are used in the ERD:

- `id`
- `uuid`
- `created`
- `createdby`
- `updated`
- `updatedby`
- `active`
- `ad_org_id`
- `ad_client_id`

## Business Assumptions

- Only an open and active branch accepts cart changes.
- The product price is copied into `CART_ITEM.unit_price` when selected.
- Quantity must be greater than zero.
- Removing an item uses a separate action.
- A matching cart line can be merged by product and notes.
- Cart changes and total recalculation occur in one database transaction.
- Checkout, payment, order creation, promotions, discounts, tax, delivery fees, and inventory reservation are outside this scope.

## Cart Actions

```text
ADD_ITEM
CHANGE_QUANTITY
REMOVE_ITEM
ADD_NOTES
```

## Error Codes

```text
BRANCH_NOT_FOUND
BRANCH_CLOSED
CART_NOT_FOUND
CART_ACCESS_DENIED
CART_ITEM_NOT_FOUND
PRODUCT_NOT_FOUND
ITEM_UNAVAILABLE
INVALID_QUANTITY
CART_UPDATE_FAILED
```
