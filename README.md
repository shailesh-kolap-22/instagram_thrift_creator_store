# Instagram Thrift and Handmade Store -- Database Design

Database design (ER diagram) for a small Instagram-based business that sells thrifted fashion items and handmade products.

## Business Context

A small creator sells products through Instagram DMs and WhatsApp. Products fall into two categories:

- **Thrifted** -- second-hand, unique, one-of-a-kind items. Each has a condition (like_new, good, fair, worn). Once sold, it is gone.
- **Handmade** -- created by the owner in batches. Multiple units can exist and stock can be replenished.

## ER Diagram

![ER Diagram](./ER_Diagram.svg)

## Entities

| Entity | Purpose |
|---|---|
| **customer** | Buyer info -- name, phone, email, Instagram handle, address |
| **product** | Catalog of items -- type (thrifted/handmade), price, size, color, condition, stock |
| **order_record** | A purchase event linked to a customer |
| **order_item** | Junction table linking orders to products (resolves many-to-many) |
| **payment** | Payment method, status, amount, and transaction reference for an order |
| **shipment** | Shipping address, method, status, and tracking for an order |

## Relationships

| Relationship | Cardinality |
|---|---|
| customer → order_record | One-to-Many |
| order_record → order_item | One-to-Many |
| product → order_item | One-to-Many |
| order_record → payment | One-to-One |
| order_record → shipment | One-to-One |

## Key Design Decisions

- Single `product` table with a `product_type` column instead of separate tables for thrifted and handmade.
- `condition` is nullable -- only relevant for thrifted items, left NULL for handmade.
- `unit_price` in `order_item` snapshots the price at purchase time so historical orders stay accurate.
- Shipping address stored on `shipment`, not just on `customer`, because delivery address can differ per order.
- `order_record` used instead of `order` to avoid SQL reserved keyword conflicts.
