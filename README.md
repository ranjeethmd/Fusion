# Fusion

A GraphQL Federated Gateway demonstration built with .NET and Hot Chocolate Fusion.

## Overview

Fusion showcases a distributed GraphQL architecture using Hot Chocolate's Fusion technology. Multiple independent GraphQL services (subgraphs) are composed into a single unified GraphQL gateway, enabling clients to query related data across services seamlessly.

## Architecture

```
                    ┌─────────────────────┐
                    │   Fusion Gateway    │
                    │    (Port 5290)      │
                    └─────────┬───────────┘
                              │
              ┌───────────────┴───────────────┐
              │                               │
    ┌─────────▼─────────┐           ┌─────────▼─────────┐
    │  Orders Service   │           │ Products Service  │
    │   (Port 5002)     │           │   (Port 5003)     │
    └───────────────────┘           └───────────────────┘
```

## Services

### Gateway (Port 5290)
Central composition point that federates all subgraph schemas into a unified GraphQL API.

### Orders Subgraph (Port 5002)
Manages order-related entities:
- `Order`: id, name, description, items
- `LineItem`: id, quantity, product (references Products service)

### Products Subgraph (Port 5003)
Manages product catalog:
- `Product`: id, name, sku, description, price

## Prerequisites

- [.NET 10.0 SDK](https://dotnet.microsoft.com/download)

## Getting Started

### 1. Clone the repository

```bash
git clone <repository-url>
cd Fusion
```

### 2. Start the services

Run all three services (each in a separate terminal):

```bash
# Terminal 1 - Products Service
cd quick-start.Products
dotnet run

# Terminal 2 - Orders Service
cd quick-start.Orders
dotnet run

# Terminal 3 - Gateway
cd quick-start.Gateway
dotnet run
```

### 3. Access GraphQL Endpoints

- **Gateway**: http://localhost:5290/graphql
- **Orders**: http://localhost:5002/graphql
- **Products**: http://localhost:5003/graphql

## Example Queries

### Query orders with product details (via Gateway)

```graphql
query {
  orders {
    id
    name
    description
    items {
      id
      quantity
      product {
        id
        name
        price
      }
    }
  }
}
```

### Query all products

```graphql
query {
  products {
    id
    name
    sku
    description
    price
  }
}
```

### Query a specific product

```graphql
query {
  productById(id: 1) {
    id
    name
    price
  }
}
```

## Project Structure

```
Fusion/
├── quick-start.Gateway/      # GraphQL Fusion Gateway
│   ├── Program.cs            # Gateway configuration
│   └── gateway.fgp           # Federation package
├── quick-start.Orders/       # Orders microservice
│   ├── Program.cs
│   ├── schema.graphql
│   └── Types/                # Order, LineItem, Query
└── quick-start.Products/     # Products microservice
    ├── Program.cs
    ├── schema.graphql
    └── Types/                # Product, Query
```

## Technologies

- **.NET 10.0**
- **Hot Chocolate v15** - GraphQL server framework
- **Hot Chocolate Fusion** - GraphQL Federation/Gateway

## Key Features

- **GraphQL Federation**: Independent services composed via gateway
- **Entity References**: Orders reference Products across service boundaries
- **Type Composition**: Unified schema from multiple subgraphs
- **Query Planning**: Gateway optimizes queries across services

## License

MIT
