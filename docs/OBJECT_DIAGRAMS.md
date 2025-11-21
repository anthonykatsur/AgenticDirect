# OpenDirect v2.1 Object Relationships

## Core Object Relationships

```mermaid
graph TD
    Org[Organization] -->|creates| Account[Account]
    Account -->|owns| Order[Order]
    Account -->|owns| Creative[Creative]
    Order -->|contains| Line[Line]
    Line -->|references| Product[Product]
    Line -->|has| Placement[Placement]
    Product -->|contains| AdUnit[AdUnit]
    Placement -->|references| AdUnit
    Creative -->|assigned to| Placement
    Assignment[Assignment] -->|links| Creative
    Assignment -->|links| Placement
    Order -->|can have| ChangeRequest[ChangeRequest]
    Order -->|has| Message[Message]
    Line -->|references in| Message
    
    style Org fill:#e1f5ff
    style Account fill:#fff4e1
    style Order fill:#ffe1e1
    style Line fill:#e1ffe1
    style Product fill:#f0e1ff
    style Creative fill:#ffe1f5
```

## Workflow State Diagram

```mermaid
stateDiagram-v2
    [*] --> Draft
    
    Draft --> PendingReservation: Request Reservation
    Draft --> [*]: Delete
    
    PendingReservation --> Reserved: Publisher Approves
    PendingReservation --> Declined: Publisher Declines
    PendingReservation --> Draft: Cancel Request
    
    Reserved --> PendingBooking: Request Booking
    Reserved --> Expired: Expiry Date Passed
    Reserved --> Draft: Reset
    
    Declined --> Draft: Reset
    
    PendingBooking --> Booked: Publisher Confirms
    PendingBooking --> Declined: Publisher Declines
    PendingBooking --> Reserved: Cancel Request
    
    Booked --> InFlight: Start Date Reached
    Booked --> Stopped: User Stops
    Booked --> Canceled: User Cancels
    Booked --> ChangePending: Change Requested
    
    ChangePending --> Booked: Change Approved
    ChangePending --> Stopped: Change Rejected
    
    InFlight --> Finished: End Date Reached
    InFlight --> Stopped: User Stops
    InFlight --> Canceled: User Cancels
    InFlight --> Pause: User Pauses
    
    Pause --> InFlight: User Resumes
    Pause --> Stopped: User Stops
    Pause --> Canceled: User Cancels
    
    Stopped --> [*]
    Canceled --> [*]
    Finished --> [*]
    Expired --> [*]
```

## Account and Organization Hierarchy

```mermaid
graph TB
    subgraph Organizations
        Advertiser[Advertiser Organization]
        Agency[Agency/Buyer Organization]
        Publisher[Publisher Organization]
    end
    
    subgraph Account Creation
        Advertiser -->|grants permission| Permission[Permission Grant]
        Permission -->|enables| Agency
        Agency -->|creates| Account[Account]
        Advertiser -->|linked in| Account
    end
    
    subgraph Order Management
        Account -->|creates| Order[Order]
        Publisher -->|provides| Product[Product]
        Order -->|contains| Line[Line]
        Line -->|books| Product
    end
    
    subgraph Creative Flow
        Account -->|uploads| Creative[Creative]
        Publisher -->|approves| Creative
        Creative -->|assigned via| Assignment[Assignment]
        Assignment -->|to| Line
    end
    
    style Advertiser fill:#e3f2fd
    style Agency fill:#fff3e0
    style Publisher fill:#f3e5f5
    style Account fill:#c8e6c9
    style Order fill:#ffccbc
    style Creative fill:#f8bbd0
```

## Complete Booking Workflow

```mermaid
sequenceDiagram
    participant Buyer
    participant Publisher
    participant System
    
    Note over Buyer,Publisher: 1. Onboarding Phase
    Buyer->>System: Create Organization
    System->>Buyer: Organization (Pending)
    Publisher->>System: Approve Organization
    System->>Buyer: Organization (Approved)
    
    Note over Buyer,Publisher: 2. Account Setup
    Buyer->>System: Create Account
    System-->>Publisher: Notify New Account
    
    Note over Buyer,Publisher: 3. Discovery
    Buyer->>System: Search Products
    System->>Buyer: Product Catalog
    Buyer->>System: Get Product Details
    System->>Buyer: Product Specifications
    
    Note over Buyer,Publisher: 4. Order Creation
    Buyer->>System: Create Order (Draft)
    System->>Buyer: Order ID
    
    Note over Buyer,Publisher: 5. Line Item Creation
    Buyer->>System: Create Line (Draft)
    System->>Buyer: Line ID
    
    Note over Buyer,Publisher: 6. Reservation
    Buyer->>System: Update Line (PendingReservation)
    System->>Publisher: Reservation Request
    Publisher->>System: Approve/Decline Reservation
    System->>Buyer: Line (Reserved/Declined)
    
    Note over Buyer,Publisher: 7. Creative Upload
    Buyer->>System: Create Creative
    System->>Publisher: Creative for Approval
    Publisher->>System: Approve Creative
    System->>Buyer: Creative (Approved)
    
    Note over Buyer,Publisher: 8. Assignment
    Buyer->>System: Create Placement
    System->>Buyer: Placement ID
    Buyer->>System: Create Assignment
    System->>Buyer: Assignment (Active)
    
    Note over Buyer,Publisher: 9. Booking
    Buyer->>System: Update Line (PendingBooking)
    System->>Publisher: Booking Request
    Publisher->>System: Confirm Booking
    System->>Buyer: Line (Booked)
    
    Note over Buyer,Publisher: 10. Campaign Execution
    System->>System: Start Date Reached
    System->>Buyer: Line (InFlight)
    System->>System: End Date Reached
    System->>Buyer: Line (Finished)
```

