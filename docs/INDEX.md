# OpenDirect v2.1 MCP Implementation - Documentation Index

## Quick Start

**Start here:** [README.md](./README.md) (4.1KB)
- Installation instructions
- Quick overview
- Core features
- Basic examples

## Core Implementation

### MCP Server Specification
**File:** [opendirect-mcp-server.json](./opendirect-mcp-server.json) (40KB)

**Contents:**
- Complete schema definitions (30+ objects)
- All OpenDirect v2.1 resources
- AdCOM specifications
- OpenRTB integration
- MCP tools (10)
- MCP prompts (3)
- Full metadata

**Use for:**
- Schema reference
- Type definitions
- Validation rules
- API specification

## Documentation Files

### 1. Project Overview
**File:** [PROJECT_OVERVIEW.md](./PROJECT_OVERVIEW.md) (17KB)

**Contents:**
- Comprehensive project summary
- Complete object model
- Standard workflow overview
- All 13 booking states
- Rate types and delivery types
- Standards compliance
- Use cases
- Best practices
- Resources and links

**Use for:**
- Understanding the complete system
- Architecture overview
- Business context
- Standards reference

### 2. Implementation Examples
**File:** [IMPLEMENTATION_EXAMPLES.md](./IMPLEMENTATION_EXAMPLES.md) (17KB)

**Contents:**
- Complete end-to-end workflow (12 steps)
- Object creation examples
- Video, audio, native creative examples
- AdCOM integration patterns
- Query and search examples
- Change request workflows
- Message communication
- Error handling patterns
- Best practices

**Use for:**
- Code examples
- Integration patterns
- Real-world scenarios
- API usage reference

### 3. Object Diagrams
**File:** [OBJECT_DIAGRAMS.md](./OBJECT_DIAGRAMS.md) (13KB)

**Contents:**
- Core object relationships
- Workflow state diagram
- Organization hierarchy
- Complete booking sequence
- Creative assignment flow
- AdCOM architecture
- PMP structure
- Change request workflow
- Message threads
- Rate application
- Status lifecycles

**Use for:**
- Visual understanding
- Relationship mapping
- Workflow comprehension
- Architecture diagrams

## Additional Resources

### Implementation Guide
**File:** [IMPLEMENTATION_GUIDE.md](./IMPLEMENTATION_GUIDE.md) (22KB)
- Detailed implementation instructions
- Step-by-step guides
- Integration patterns

### Project Summary
**File:** [PROJECT_SUMMARY.md](./PROJECT_SUMMARY.md) (7.2KB)
- Executive summary
- Key features
- Quick reference

## Document Purpose Matrix

| Document | Purpose | Audience | Priority |
|----------|---------|----------|----------|
| README.md | Quick start & overview | Everyone | 1st |
| opendirect-mcp-server.json | Schema reference | Developers | 1st |
| PROJECT_OVERVIEW.md | Complete understanding | Everyone | 2nd |
| IMPLEMENTATION_EXAMPLES.md | Code patterns | Developers | 2nd |
| OBJECT_DIAGRAMS.md | Visual reference | Architects/Developers | 3rd |
| IMPLEMENTATION_GUIDE.md | Detailed how-to | Developers | 3rd |
| PROJECT_SUMMARY.md | Executive summary | Business users | Optional |

## Learning Path

### For Business Users
1. Start with PROJECT_OVERVIEW.md
   - Understand what OpenDirect is
   - Learn the benefits
   - Review use cases
2. Review OBJECT_DIAGRAMS.md
   - Visualize workflows
   - Understand relationships
3. Skim IMPLEMENTATION_EXAMPLES.md
   - See real scenarios
   - Understand capabilities

### For Developers
1. Start with README.md
   - Quick orientation
   - Installation
   - Basic examples
2. Study opendirect-mcp-server.json
   - Schema structure
   - Type definitions
   - Validation rules
3. Deep dive into IMPLEMENTATION_EXAMPLES.md
   - Complete workflow
   - Code patterns
   - Error handling
4. Reference OBJECT_DIAGRAMS.md
   - Visualize relationships
   - Understand state flows
5. Use IMPLEMENTATION_GUIDE.md
   - Step-by-step instructions
   - Integration patterns

