# OpenDirect MCP Implementation Guide

A practical guide for implementing the IAB OpenDirect standard using the provided MCP schemas and type definitions.

## Table of Contents
1. [Quick Start](#quick-start)
2. [TypeScript Implementation](#typescript-implementation)
3. [Python Implementation](#python-implementation)
4. [Common Patterns](#common-patterns)
5. [Error Handling](#error-handling)
6. [Testing Strategies](#testing-strategies)
7. [Production Considerations](#production-considerations)

## Quick Start

### Prerequisites
- Understanding of programmatic advertising concepts
- OAuth 2.0 authentication knowledge
- RESTful API experience
- Publisher API credentials

### Installation

#### TypeScript/Node.js
```bash
npm install axios
npm install @types/node
```

#### Python
```bash
pip install requests
pip install pydantic  # Optional: for enhanced validation
```

## TypeScript Implementation

### 1. Setting Up the Client

```typescript
import axios, { AxiosInstance } from 'axios';
import { OpenDirectConfig, IOpenDirectClient, Account, Order, Line } from './opendirect_mcp_types';

class OpenDirectClient implements IOpenDirectClient {
  private client: AxiosInstance;
  private config: OpenDirectConfig;

  constructor(config: OpenDirectConfig) {
    this.config = config;
    this.client = axios.create({
      baseURL: `${config.baseUrl}/api/${config.version}`,
      headers: {
        'AccessToken': config.accessToken,
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      }
    });
  }

  // Account operations
  async getAccount(id: string): Promise<Account> {
    const response = await this.client.get(`/accounts/${id}`);
    return response.data;
  }

  async listAccounts(): Promise<CollectionResponse<Account>> {
    const response = await this.client.get('/accounts');
    return response.data;
  }

  async createAccount(account: Account): Promise<Account> {
    const response = await this.client.post('/accounts', account);
    return response.data;
  }

  async updateAccount(id: string, account: Partial<Account>): Promise<Account> {
    const response = await this.client.patch(`/accounts/${id}`, account);
    return response.data;
  }

  // Line operations with state management
  async reserveLine(accountId: string, orderId: string, lineId: string): Promise<Line> {
    const response = await this.client.put(
      `/accounts/${accountId}/orders/${orderId}/lines/${lineId}?reserve`
    );
    return response.data;
  }

  async bookLine(accountId: string, orderId: string, lineId: string): Promise<Line> {
    const response = await this.client.put(
      `/accounts/${accountId}/orders/${orderId}/lines/${lineId}?book`
    );
    return response.data;
  }

  // Product availability check
  async checkAvailability(request: ProductAvails): Promise<ProductAvails> {
    const response = await this.client.post('/products/avails', request);
    return response.data;
  }
}
```

### 2. Complete Campaign Creation Flow

```typescript
async function createCampaign() {
  const client = new OpenDirectClient({
    baseUrl: 'https://api.publisher.com',
    version: 'v1',
    accessToken: 'your-oauth-token'
  });

  try {
    // Step 1: Create account (if not exists)
    const account = await client.createAccount({
      AdvertiserId: 'adv-12345',
      BuyerId: 'buyer-67890',
      Name: 'Q4 2024 Campaign Account'
    });
    console.log(`✓ Created account: ${account.Id}`);

    // Step 2: Search for products
    const products = await client.searchProducts({
      AdFormatTypes: ['Banner'],
      Geometry: [{ Height: 250, Width: 300 }],
      DeliveryType: DeliveryType.Guaranteed
    });
    console.log(`✓ Found ${products.items.length} products`);

    // Step 3: Check availability
    const avails = await client.checkAvailability({
      AccountId: account.Id,
      StartDate: '2024-10-01T00:00:00Z',
      EndDate: '2024-10-31T23:59:59Z',
      Quantity: 1000000,
      Currency: 'USD'
    });
    console.log(`✓ Availability checked for ${avails.Avails.length} products`);

    // Step 4: Create order
    const order = await client.createOrder(account.Id, {
      AccountId: account.Id,
      Brand: 'Example Brand',
      Name: 'Q4 Display Campaign',
      Budget: 50000,
      Currency: 'USD',
      StartDate: '2024-10-01T00:00:00Z',
      EndDate: '2024-10-31T23:59:59Z'
    });
    console.log(`✓ Created order: ${order.Id}`);

    // Step 5: Create line with targeting
    const line = await client.createLine(account.Id, order.Id, {
      OrderId: order.Id,
      ProductId: products.items[0].Id,
      Name: 'Homepage Banner - Desktop',
      Quantity: 1000000,
      Rate: 5.00,
      RateType: RateType.CPM,
      StartDate: '2024-10-01T00:00:00Z',
      EndDate: '2024-10-31T23:59:59Z',
      FrequencyCount: 3,
      FrequencyInterval: FrequencyInterval.Day,
      Targeting: [
        {
          Target: 'Age',
          TargetValues: ['25-34', '35-44']
        },
        {
          Target: 'Geography',
          TargetValues: ['US-CA', 'US-NY']
        }
      ]
    });
    console.log(`✓ Created line: ${line.Id}`);

    // Step 6: Upload creative
    const creative = await client.createCreative(account.Id, {
      AccountId: account.Id,
      Name: 'Q4 Banner Creative',
      AdFormatType: 'Banner',
      ClickUrl: 'https://example.com/landing',
      Geometry: [{ Height: 250, Width: 300 }],
      Language: 'EN',
      Maturity: MaturityLevel.General,
      HttpsCompatible: true
    });
    console.log(`✓ Created creative: ${creative.Id}`);

    // Step 7: Assign creative to line
    const assignment = await client.createAssignment(account.Id, {
      LineId: line.Id,
      CreativeId: creative.Id,
      Status: AssignmentStatus.Active,
      Weight: 100
    });
    console.log(`✓ Created assignment: ${assignment.Id}`);

    // Step 8: Reserve the line
    const reservedLine = await client.reserveLine(account.Id, order.Id, line.Id);
    console.log(`✓ Reserved line - Status: ${reservedLine.BookingStatus}`);

    // Step 9: Book the line
    const bookedLine = await client.bookLine(account.Id, order.Id, line.Id);
    console.log(`✓ Booked line - Status: ${bookedLine.BookingStatus}`);

    return {
      account,
      order,
      line: bookedLine,
      creative,
      assignment
    };

  } catch (error) {
    console.error('Campaign creation failed:', error);
    throw error;
  }
}
```

## Python Implementation

### 1. Setting Up the Client

```python
import requests
from typing import List, Dict, Any, Optional
from opendirect_mcp_types import *

class OpenDirectClient:
    """OpenDirect API Client Implementation"""
    
    def __init__(self, base_url: str, version: str, access_token: str):
        self.base_url = base_url
        self.version = version
        self.access_token = access_token
        self.session = requests.Session()
        self.session.headers.update({
            'AccessToken': access_token,
            'Content-Type': 'application/json',
            'Accept': 'application/json'
        })
    
    def _build_url(self, path: str) -> str:
        """Build complete API URL"""
        return f"{self.base_url}/api/{self.version}{path}"
    
    def _handle_response(self, response: requests.Response) -> Any:
        """Handle API response with error checking"""
        if response.status_code >= 400:
            error_data = response.json() if response.content else {}
            raise Exception(f"API Error {response.status_code}: {error_data}")
        return response.json() if response.content else None
    
    # Account operations
    def get_account(self, account_id: str) -> Account:
        """Retrieve a specific account"""
        response = self.session.get(self._build_url(f"/accounts/{account_id}"))
        data = self._handle_response(response)
        return Account(**data)
    
    def create_account(self, account: Account) -> Account:
        """Create a new account"""
        response = self.session.post(
            self._build_url("/accounts"),
            json=account.__dict__
        )
        data = self._handle_response(response)
        return Account(**data)
    
    # Line operations
    def reserve_line(self, account_id: str, order_id: str, line_id: str) -> Line:
        """Reserve a line"""
        response = self.session.put(
            self._build_url(f"/accounts/{account_id}/orders/{order_id}/lines/{line_id}?reserve")
        )
        data = self._handle_response(response)
        return Line(**data)
    
    def book_line(self, account_id: str, order_id: str, line_id: str) -> Line:
        """Book a line"""
        response = self.session.put(
            self._build_url(f"/accounts/{account_id}/orders/{order_id}/lines/{line_id}?book")
        )
        data = self._handle_response(response)
        return Line(**data)
    
    # Product operations
    def check_availability(self, request: ProductAvails) -> ProductAvails:
        """Check product availability"""
        response = self.session.post(
            self._build_url("/products/avails"),
            json=request.__dict__
        )
        data = self._handle_response(response)
        return ProductAvails(**data)
```

### 2. Complete Campaign Creation Flow

```python
from datetime import datetime

def create_campaign():
    """Complete campaign creation workflow"""
    client = OpenDirectClient(
        base_url='https://api.publisher.com',
        version='v1',
        access_token='your-oauth-token'
    )
    
    try:
        # Step 1: Create account
        account = client.create_account(Account(
            advertiser_id='adv-12345',
            buyer_id='buyer-67890',
            name='Q4 2024 Campaign Account'
        ))
        print(f"✓ Created account: {account.id}")
        
        # Step 2: Create order
        order = client.create_order(account.id, Order(
            account_id=account.id,
            brand='Example Brand',
            name='Q4 Display Campaign',
            budget=50000.00,
            currency='USD',
            start_date=datetime(2024, 10, 1),
            end_date=datetime(2024, 10, 31)
        ))
        print(f"✓ Created order: {order.id}")
        
        # Step 3: Create line with targeting
        line = client.create_line(account.id, order.id, Line(
            order_id=order.id,
            product_id='prod-12345',
            name='Homepage Banner - Desktop',
            quantity=1000000,
            rate=5.00,
            rate_type=RateType.CPM,
            start_date=datetime(2024, 10, 1),
            end_date=datetime(2024, 10, 31),
            frequency_count=3,
            frequency_interval=FrequencyInterval.DAY,
            targeting=[
                Segment(
                    target='Age',
                    target_values=['25-34', '35-44']
                ),
                Segment(
                    target='Geography',
                    target_values=['US-CA', 'US-NY']
                )
            ]
        ))
        print(f"✓ Created line: {line.id}")
        
        # Step 4: Upload creative
        creative = client.create_creative(account.id, Creative(
            id='',  # Will be assigned by API
            account_id=account.id,
            name='Q4 Banner Creative',
            ad_format_type='Banner',
            click_url='https://example.com/landing',
            geometry=[Size(height=250, width=300)],
            language='EN',
            maturity=MaturityLevel.GENERAL,
            https_compatible=True
        ))
        print(f"✓ Created creative: {creative.id}")
        
        # Step 5: Assign creative to line
        assignment = client.create_assignment(account.id, Assignment(
            line_id=line.id,
            creative_id=creative.id,
            status=AssignmentStatus.ACTIVE,
            weight=100
        ))
        print(f"✓ Created assignment: {assignment.id}")
        
        # Step 6: Reserve the line
        reserved_line = client.reserve_line(account.id, order.id, line.id)
        print(f"✓ Reserved line - Status: {reserved_line.booking_status.value}")
        
        # Step 7: Book the line
        booked_line = client.book_line(account.id, order.id, line.id)
        print(f"✓ Booked line - Status: {booked_line.booking_status.value}")
        
        return {
            'account': account,
            'order': order,
            'line': booked_line,
            'creative': creative,
            'assignment': assignment
        }
        
    except Exception as e:
        print(f"Campaign creation failed: {e}")
        raise
```

## Common Patterns

### 1. Polling for Status Changes

```typescript
async function waitForBookingStatus(
  client: OpenDirectClient,
  accountId: string,
  orderId: string,
  lineId: string,
  targetStatus: BookingStatus,
  maxAttempts: number = 30,
  intervalMs: number = 2000
): Promise<Line> {
  for (let attempt = 0; attempt < maxAttempts; attempt++) {
    const line = await client.getLine(accountId, orderId, lineId);
    
    if (line.BookingStatus === targetStatus) {
      return line;
    }
    
    if (line.BookingStatus === BookingStatus.Declined) {
      throw new Error(`Line declined: ${line.StateChangeReason}`);
    }
    
    await new Promise(resolve => setTimeout(resolve, intervalMs));
  }
  
  throw new Error(`Timeout waiting for status ${targetStatus}`);
}
```

### 2. Batch Operations

```typescript
async function createMultipleLines(
  client: OpenDirectClient,
  accountId: string,
  orderId: string,
  lineConfigs: Partial<Line>[]
): Promise<Line[]> {
  const lines: Line[] = [];
  
  for (const config of lineConfigs) {
    try {
      const line = await client.createLine(accountId, orderId, {
        OrderId: orderId,
        ...config
      } as Line);
      lines.push(line);
      console.log(`✓ Created line: ${line.Name}`);
    } catch (error) {
      console.error(`✗ Failed to create line: ${config.Name}`, error);
      // Continue with other lines or throw based on requirements
    }
  }
  
  return lines;
}
```

### 3. Reference Data Caching

```typescript
class CachedOpenDirectClient extends OpenDirectClient {
  private cache: Map<string, { data: any; timestamp: number }> = new Map();
  private cacheTTL: number = 3600000; // 1 hour

  async getCachedReferenceData<T>(
    key: string,
    fetcher: () => Promise<T>
  ): Promise<T> {
    const cached = this.cache.get(key);
    
    if (cached && Date.now() - cached.timestamp < this.cacheTTL) {
      return cached.data as T;
    }
    
    const data = await fetcher();
    this.cache.set(key, { data, timestamp: Date.now() });
    return data;
  }

  async getProducts(): Promise<CollectionResponse<Product>> {
    return this.getCachedReferenceData('products', () => super.listProducts());
  }
}
```

## Error Handling

### Comprehensive Error Handler

```typescript
interface OpenDirectError {
  statusCode: number;
  code: string;
  message: string;
  details?: any;
}

class OpenDirectErrorHandler {
  static handle(error: any): never {
    if (error.response) {
      const { status, data } = error.response;
      
      switch (status) {
        case 400:
          throw new Error(`Validation Error: ${data.message || 'Invalid request'}`);
        case 401:
          throw new Error('Authentication failed. Please check your access token.');
        case 403:
          throw new Error('Permission denied. Check account permissions.');
        case 404:
          throw new Error(`Resource not found: ${data.message || ''}`);
        case 500:
          throw new Error('Publisher API error. Please try again later.');
        default:
          throw new Error(`API Error ${status}: ${data.message || 'Unknown error'}`);
      }
    }
    
    throw error;
  }

  static async retry<T>(
    operation: () => Promise<T>,
    maxRetries: number = 3,
    delayMs: number = 1000
  ): Promise<T> {
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
      try {
        return await operation();
      } catch (error) {
        if (attempt === maxRetries) {
          throw error;
        }
        
        console.log(`Retry ${attempt}/${maxRetries} after ${delayMs}ms`);
        await new Promise(resolve => setTimeout(resolve, delayMs * attempt));
      }
    }
    
    throw new Error('Max retries exceeded');
  }
}
```

## Testing Strategies

### Unit Test Example (Jest)

```typescript
import { OpenDirectClient } from './client';
import { Account, BookingStatus } from './opendirect_mcp_types';

describe('OpenDirectClient', () => {
  let client: OpenDirectClient;

  beforeEach(() => {
    client = new OpenDirectClient({
      baseUrl: 'https://sandbox.publisher.com',
      version: 'v1',
      accessToken: 'test-token'
    });
  });

  it('should create account successfully', async () => {
    const account: Account = {
      AdvertiserId: 'test-adv',
      BuyerId: 'test-buyer',
      Name: 'Test Account'
    };

    const created = await client.createAccount(account);
    
    expect(created.Id).toBeDefined();
    expect(created.AdvertiserId).toBe(account.AdvertiserId);
    expect(created.BuyerId).toBe(account.BuyerId);
  });

  it('should handle line booking workflow', async () => {
    const accountId = 'acc-123';
    const orderId = 'ord-456';
    const lineId = 'line-789';

    // Reserve
    const reserved = await client.reserveLine(accountId, orderId, lineId);
    expect(reserved.BookingStatus).toBe(BookingStatus.Reserved);

    // Book
    const booked = await client.bookLine(accountId, orderId, lineId);
    expect(booked.BookingStatus).toBe(BookingStatus.Booked);
  });
});
```

### Integration Test Example

```typescript
async function integrationTest() {
  const client = new OpenDirectClient({
    baseUrl: process.env.OPENDIRECT_BASE_URL,
    version: 'v1',
    accessToken: process.env.OPENDIRECT_ACCESS_TOKEN
  });

  console.log('Running OpenDirect Integration Test...');

  // Test 1: Account creation
  const account = await client.createAccount({
    AdvertiserId: 'test-adv-' + Date.now(),
    BuyerId: 'test-buyer-' + Date.now(),
    Name: 'Integration Test Account'
  });
  console.log('✓ Account created:', account.Id);

  // Test 2: Product search
  const products = await client.searchProducts({
    AdFormatTypes: ['Banner']
  });
  console.log('✓ Products found:', products.items.length);

  // Test 3: Order creation
  const order = await client.createOrder(account.Id, {
    AccountId: account.Id,
    Brand: 'Test Brand',
    Name: 'Test Order',
    Currency: 'USD'
  });
  console.log('✓ Order created:', order.Id);

  // Cleanup
  await client.deleteOrder(account.Id, order.Id);
  console.log('✓ Cleanup completed');
}
```

## Production Considerations

### 1. Authentication Management

```typescript
class OAuthTokenManager {
  private token: string | null = null;
  private expiresAt: number = 0;

  async getToken(): Promise<string> {
    if (this.token && Date.now() < this.expiresAt) {
      return this.token;
    }

    // Refresh token logic
    const response = await this.refreshToken();
    this.token = response.accessToken;
    this.expiresAt = Date.now() + (response.expiresIn * 1000);
    
    return this.token;
  }

  private async refreshToken(): Promise<{ accessToken: string; expiresIn: number }> {
    // Implementation specific to OAuth provider
    throw new Error('Not implemented');
  }
}
```

### 2. Rate Limiting

```typescript
class RateLimiter {
  private requests: number[] = [];
  private maxRequests: number;
  private windowMs: number;

  constructor(maxRequests: number, windowMs: number) {
    this.maxRequests = maxRequests;
    this.windowMs = windowMs;
  }

  async throttle(): Promise<void> {
    const now = Date.now();
    this.requests = this.requests.filter(time => now - time < this.windowMs);

    if (this.requests.length >= this.maxRequests) {
      const oldestRequest = this.requests[0];
      const waitTime = this.windowMs - (now - oldestRequest);
      await new Promise(resolve => setTimeout(resolve, waitTime));
    }

    this.requests.push(Date.now());
  }
}
```

### 3. Logging and Monitoring

```typescript
class OpenDirectLogger {
  static logRequest(method: string, url: string, body?: any) {
    console.log({
      timestamp: new Date().toISOString(),
      type: 'REQUEST',
      method,
      url,
      body
    });
  }

  static logResponse(method: string, url: string, status: number, duration: number) {
    console.log({
      timestamp: new Date().toISOString(),
      type: 'RESPONSE',
      method,
      url,
      status,
      duration
    });
  }

  static logError(error: any) {
    console.error({
      timestamp: new Date().toISOString(),
      type: 'ERROR',
      error: error.message,
      stack: error.stack
    });
  }
}
```

### 4. Configuration Management

```typescript
interface OpenDirectConfig {
  baseUrl: string;
  version: string;
  accessToken: string;
  timeout?: number;
  retryAttempts?: number;
  retryDelay?: number;
}

class ConfigManager {
  static loadConfig(): OpenDirectConfig {
    return {
      baseUrl: process.env.OPENDIRECT_BASE_URL || '',
      version: process.env.OPENDIRECT_VERSION || 'v1',
      accessToken: process.env.OPENDIRECT_ACCESS_TOKEN || '',
      timeout: parseInt(process.env.OPENDIRECT_TIMEOUT || '30000'),
      retryAttempts: parseInt(process.env.OPENDIRECT_RETRY_ATTEMPTS || '3'),
      retryDelay: parseInt(process.env.OPENDIRECT_RETRY_DELAY || '1000')
    };
  }

  static validate(config: OpenDirectConfig): void {
    if (!config.baseUrl) {
      throw new Error('OPENDIRECT_BASE_URL is required');
    }
    if (!config.accessToken) {
      throw new Error('OPENDIRECT_ACCESS_TOKEN is required');
    }
  }
}
```

## Summary

This implementation guide provides:
- Complete client implementations in TypeScript and Python
- Real-world workflow examples
- Common patterns for production use
- Comprehensive error handling
- Testing strategies
- Production considerations

For more details, refer to:
- `opendirect_mcp_schema.json` - Complete schema definition
- `opendirect_mcp_documentation.md` - Detailed documentation
- `opendirect_mcp_types.ts` - TypeScript type definitions
- `opendirect_mcp_types.py` - Python dataclass definitions