## Creative Assignment Flow

```mermaid
graph LR
    subgraph Line Configuration
        Line[Line Item] -->|references| Product[Product]
        Product -->|defines| AdUnit[Ad Unit]
    end
    
    subgraph Creative Management
        Account[Account] -->|uploads| Creative[Creative]
        Creative -->|contains| CreativeAsset[Creative Asset]
        Publisher -->|approves| Creative
    end
    
    subgraph Assignment Process
        Line -->|creates| Placement[Placement]
        Placement -->|references| AdUnit
        Placement -->|specs from| AdUnit
        Assignment[Assignment] -->|links| Creative
        Assignment -->|to| Placement
        Assignment -->|weight| Rotation[Rotation Weight]
    end
    
    subgraph Serving
        Placement -->|serves| CreativeAsset
        Rotation -->|controls| CreativeAsset
    end
    
    style Line fill:#e1f5ff
    style Creative fill:#ffe1f5
    style Placement fill:#f0e1ff
    style Assignment fill:#fff4e1
```

## Data Model - AdCOM Integration

```mermaid
graph TB
    subgraph OpenDirect Objects
        Product[Product]
        Creative[Creative]
        Line[Line]
        Placement[Placement]
    end
    
    subgraph AdCOM Specifications
        Product -->|contains| AdUnit[AdUnit]
        AdUnit -->|has| CreativeSpec[CreativeSpec]
        
        CreativeSpec -->|may include| VideoSpec[VideoSpec]
        CreativeSpec -->|may include| DisplaySpec[DisplaySpec]
        CreativeSpec -->|may include| AudioSpec[AudioSpec]
        
        DisplaySpec -->|contains| BannerSpec[BannerSpec]
        DisplaySpec -->|contains| NativeSpec[NativeSpec]
        DisplaySpec -->|contains| AMPSpec[AMPSpec]
    end
    
    subgraph AdCOM Responses
        Creative -->|contains| CreativeResp[CreativeResp]
        CreativeResp -->|may have| VideoResp[VideoResp]
        CreativeResp -->|may have| DisplayResp[DisplayResp]
        CreativeResp -->|may have| AudioResp[AudioResp]
    end
    
    subgraph Context Objects
        Product -->|may reference| Site[Site]
        Product -->|may reference| App[App]
        Product -->|may reference| Device[Device]
        Product -->|may reference| Geo[Geo]
        
        Site -->|has| Publisher[Publisher]
        Site -->|has| Content[Content]
        App -->|has| Publisher
        App -->|has| Content
        Content -->|has| Producer[Producer]
    end
    
    subgraph User Data
        Line -->|targeting| Segment[Segment]
        Segment -->|part of| Data[Data]
        User[User] -->|has| Data
        User -->|has| Geo
    end
    
    style Product fill:#e1f5ff
    style Creative fill:#ffe1f5
    style CreativeSpec fill:#f0e1ff
    style CreativeResp fill:#ffe1e1
```

## Private Marketplace Integration

```mermaid
graph TD
    subgraph OpenDirect
        Product[Product] -->|contains| PMP1[PMP]
        Line[Line] -->|references| PMP2[PMP]
    end
    
    subgraph OpenRTB PMP
        PMP1 -->|has| Deals1[Deals Array]
        PMP2 -->|has| Deals2[Deals Array]
        
        Deals1 -->|contains| Deal1[Deal]
        Deals2 -->|contains| Deal2[Deal]
        
        Deal1 -->|defines| BidFloor[Bid Floor]
        Deal1 -->|defines| AuctionType[Auction Type]
        Deal1 -->|defines| WSeat[Whitelisted Seats]
        Deal1 -->|defines| WADomain[Whitelisted Domains]
        
        Deal2 -->|same structure| BidFloor
    end
    
    subgraph Deal Types
        AuctionType -->|1| FirstPrice[First Price]
        AuctionType -->|2| SecondPrice[Second Price Plus]
        AuctionType -->|3| FixedPrice[Fixed Price / Deal Price]
    end
    
    subgraph Private Auction
        PMP1 -->|private_auction flag| Restriction[Bid Restrictions]
        Restriction -->|0| AllBids[All Bids Accepted]
        Restriction -->|1| DealsOnly[Deals Only]
    end
    
    style Product fill:#e1f5ff
    style Line fill:#e1ffe1
    style PMP1 fill:#f0e1ff
    style Deal1 fill:#ffe1e1
```