### For Architects
1. Review PROJECT_OVERVIEW.md
   - Complete system understanding
   - Standards integration
   - Architecture layers
2. Study OBJECT_DIAGRAMS.md
   - System architecture
   - Object relationships
   - Data flows
3. Review opendirect-mcp-server.json
   - Schema design
   - Type system
   - Extensions
4. Reference IMPLEMENTATION_EXAMPLES.md
   - Integration patterns
   - Best practices

## Key Concepts by Document

### README.md
- Quick installation
- Core resources (10)
- Referenced specs (3)
- Basic usage

### PROJECT_OVERVIEW.md
- OpenDirect introduction
- Object model (35+ objects)
- Complete workflow
- 13 booking states
- 5 rate types
- 7 delivery types
- Standards (ISO, IAB)
- 10 MCP tools
- Use cases

### IMPLEMENTATION_EXAMPLES.md
- 12-step complete workflow
- Video creative examples
- Native ad examples
- Audio with companions
- Multi-format packages
- Targeted campaigns
- Change requests
- Message threads
- Error responses
- 7 best practices

### OBJECT_DIAGRAMS.md
- 10 Mermaid diagrams
- Object relationships
- State machines
- Sequence diagrams
- Architecture views
- Data flows

### opendirect-mcp-server.json
- 10 core resources
- 25+ AdCOM objects
- 4 OpenRTB objects
- 10 MCP tools
- Full type system
- Validation rules

## Object Reference Guide

### Core OpenDirect Objects
Find in: opendirect-mcp-server.json → schemas

- OpenDirect.Organization
- OpenDirect.Account
- OpenDirect.Order
- OpenDirect.Line
- OpenDirect.Product
- OpenDirect.Creative
- OpenDirect.Assignment
- OpenDirect.Placement
- OpenDirect.ChangeRequest
- OpenDirect.Message

### AdCOM Specifications
Find in: opendirect-mcp-server.json → schemas

- AdCOM.AdSpec
- AdCOM.CreativeSpec
- AdCOM.VideoSpec
- AdCOM.DisplaySpec
- AdCOM.AudioSpec
- AdCOM.BannerSpec
- AdCOM.NativeSpec
- AdCOM.AMPSpec

### AdCOM Responses
Find in: opendirect-mcp-server.json → schemas

- AdCOM.AdResp
- AdCOM.CreativeResp
- AdCOM.VideoResp
- AdCOM.DisplayResp
- AdCOM.AudioResp

### AdCOM Context
Find in: opendirect-mcp-server.json → schemas

- AdCOM.Site
- AdCOM.App
- AdCOM.Device
- AdCOM.Geo
- AdCOM.User
- AdCOM.Content
- AdCOM.Publisher
- AdCOM.Producer
- AdCOM.Data
- AdCOM.Segment

### OpenRTB Objects
Find in: opendirect-mcp-server.json → schemas

- OpenRTB.Deal
- OpenRTB.PMP
- OpenRTB.Regs
- OpenRTB.Source

## Workflow Reference

### Complete Campaign Booking
See: IMPLEMENTATION_EXAMPLES.md → Complete Workflow Example
- 12 steps from organization to launch
- All API calls with full payloads

### State Transitions
See: OBJECT_DIAGRAMS.md → Workflow State Diagram
- Visual state machine
- All 13 booking statuses
- Transition conditions

### Change Management
See: IMPLEMENTATION_EXAMPLES.md → Change Request Examples
See: OBJECT_DIAGRAMS.md → Change Request Workflow
- Request creation
- Approval process
- Webhook notifications

### Communication
See: IMPLEMENTATION_EXAMPLES.md → Message Examples
See: OBJECT_DIAGRAMS.md → Message Thread Flow
- Send messages
- Reply threads
- Status tracking

## Code Examples

### Create Organization
See: IMPLEMENTATION_EXAMPLES.md → Step 1

### Create Account
See: IMPLEMENTATION_EXAMPLES.md → Step 3

### Search Products
See: IMPLEMENTATION_EXAMPLES.md → Step 4

### Create Order
See: IMPLEMENTATION_EXAMPLES.md → Step 5

### Create Line
See: IMPLEMENTATION_EXAMPLES.md → Step 6

### Create Creative
See: IMPLEMENTATION_EXAMPLES.md → Step 7
See: IMPLEMENTATION_EXAMPLES.md → Object Creation Examples (video, native, audio)

