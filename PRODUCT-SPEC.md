# PRODUCT-SPEC: Real Estate

## Overview

**App Name:** Real Estate
**Domain:** Property listings, tenant management, lease tracking
**Target User:** Real estate agents, property managers, landlords

## Core Entities

### Property
```
Property
├── id: UUID (PK)
├── title: str (required)
├── address: str (required)
├── city: str (required)
├── state: str (required)
├── zip_code: str (required)
├── price: float (required)
├── property_type: enum ["house", "apartment", "condo", "commercial", "land"] (required)
├── bedrooms: int (optional)
├── bathrooms: float (optional)
├── square_feet: int (optional)
├── status: enum ["available", "rented", "sold", "pending"] (default: "available")
├── description: str (optional)
├── created_at: datetime
└── updated_at: datetime
```

### Tenant
```
Tenant
├── id: UUID (PK)
├── first_name: str (required)
├── last_name: str (required)
├── email: str (unique, required)
├── phone: str (optional)
├── property_id: UUID (FK → Property, ondelete=SET NULL, optional)
├── lease_start: date (optional)
├── lease_end: date (optional)
├── rent_amount: float (optional)
├── created_at: datetime
└── updated_at: datetime
```

### MaintenanceRequest
```
MaintenanceRequest
├── id: UUID (PK)
├── property_id: UUID (FK → Property, ondelete=CASCADE)
├── tenant_id: UUID (FK → Tenant, ondelete=SET NULL, optional)
├── title: str (required)
├── description: str (required)
├── priority: enum ["low", "medium", "high", "emergency"] (default: "medium")
├── status: enum ["open", "in_progress", "resolved", "closed"] (default: "open")
├── created_at: datetime
└── updated_at: datetime
```

## User Stories / Screens

### Screen 1: Dashboard
- Summary cards: total properties, occupied units, vacant units, open maintenance requests
- Properties by status chart
- Recent maintenance requests

### Screen 2: Properties
- Card grid with property image placeholder, title, price, status badge
- Property type filter, status filter, price range filter
- "Add Property" form

### Screen 3: Property Detail
- Property info with edit/delete
- Tenant info if occupied
- Maintenance history
- "Add Maintenance Request" button

### Screen 4: Tenants
- Table view with name, property, lease dates, rent
- "Add Tenant" form with property dropdown

### Screen 5: Maintenance Requests
- Table view with priority color-coding, status filter
- "Create Request" form

## AI Features

- **Price estimation:** Estimate property value based on attributes (mock)
- **Tenant screening:** Flag high-risk tenants (mock)

## API Endpoints (v1.0)

```
GET    /api/v1/properties         → List properties
POST   /api/v1/properties         → Create property
GET    /api/v1/properties/{id}    → Get property
PUT    /api/v1/properties/{id}    → Update property
DELETE /api/v1/properties/{id}    → Delete property
GET    /api/v1/tenants            → List tenants
POST   /api/v1/tenants            → Create tenant
GET    /api/v1/tenants/{id}       → Get tenant
PUT    /api/v1/tenants/{id}       → Update tenant
DELETE /api/v1/tenants/{id}       → Delete tenant
GET    /api/v1/maintenance        → List maintenance requests
POST   /api/v1/maintenance        → Create maintenance request
GET    /api/v1/maintenance/{id}   → Get maintenance request
PUT    /api/v1/maintenance/{id}   → Update maintenance request
DELETE /api/v1/maintenance/{id}   → Delete maintenance request
GET    /api/v1/dashboard          → Dashboard stats
```

## Non-Functional Requirements

- Backend tests: 70%+ coverage
- Frontend: Responsive, Tailwind + pre-built UI components
- Docker: All services start with `docker compose up -d`
- No mock data — everything persisted to PostgreSQL
