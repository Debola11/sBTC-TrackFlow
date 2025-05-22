
---

# 📦 sBTC-TrackFlow – A Decentralized Supply Chain Tracking Smart Contract

## 📄 Overview

SupplyChainX is a Clarity-based smart contract for managing a decentralized supply chain network on the Stacks blockchain. It enables verified participants (Manufacturers, Transporters, Retailers) to add, track, and update products while maintaining an immutable history of changes.

This contract can be used for a variety of supply chain scenarios such as food tracking, electronics logistics, medical supply chains, and more.

---

## 🛠 Features

* ✅ **Role-Based Access Control**: Only contract owner can assign or revoke roles.
* 🏷 **Product Creation**: Only manufacturers can create products.
* 🚚 **Product Movement**: Authorized parties (Transporters, Manufacturers) can update the product location.
* 🕓 **History Tracking**: Each location update is recorded with a timestamp.
* 🔍 **Query Support**: Retrieve role details, product information, and movement history.

---

## 📦 Roles

| Role         | Permissions                          |
| ------------ | ------------------------------------ |
| Manufacturer | Create products, update location     |
| Transporter  | Update location                      |
| Retailer     | Read-only access (future extensions) |

---

## 🧱 Data Structures

### 🧑 Roles Map

```clarity
(define-map roles principal 
  { role: (string-utf8 20), is-active: bool })
```

### 📦 Product Map

```clarity
(define-map products uint 
  { id, name, manufacturer, origin, timestamp, current-location, status })
```

### 🕓 Product History Map

```clarity
(define-map product-history 
  {product-id: uint, change-id: uint} 
  { timestamp, location, status })
```

---

## 🔐 Access Control

Only the contract owner (`contract-owner`) can:

* Assign roles (`assign-role`)
* Revoke roles (`revoke-role`)

---

## 🚀 Public Functions

### 1. `assign-role`

Assign a role to a principal.

```clarity
(assign-role address role)
```

### 2. `revoke-role`

Revoke a role from a principal.

```clarity
(revoke-role address)
```

### 3. `add-product`

Create a new product (only by manufacturers).

```clarity
(add-product name origin location)
```

### 4. `update-location`

Update a product’s current location.

```clarity
(update-location product-id new-location)
```

---

## 🔍 Read-Only Functions

### 1. `get-role`

Get the role and active status of an address.

```clarity
(get-role address)
```

### 2. `get-product`

Fetch details of a product by ID.

```clarity
(get-product product-id)
```

### 3. `get-product-history`

Fetch the most recent product history entry (currently limited to `change-id: u0`).

```clarity
(get-product-history product-id)
```

---

## ⚠️ Error Codes

| Code                 | Meaning               |
| -------------------- | --------------------- |
| `ERR-NOT-AUTHORIZED` | Sender not authorized |
| `ERR-INVALID-ROLE`   | Role is not valid     |
| `ERR-NOT-FOUND`      | Entry not found       |
| `ERR-INVALID-INPUT`  | Invalid string length |
| `ERR-ALREADY-EXISTS` | Duplicate entry       |

---

## 📈 Future Improvements

* Add support for Retailer updates and confirmations.
* Enable full product history retrieval (all changes).
* Include product lifecycle transitions (`in-transit`, `delivered`, etc).
* Extend role system to include Admins or Auditors.

---

## 🧠 Example Usage

```clarity
;; Assign a role
(assign-role 'ST123ABC' u"manufacturer")

;; Add a product
(add-product u"Apples" u"USA" u"Warehouse A")

;; Update location
(update-location u0 u"Truck Depot 3")

;; Get product
(get-product u0)

;; Get role
(get-role 'ST123ABC')
```

---