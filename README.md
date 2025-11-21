# OpenDirect MCP Server v2.1

Complete Model Context Protocol implementation of IAB Tech Lab OpenDirect v2.1 specification for programmatic direct advertising inventory management.

## Quick Links

- [Full Documentation](./OPENDIRECT_DOCUMENTATION.md)
- [IAB Tech Lab OpenDirect](https://iabtechlab.com/opendirect)
- [GitHub Repository](https://github.com/InteractiveAdvertisingBureau/OpenDirect)

## Overview

OpenDirect enables publishers to offer premium guaranteed inventory using a programmatic interface. This MCP server provides complete schema definitions and tools for all OpenDirect v2.1 objects and workflows.

## Installation

```bash
# Copy opendirect-mcp-server.json to your MCP configuration directory
cp opendirect-mcp-server.json ~/.mcp/servers/

# Or use with MCP client
mcp-client connect opendirect-mcp-server.json
```

## Core Resources

- **Organizations** - Advertisers, agencies, publishers
- **Accounts** - Buyer-advertiser relationships
- **Orders** - Campaign plans with budget and dates
- **Lines** - Line items for specific products
- **Products** - Available inventory catalog
- **Creatives** - Ad assets
- **Assignments** - Creative-to-line mappings
- **Placements** - Ad unit specifications
- **Change Requests** - Order modification requests
- **Messages** - Buyer-seller communication

## Referenced Specifications

- **AdCOM** - Advertising Common Object Model
- **OpenRTB** - Real-Time Bidding protocol
- **DP-AA** - Digital Out-of-Home extensions

## Key Features

### Complete Schema Coverage
All OpenDirect v2.1 objects with full property definitions:
- OpenDirect core objects (Account, Order, Line, Product, etc.)
- AdCOM domain objects (Creative specs, Device, Geo, Site, App, etc.)
- OpenRTB objects (Deal, PMP, Regs, Source)

### MCP Tools
10 primary tools for object creation and management:
- `create_account` - Create buyer-advertiser accounts
- `create_order` - Create advertising orders
- `create_line` - Create line items
- `create_creative` - Upload ad creatives
- `create_assignment` - Assign creatives to lines
- `search_products` - Search inventory
- `create_organization` - Register organizations
- `create_change_request` - Request order changes
- `send_message` - Send messages
- `update_line_booking_status` - Update booking status

### Workflow Prompts
Guided workflows for common scenarios:
- `order_workflow` - Complete order placement workflow
- `product_discovery` - Find suitable inventory
- `creative_specs` - Get creative requirements

## Example Usage

### Create Order
```json
{
  "tool": "create_order",
  "arguments": {
    "name": "Q1 2025 Campaign",
    "accountid": "acc_12345",
    "publisherid": "pub_67890",
    "currency": "USD",
    "budget": 500000,
    "startdate": "2025-01-01T00:00:00Z",
    "enddate": "2025-03-31T23:59:59Z"
  }
}
```

### Create Line Item
```json
{
  "tool": "create_line",
  "arguments": {
    "name": "Homepage Banner",
    "orderid": "ord_98765",
    "productid": "prod_homepage_001",
    "startdate": "2025-01-01T00:00:00Z",
    "enddate": "2025-01-31T23:59:59Z",
    "ratetype": "CPM",
    "quantity": 1000000
  }
}
```

## Booking Status Workflow

```
Draft → PendingReservation → Reserved → PendingBooking → Booked → InFlight → Finished
```

Additional states: Stopped, Canceled, Pause, Expired, Declined, ChangePending

## Rate Types

- **CPM** - Cost Per Thousand Impressions
- **CPMV** - Cost Per Thousand Viewable Impressions  
- **CPC** - Cost Per Click
- **CPD** - Cost Per Day
- **FlatRate** - Fixed rate pricing

## Standards Compliance

- ISO-639-1 (Languages)
- ISO-3166 (Countries/Regions)
- ISO-4217 (Currencies)
- IAB Tech Lab Content Taxonomy
- IQG 2.1 Media Ratings

## Documentation

See `OPENDIRECT_DOCUMENTATION.md` for complete details on:
- All object schemas and properties
- Workflow diagrams
- Business rules and validation
- AdCOM specifications
- OpenRTB integration
- Use cases and examples

## License

IAB Technology Laboratory - Creative Commons Attribution 3.0
https://creativecommons.org/licenses/by/3.0/

## Version

OpenDirect v2.1 Final (IAB Tech Lab 2018-2024)