### Create Assignment
See: IMPLEMENTATION_EXAMPLES.md → Step 8

### Update Booking Status
See: IMPLEMENTATION_EXAMPLES.md → Steps 9-12

### Handle Errors
See: IMPLEMENTATION_EXAMPLES.md → Error Handling

## Standards Reference

### ISO Standards
See: PROJECT_OVERVIEW.md → Standards Compliance
- ISO-639-1: Languages
- ISO-3166: Countries/Regions
- ISO-4217: Currencies

### IAB Standards
See: PROJECT_OVERVIEW.md → Standards Compliance
- Content Taxonomy
- IQG 2.1 Media Ratings
- OpenRTB Protocol
- VAST/DAAST
- Native Ad Spec
- AMP Ads

## Tool Reference

### MCP Tools
See: opendirect-mcp-server.json → tools
See: PROJECT_OVERVIEW.md → MCP Tools

1. create_organization
2. create_account
3. create_order
4. create_line
5. create_creative
6. create_assignment
7. search_products
8. create_change_request
9. send_message
10. update_line_booking_status

## FAQ Quick Reference

**Q: What is OpenDirect?**
A: See PROJECT_OVERVIEW.md → Overview

**Q: How do I book a campaign?**
A: See IMPLEMENTATION_EXAMPLES.md → Complete Workflow Example

**Q: What are the booking states?**
A: See PROJECT_OVERVIEW.md → Line Booking States
See OBJECT_DIAGRAMS.md → Workflow State Diagram

**Q: How do rate types work?**
A: See PROJECT_OVERVIEW.md → Rate Types
See OBJECT_DIAGRAMS.md → Rate Type Application

**Q: How do I create a video creative?**
A: See IMPLEMENTATION_EXAMPLES.md → Video Creative with VAST

**Q: What's the difference between Account and Order?**
A: See PROJECT_OVERVIEW.md → Object Model
See OBJECT_DIAGRAMS.md → Account and Organization Hierarchy

**Q: How do change requests work?**
A: See IMPLEMENTATION_EXAMPLES.md → Change Request Examples
See OBJECT_DIAGRAMS.md → Change Request Workflow

**Q: What standards does OpenDirect use?**
A: See PROJECT_OVERVIEW.md → Standards Compliance

**Q: How do I handle errors?**
A: See IMPLEMENTATION_EXAMPLES.md → Error Handling

**Q: What is AdCOM?**
A: See PROJECT_OVERVIEW.md → Referenced Specifications
See OBJECT_DIAGRAMS.md → AdCOM Integration

## Version Information

- **OpenDirect Version**: v2.1 Final
- **Specification Date**: 2018
- **IAB Tech Lab**: https://iabtechlab.com
- **License**: Creative Commons Attribution 3.0

## Getting Help

### Technical Questions
- Review IMPLEMENTATION_EXAMPLES.md
- Check IMPLEMENTATION_GUIDE.md
- Reference opendirect-mcp-server.json

### Business Questions
- Review PROJECT_OVERVIEW.md
- Check use cases section
- Review best practices

### Conceptual Questions
- Study OBJECT_DIAGRAMS.md
- Review PROJECT_OVERVIEW.md
- Check workflow documentation

### Official Support
- **Email**: openmedia@iabtechlab.com
- **Website**: https://iabtechlab.com/opendirect
- **GitHub**: https://github.com/InteractiveAdvertisingBureau/OpenDirect

## File Size Reference

| File | Size | Lines | Purpose |
|------|------|-------|---------|
| README.md | 4.1KB | 146 | Quick start |
| opendirect-mcp-server.json | 40KB | 1733 | MCP schema |
| PROJECT_OVERVIEW.md | 17KB | ~700 | Complete guide |
| IMPLEMENTATION_EXAMPLES.md | 17KB | ~700 | Code examples |
| OBJECT_DIAGRAMS.md | 13KB | ~550 | Visual docs |
| IMPLEMENTATION_GUIDE.md | 22KB | ~900 | How-to guide |
| PROJECT_SUMMARY.md | 7.2KB | ~300 | Executive summary |

## Last Updated

November 21, 2025

---

**Ready to get started?** Begin with [README.md](./README.md)!
