# OpenDirect MCP Implementation Examples

## Table of Contents
1. [Complete Workflow Example](#complete-workflow-example)
2. [Object Creation Examples](#object-creation-examples)
3. [AdCOM Integration Examples](#adcom-integration-examples)
4. [Query Examples](#query-examples)
5. [Error Handling](#error-handling)

## Complete Workflow Example

### Scenario: Agency Books Premium Homepage Campaign

```javascript
// Step 1: Create Agency Organization
{
  "tool": "create_organization",
  "arguments": {
    "name": "Digital Marketing Agency Inc",
    "address": {
      "addressline1": "500 Madison Avenue",
      "city": "New York",
      "state": "NY",
      "country": "US",
      "postalcode": "10022"
    },
    "contacts": [
      {
        "firstname": "Sarah",
        "lastname": "Johnson",
        "email": "sarah.johnson@dma.com",
        "phone": "+1-212-555-0100",
        "type": "Billing"
      },
      {
        "firstname": "Mike",
        "lastname": "Chen",
        "email": "mike.chen@dma.com",
        "phone": "+1-212-555-0101",
        "type": "Buyer"
      }
    ],
    "phone": "+1-212-555-0100",
    "url": "https://www.dma.com"
  }
}

// Step 2: Create Advertiser Organization
{
  "tool": "create_organization",
  "arguments": {
    "name": "Global Retail Corp",
    "address": {
      "addressline1": "1000 Commerce Blvd",
      "city": "San Francisco",
      "state": "CA",
      "country": "US",
      "postalcode": "94102"
    },
    "contacts": [
      {
        "firstname": "Jennifer",
        "lastname": "Martinez",
        "email": "j.martinez@globalretail.com",
        "phone": "+1-415-555-0200",
        "type": "Billing"
      }
    ],
    "cat": ["IAB19"],  // Technology & Computing
    "url": "https://www.globalretail.com"
  }
}

// Step 3: Create Account (after organizations approved)
{
  "tool": "create_account",
  "arguments": {
    "advertiserid": "org_global_retail_001",
    "buyerid": "org_dma_001",
    "name": "Global Retail - Holiday Campaign 2025"
  }
}

// Step 4: Search Products
{
  "tool": "search_products",
  "arguments": {
    "publisherid": "org_premium_publisher_001",
    "ratetype": "CPM",
    "tags": ["homepage", "premium", "above-fold"]
  }
}

// Step 5: Create Order
{
  "tool": "create_order",
  "arguments": {
    "name": "Holiday 2025 - Homepage Takeover",
    "accountid": "acc_global_retail_001",
    "publisherid": "org_premium_publisher_001",
    "brand": "Global Retail",
    "cat": ["IAB19-6"],  // Shopping
    "currency": "USD",
    "budget": 750000,
    "startdate": "2025-11-15T00:00:00-05:00",
    "enddate": "2025-12-31T23:59:59-05:00",
    "preferredbillingmethod": "Electronic",
    "contacts": [
      {
        "firstname": "Mike",
        "lastname": "Chen",
        "email": "mike.chen@dma.com",
        "phone": "+1-212-555-0101",
        "type": "Buyer"
      }
    ]
  }
}

// Step 6: Create Line Item
{
  "tool": "create_line",
  "arguments": {
    "name": "Homepage Hero Banner - Week 1",
    "orderid": "ord_holiday_2025_001",
    "productid": "prod_homepage_hero_banner",
    "bookingstatus": "Draft",
    "startdate": "2025-11-15T00:00:00-05:00",
    "enddate": "2025-11-21T23:59:59-05:00",
    "ratetype": "CPM",
    "quantity": 5000000,
    "frequencycount": 3,
    "frequencyinterval": "Day",
    "comment": "Premium homepage placement for Black Friday launch",
    "targeting": {
      "id": "seg_high_value_shoppers",
      "name": "High-Value Holiday Shoppers",
      "value": "income_100k_plus,age_25_54,interest_shopping"
    }
  }
}

// Step 7: Create Creative
{
  "tool": "create_creative",
  "arguments": {
    "accountid": "acc_global_retail_001",
    "name": "Holiday Sale Banner 970x250",
    "language": "en",
    "creativeasset": {
      "display": {
        "adm": "<div class='ad-container'>...</div>",
        "w": 970,
        "h": 250,
        "api": 5  // MRAID-2
      },
      "attr": [1, 2, 3]  // Audio: Auto-Play, User Initiated, Expandable
    },
    "clickurl": "https://www.globalretail.com/holiday-sale?utm_campaign=homepage",
    "iqgmediarating": 1  // All Audiences
  }
}

// Step 8: Create Assignment
{
  "tool": "create_assignment",
  "arguments": {
    "creativeid": "cre_holiday_banner_970x250",
    "placementid": "plc_homepage_hero_001",
    "weight": 100
  }
}

// Step 9: Update Line to Reserve
{
  "tool": "update_line_booking_status",
  "arguments": {
    "lineid": "line_homepage_week1_001",
    "bookingstatus": "PendingReservation"
  }
}

// Step 10: Publisher Approves - Line moves to Reserved
// (Publisher system updates status)

// Step 11: Book the Line
{
  "tool": "update_line_booking_status",
  "arguments": {
    "lineid": "line_homepage_week1_001",
    "bookingstatus": "PendingBooking"
  }
}

// Step 12: Publisher Confirms - Line moves to Booked
// Campaign is now confirmed and will go live on start date
```

## Object Creation Examples

### Video Creative with VAST
```json
{
  "tool": "create_creative",
  "arguments": {
    "accountid": "acc_12345",
    "name": "15-Second Pre-Roll Video",
    "language": "en",
    "creativeasset": {
      "video": {
        "curl": "https://cdn.example.com/vast/video_001.xml",
        "subtype": 1,
        "qagmediarating": 2,
        "api": 2,
        "mimes": ["video/mp4", "video/webm"]
      },
      "attr": [16]  // VAST 4.0
    }
  }
}
```

### Native Creative
```json
{
  "tool": "create_creative",
  "arguments": {
    "accountid": "acc_12345",
    "name": "Native Article Sponsorship",
    "language": "en",
    "creativeasset": {
      "display": {
        "subtype": 4,
        "adm": "{\"native\":{\"ver\":\"1.2\",\"assets\":[...]}}",
        "w": 0,
        "h": 0
      }
    },
    "clickurl": "https://www.brand.com/article"
  }
}
```

### Audio Creative with Companion
```json
{
  "tool": "create_creative",
  "arguments": {
    "accountid": "acc_12345",
    "name": "30-Second Audio Spot with Banner",
    "language": "en",
    "creativeasset": {
      "audio": {
        "curl": "https://cdn.example.com/daast/audio_001.xml",
        "subtype": 1,
        "companionad": [
          {
            "adm": "<div>...</div>",
            "w": 300,
            "h": 250
          }
        ]
      }
    }
  }
}
```

### Product with Multiple Ad Units
```json
{
  "name": "Multi-Format Homepage Package",
  "publisherid": "pub_001",
  "description": "Homepage takeover with hero, sidebar, and footer units",
  "currency": "USD",
  "baseprice": 50,
  "ratetype": "CPM",
  "deliverytype": "Guaranteed",
  "adunit": {
    "id": "au_homepage_package",
    "name": "Homepage Package",
    "creativespec": {
      "display": {
        "bannerformat": [
          {"w": 970, "h": 250},
          {"w": 300, "h": 600},
          {"w": 728, "h": 90}
        ]
      }
    }
  },
  "alladunits": 1,
  "minflight": 7,
  "maxflight": 30,
  "leadtime": 3,
  "languages": ["en", "es"],
  "geo": {
    "country": "USA",
    "region": "NY"
  },
  "device": {
    "devicetype": [1, 4, 5]
  }
}
```

### Order with Multiple Contacts
```json
{
  "tool": "create_order",
  "arguments": {
    "name": "Multi-Market Campaign Q1",
    "accountid": "acc_12345",
    "publisherid": "pub_001",
    "currency": "USD",
    "budget": 1000000,
    "startdate": "2025-01-01T00:00:00Z",
    "enddate": "2025-03-31T23:59:59Z",
    "contacts": [
      {
        "firstname": "Alice",
        "lastname": "Smith",
        "email": "billing@agency.com",
        "type": "Billing",
        "address": {
          "addressline1": "123 Main St",
          "city": "Boston",
          "country": "US"
        }
      },
      {
        "firstname": "Bob",
        "lastname": "Johnson",
        "email": "bob@agency.com",
        "type": "Buyer"
      },
      {
        "firstname": "Carol",
        "lastname": "Williams",
        "email": "creative@agency.com",
        "type": "Creative"
      }
    ]
  }
}
```

## AdCOM Integration Examples

### Line with Detailed Targeting
```json
{
  "tool": "create_line",
  "arguments": {
    "name": "Targeted Campaign Line",
    "orderid": "ord_001",
    "productid": "prod_001",
    "startdate": "2025-01-01T00:00:00Z",
    "enddate": "2025-01-31T23:59:59Z",
    "ratetype": "CPM",
    "quantity": 10000000,
    "targeting": {
      "name": "Premium Audience Segment",
      "segment": [
        {
          "id": "demo_age",
          "name": "Age 25-54",
          "value": "25-54"
        },
        {
          "id": "demo_income",
          "name": "HHI $75K+",
          "value": "75000+"
        },
        {
          "id": "interest_tech",
          "name": "Technology Enthusiasts",
          "value": "tech_early_adopter"
        }
      ]
    },
    "pmp": {
      "private_auction": 1,
      "deals": [
        {
          "id": "deal_premium_001",
          "bidfloor": 15.00,
          "bidfloorcur": "USD",
          "at": 3,
          "wseat": ["seat_agency_001"],
          "wadomain": ["brand.com"]
        }
      ]
    }
  }
}
```

### Product with Device and Geo Targeting
```json
{
  "name": "Mobile App Interstitial - US Only",
  "publisherid": "pub_001",
  "currency": "USD",
  "baseprice": 8.50,
  "ratetype": "CPM",
  "deliverytype": "Guaranteed",
  "adunit": {
    "id": "au_mobile_interstitial",
    "name": "Mobile Full Screen",
    "creativespec": {
      "display": {
        "bannerformat": {
          "w": 320,
          "h": 480,
          "instl": 1
        }
      }
    }
  },
  "device": {
    "devicetype": [4, 5],
    "os": ["iOS", "Android"],
    "carrier": ["Verizon", "AT&T", "T-Mobile"],
    "conntype": [2, 6]
  },
  "geo": {
    "country": "USA",
    "type": 2
  },
  "regs": {
    "coppa": 0
  }
}
```

### Creative with Site Context
```json
{
  "accountid": "acc_12345",
  "name": "Contextual Banner - Tech News",
  "language": "en",
  "creativeasset": {
    "display": {
      "adm": "<div>...</div>",
      "w": 728,
      "h": 90
    }
  },
  "ext": {
    "preferred_context": {
      "site": {
        "cat": ["IAB19"],
        "sectioncat": ["IAB19-6", "IAB19-18"],
        "keywords": "technology,gadgets,innovation",
        "content": {
          "context": 1,
          "prodq": 2,
          "cat": ["IAB19"]
        }
      }
    }
  }
}
```

## Query Examples

### Search Products by Criteria
```json
{
  "tool": "search_products",
  "arguments": {
    "publisherid": "pub_001",
    "ratetype": "CPM",
    "tags": ["premium", "video", "preroll"],
    "filters": {
      "min_price": 10.00,
      "max_price": 50.00,
      "languages": ["en"],
      "delivery_types": ["Guaranteed", "PMP - Prioritized"]
    }
  }
}
```

### Get Products by Category
```json
{
  "query": {
    "resource": "opendirect://products",
    "filters": {
      "cat": ["IAB1", "IAB19"],
      "active": true,
      "min_avails": 1000000
    },
    "sort": "baseprice",
    "limit": 50
  }
}
```

### Find Lines by Status
```json
{
  "query": {
    "resource": "opendirect://lines",
    "filters": {
      "orderid": "ord_001",
      "bookingstatus": ["Reserved", "Booked", "InFlight"]
    }
  }
}
```

### Get Creative Approvals
```json
{
  "query": {
    "resource": "opendirect://creatives",
    "filters": {
      "accountid": "acc_12345",
      "approval_status": "Pending"
    },
    "include": ["creativeapprovals"]
  }
}
```

## Change Request Examples

### Request Line Quantity Change
```json
{
  "tool": "create_change_request",
  "arguments": {
    "orderid": "ord_001",
    "accountid": "acc_12345",
    "requesterid": "org_agency_001",
    "comments": "Requesting quantity increase from 5M to 7.5M impressions due to increased campaign budget. Line: line_001",
    "webhook": "https://api.agency.com/webhooks/opendirect/change-status"
  }
}
```

### Request Date Extension
```json
{
  "tool": "create_change_request",
  "arguments": {
    "orderid": "ord_002",
    "accountid": "acc_12345",
    "requesterid": "org_agency_001",
    "comments": "Extend campaign end date from 2025-01-31 to 2025-02-15 due to strong performance. All line items affected.",
    "contacts": [
      {
        "firstname": "Jane",
        "lastname": "Doe",
        "email": "jane@agency.com",
        "type": "Buyer"
      }
    ]
  }
}
```

## Message Examples

### Send Order Question
```json
{
  "tool": "send_message",
  "arguments": {
    "orderid": "ord_001",
    "sender": {
      "firstname": "Mike",
      "lastname": "Chen",
      "email": "mike@agency.com",
      "type": "Buyer"
    },
    "recipient": {
      "firstname": "Sarah",
      "lastname": "Publisher",
      "email": "sarah@publisher.com",
      "type": "Sales"
    },
    "message": "Can we get availability for an additional 2M impressions on line_homepage_001 if we extend to Feb 15?",
    "replywebhook": "https://api.agency.com/webhooks/opendirect/messages"
  }
}
```

### Reply to Message
```json
{
  "tool": "send_message",
  "arguments": {
    "orderid": "ord_001",
    "replytomessageid": "msg_12345",
    "sender": {
      "firstname": "Sarah",
      "lastname": "Publisher",
      "email": "sarah@publisher.com",
      "type": "Sales"
    },
    "recipient": {
      "firstname": "Mike",
      "lastname": "Chen",
      "email": "mike@agency.com",
      "type": "Buyer"
    },
    "message": "Yes, we have availability. I'll create a change request to extend the dates and update the quantity."
  }
}
```

## Error Handling

### Standard Error Response
```json
{
  "errors": [
    {
      "ErrorCode": "VALIDATION_ERROR",
      "ErrorMessage": "Line quantity exceeds available inventory",
      "Context": {
        "lineid": "line_001",
        "requested_quantity": 10000000,
        "available_quantity": 5000000,
        "productid": "prod_001"
      },
      "Link": "https://docs.publisher.com/errors/inventory-availability"
    }
  ]
}
```

### Authentication Error
```json
{
  "errors": [
    {
      "ErrorCode": "AUTH_REQUIRED",
      "ErrorMessage": "Valid OAuth 2.0 access token required",
      "Context": {
        "header": "Authorization",
        "expected_format": "Bearer <token>"
      },
      "Link": "https://docs.publisher.com/authentication"
    }
  ]
}
```

### Validation Error
```json
{
  "errors": [
    {
      "ErrorCode": "INVALID_STATUS_TRANSITION",
      "ErrorMessage": "Cannot transition from 'Booked' to 'Draft'",
      "Context": {
        "lineid": "line_001",
        "current_status": "Booked",
        "requested_status": "Draft",
        "allowed_transitions": ["InFlight", "Stopped", "Canceled"]
      },
      "Link": "https://docs.publisher.com/workflows/booking-status"
    }
  ]
}
```

### Business Rule Error
```json
{
  "errors": [
    {
      "ErrorCode": "CREATIVE_LANGUAGE_MISMATCH",
      "ErrorMessage": "Creative language does not match product languages",
      "Context": {
        "creativeid": "cre_001",
        "creative_language": "fr",
        "productid": "prod_001",
        "product_languages": ["en", "es"]
      },
      "Link": "https://docs.publisher.com/creative-requirements"
    }
  ]
}
```

### Permission Error
```json
{
  "errors": [
    {
      "ErrorCode": "PERMISSION_DENIED",
      "ErrorMessage": "Advertiser has not granted permissions to buyer",
      "Context": {
        "advertiserid": "org_advertiser_001",
        "buyerid": "org_agency_001",
        "required_permission": "CREATE_ACCOUNT"
      },
      "Link": "https://docs.publisher.com/permissions"
    }
  ]
}
```

## Best Practices

### 1. Always Validate Before Booking
```javascript
// Check product availability
search_products({...})

// Validate creative against product specs
// Ensure language matches
// Verify format compatibility

// Create line in Draft
create_line({bookingstatus: "Draft", ...})

// Review and adjust
// Then move to PendingReservation
```

### 2. Use Frequency Capping Appropriately
```javascript
{
  "frequencycount": 3,
  "frequencyinterval": "Day"  // Max 3 impressions per user per day
}

{
  "frequencycount": 10,
  "frequencyinterval": "LineDuration"  // Max 10 over entire campaign
}
```

### 3. Leverage Private Marketplace Deals
```javascript
{
  "pmp": {
    "private_auction": 1,
    "deals": [
      {
        "id": "exclusive_deal_001",
        "at": 3,  // Fixed price
        "bidfloor": 25.00,
        "wseat": ["our_seat_id"]
      }
    ]
  }
}
```

### 4. Use Change Requests for Modifications
```javascript
// Instead of direct edits after booking:
// 1. Create change request
create_change_request({...})

// 2. Wait for approval
// 3. Make changes
// 4. Status updates via webhook
```

### 5. Monitor Creative Approvals
```javascript
// Check approval status regularly
{
  "query": "creatives",
  "filters": {
    "accountid": "acc_12345",
    "approvalstatus": "Pending"
  }
}

// Follow up on rejected creatives
// Resubmit after fixes
```

This implementation guide provides comprehensive examples for working with the OpenDirect MCP server.