## Change Request Workflow

```mermaid
sequenceDiagram
    participant Buyer
    participant System
    participant Publisher
    participant Webhook
    
    Note over Buyer,Publisher: Change Request Initiated
    Buyer->>System: Create ChangeRequest
    System->>Buyer: ChangeRequest ID (PENDING)
    System->>Publisher: Notify Change Request
    
    Note over Buyer,Publisher: Optional Communication
    Buyer->>System: Send Message about Change
    System->>Publisher: Deliver Message
    Publisher->>System: Reply to Message
    System->>Buyer: Deliver Reply
    
    Note over Buyer,Publisher: Publisher Decision
    alt Approve Change
        Publisher->>System: Approve ChangeRequest
        System->>System: Update Status (APPROVED)
        System->>Webhook: POST ChangeRequest (APPROVED)
        System->>Buyer: Notify Approval
        Buyer->>System: Modify Order/Lines
        System->>Buyer: Changes Applied
    else Reject Change
        Publisher->>System: Reject ChangeRequest
        System->>System: Update Status (REJECTED)
        System->>Webhook: POST ChangeRequest (REJECTED)
        System->>Buyer: Notify Rejection
    end
```

## Message Thread Flow

```mermaid
graph TD
    Order[Order] -->|has| Message1[Message 1]
    Message1 -->|sender| Contact1[Buyer Contact]
    Message1 -->|recipient| Contact2[Publisher Contact]
    Message1 -->|status| Status1[New]
    
    Contact2 -->|reads| Message1
    Status1 -->|becomes| Status2[Read]
    
    Message1 -->|reply creates| Message2[Message 2]
    Message2 -->|replytomessageid| Message1
    Message2 -->|sender| Contact2
    Message2 -->|recipient| Contact1
    
    Message2 -->|reply creates| Message3[Message 3]
    Message3 -->|replytomessageid| Message2
    Message3 -->|sender| Contact1
    Message3 -->|recipient| Contact2
    
    Order -->|references| LineIDs[Line IDs Array]
    Message1 -.->|may reference| LineIDs
    Message2 -.->|may reference| LineIDs
    
    Order -->|may have| ChangeRequest[Change Request]
    Message1 -.->|may reference| ChangeRequest
    Message2 -.->|may reference| ChangeRequest
    
    style Order fill:#ffe1e1
    style Message1 fill:#fff4e1
    style Message2 fill:#e1ffe1
    style Message3 fill:#e1f5ff
```

## Rate Type Application

```mermaid
graph LR
    subgraph Rate Models
        CPM[CPM - Cost Per Mille] -->|applied to| Impressions[Impression Count]
        CPMV[CPMV - Cost Per Viewable Mille] -->|applied to| ViewableImps[Viewable Impressions]
        CPC[CPC - Cost Per Click] -->|applied to| Clicks[Click Count]
        CPD[CPD - Cost Per Day] -->|applied to| Days[Day Count]
        FlatRate[Flat Rate] -->|applied to| Fixed[Fixed Cost]
    end
    
    subgraph Line Calculation
        Impressions -->|quantity| LineQty[Line Quantity]
        ViewableImps -->|quantity| LineQty
        Clicks -->|quantity| LineQty
        Days -->|quantity| LineQty
        Fixed -->|quantity| LineQty
        
        LineQty -->|multiplied by| Rate[Rate]
        Rate -->|equals| Cost[Line Cost]
    end
    
    subgraph Order Total
        Cost -->|sum of all lines| OrderCost[Order Total Cost]
        OrderCost -->|compared to| Budget[Order Budget]
        Budget -->|directional only| Note[Budget is Estimate]
    end
```

## Organization Status Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Pending: Create Organization
    
    Pending --> Approved: Publisher Approves
    Pending --> Disapproved: Publisher Rejects
    Pending --> Limited: Publisher Restricts
    
    Approved --> Limited: Restrictions Applied
    Approved --> Disapproved: Violations Found
    
    Limited --> Approved: Restrictions Lifted
    Limited --> Disapproved: Further Violations
    
    Disapproved --> Pending: Re-application
    
    note right of Approved: Can create Accounts,\nOrders, Lines
    
    note right of Limited: Restricted capabilities,\nsome actions blocked
    
    note right of Disapproved: Cannot transact,\nmust re-apply
```

These diagrams provide a comprehensive visual reference for the OpenDirect v2.1 object model, relationships, and workflows.
