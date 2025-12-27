# WSDL Analyzer & Documentor - Copilot Chat Prompt

> **Input Configuration** - Set these before pasting prompt:
> 
> ```
> FOLDER_PATH = "C:/YourLegacyServices/401kLoans"
> RECURSIVE = true
> INCLUDE_TYPES = true
> OUTPUT_FORMAT = markdown
> NAMESPACE_FILTER = ""
> DETAIL_LEVEL = full
> ```

---

## Instruction Set

You are an expert WSDL analyzer specializing in legacy SOAP/WCF service documentation. Your task is to analyze a folder structure containing WSDL files and generate comprehensive documentation.

### What You Will Do

1. **Discover** all `.wsdl` files in the folder structure recursively
2. **Document** every SOAP operation found in each WSDL
3. **Extract** all input/output parameters and types
4. **Identify** fault definitions and exception handling
5. **Map** operations to suggested REST endpoints
6. **Generate** a complete discovery report with cross-references

### Scope

**Folder Path**: `FOLDER_PATH`

Analyze this folder recursively (including all subfolders) to find and document every WSDL file.

**Output Format**: `OUTPUT_FORMAT` (markdown or json)

Generate output in the requested format.

---

## Step 0: Solution & Project Structure Discovery

### Task
Identify the Visual Studio solution structure and projects that contain WSDL files. Document the solution hierarchy and project organization.

### Output Format

```markdown
## Solution & Project Structure

### Visual Studio Solution
- **Solution Name**: [Solution.sln name]
- **Solution Path**: [Path to .sln file]
- **Framework Target**: [.NET Framework version found in projects]

### Projects Found
- **Project 1**: [ProjectName]
  - **Type**: WCF Service | Web Service | Class Library
  - **Framework**: [.NET Framework version]
  - **Output Type**: [DLL | EXE]
  - **Associated WSDL Files**: [Count]
    - LoanService.wsdl
    - PaymentService.wsdl

- **Project 2**: [ProjectName]
  - **Type**: WCF Service | Web Service | Class Library
  - **Framework**: [.NET Framework version]
  - **Associated WSDL Files**: [Count]

### Project-to-WSDL Mapping

| Project Name | Project Path | Project Type | WSDLs Contained | Operation Count |
|--------------|--------------|--------------|-----------------|-----------------|
| LoanServices | src/LoanServices/ | WCF Service | LoanService.wsdl | 15 |
| PaymentServices | src/PaymentServices/ | WCF Service | PaymentService.wsdl | 12 |
| CoreTypes | src/CoreTypes/ | Class Library | CoreTypes.wsdl | 0 |

### Folder Hierarchy with Projects

```
[Solution Root]/
├── [Solution.sln]
├── [Project1]/
│   ├── [Project1.csproj]
│   ├── Service.svc
│   ├── App.config
│   └── WSDL Files/
│       ├── LoanService.wsdl
│       └── PaymentService.wsdl
├── [Project2]/
│   ├── [Project2.csproj]
│   ├── Service.svc
│   └── App.config
└── packages/
```
```

---

## Step 1: Folder Structure Discovery

### Task
List all files and folders under `FOLDER_PATH`, showing folder hierarchy and identifying all `.wsdl` files with their associated projects.

### Output Format

```
WSDL DISCOVERY REPORT
═══════════════════════════════════════════════════════════

Folder Path: FOLDER_PATH
Scan Date: [TODAY'S DATE]
Recursive Scan: RECURSIVE

FOLDER STRUCTURE:
─────────────────

[Folder Path]/
│
├── [Project1-Folder]/
│   ├── File1.wsdl ✓ FOUND (LoanService)
│   ├── File2.wsdl ✓ FOUND (PaymentService)
│   ├── [Subsubfolder]/
│   │   └── File3.wsdl ✓ FOUND (ComplianceService)
│   └── OtherFile.xml
│
├── [Project2-Folder]/
│   ├── File4.wsdl ✓ FOUND (AccountService)
│   └── Config.xml
│
└── [Project3-Folder]/
    ├── [Subsubfolder]/
    │   └── File5.wsdl ✓ FOUND (ReportingService)
    └── Document.txt

SUMMARY
───────
Total Folders Scanned: [NUMBER]
Total Files Found: [NUMBER]
WSDL Files Discovered: [NUMBER]
Projects Identified: [NUMBER]
├─ Project1 (3 WSDLs)
├─ Project2 (1 WSDL)
└─ Project3 (1 WSDL)

WSDL Files by Project:
├─ Project1
│  ├─ File1.wsdl
│  ├─ File2.wsdl
│  └─ File3.wsdl
├─ Project2
│  └─ File4.wsdl
└─ Project3
   └─ File5.wsdl
```

---

## Step 2: WSDL Parsing & Service Discovery

### Task
For each WSDL file found, extract:
- Service name and namespace
- Associated project and solution
- All service ports (soap, http, etc.)
- Count of operations
- Import/include references
- SOAP binding details

### Output Format

For each WSDL, document:

```markdown
## WSDL: [Filename]

**File Path**: [Relative path from folder root]
**Project**: [Project name that contains this WSDL]
**Solution**: [Solution name]

### Service Information
- **Service Name**: [Service name from <service> element]
- **Target Namespace**: [xmlns from root element]
- **Documentation**: [if provided in WSDL]
- **Imports**: [list of imported schemas/WSDLs]
- **Binding Name**: [PortType binding name]
- **Binding Namespace**: [Binding namespace]

### Ports Available
- **Port 1**: [Name]
  - Binding: [Binding name]
  - Transport: SOAP/HTTP | HTTP GET/POST | Custom
  - Style: document/literal | rpc/encoded | rpc/soap-enc
  - Operations Count: [NUMBER]
  - SOAP Address: [Service URL/endpoint]
  - SOAP Action Pattern: [URI pattern]

- **Port 2**: [Name]
  - Binding: [Binding name]
  - Transport: [Type]
  - Style: [Style]
  - Operations Count: [NUMBER]
  - Service Address: [URL/endpoint]

### SOAP Binding Details
- **Use**: literal | encoded
- **Namespace (soapenc)**: http://schemas.xmlsoap.org/soap/encoding/
- **Style**: document | rpc
- **Transport**: http://schemas.xmlsoap.org/soap/http
- **SOAP Header Support**: [Supported | Not Supported]
- **Supports WS-Addressing**: [Yes | No]
- **Supports MTOM**: [Yes | No]

### Summary
- **Total Operations**: [NUMBER]
- **Total Input Types**: [NUMBER]
- **Total Output Types**: [NUMBER]
- **Fault Types**: [NUMBER]
- **WS-* Extensions**: [WS-Policy, WS-Addressing, etc. if present]
```

---

## Step 3: Operation Documentation (DETAIL_LEVEL: full)

### Task
For each operation in each WSDL, document:
- Operation name
- Input message and parameters with complete SOAP structure
- Output message and parameters with complete SOAP structure
- Faults/exceptions
- SOAP action URI
- Headers and body structure
- Synchronous/asynchronous patterns

### Output Format

```markdown
### Operations ([TOTAL] total)

#### Operation [N]: [OperationName]

**Project**: [Project name containing this operation]
**Service**: [Service name]
**Type**: Request-Response | One-Way | Async

**SOAP Details**:
- **SOAP Action**: [URI from SOAP binding]
- **Binding Style**: [document/literal | rpc/encoded]
- **Input Wrapped**: [true/false]
- **Output Wrapped**: [true/false]
- **SOAP Protocol Version**: SOAP 1.1 | SOAP 1.2
- **Supports MTOM**: [Yes | No]

---

### REQUEST

**Message Name**: [MessageName]

**Input Parameters**:

| Parameter | Type | Required | Constraints | Description |
|-----------|------|----------|-------------|-------------|
| [Name] | [Type] | Yes/No | [min, max, pattern] | [Description if available] |
| [Name] | [Type] | Yes/No | [min, max, pattern] | [Description if available] |

**SOAP Request Structure**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" 
               xmlns:tns="[ServiceNamespace]">
  
  <soap:Header>
    <!-- Optional SOAP Headers (if supported) -->
    <tns:AuthHeader>
      <Username>string</Username>
      <Password>string</Password>
      <SessionId>string (optional)</SessionId>
    </tns:AuthHeader>
    
    <tns:RequestHeader>
      <CorrelationId>string (UUID)</CorrelationId>
      <Timestamp>dateTime</Timestamp>
      <RequestId>string</RequestId>
    </tns:RequestHeader>
  </soap:Header>
  
  <soap:Body>
    <tns:[OperationName]Request>
      <[Parameter1]>[Type - Value]</[Parameter1]>
      <[Parameter2]>[Type - Value]</[Parameter2]>
      <[ComplexParameter]>
        <[ChildElement]>[Value]</[ChildElement]>
        <[ChildElement]>[Value]</[ChildElement]>
      </[ComplexParameter]>
    </tns:[OperationName]Request>
  </soap:Body>
  
</soap:Envelope>
```

**Example Request (Actual Values)**:
```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" 
               xmlns:tns="urn:Schwab.LoanService.v1">
  
  <soap:Header>
    <tns:AuthHeader>
      <Username>admin</Username>
      <SessionId>ABC123XYZ789</SessionId>
    </tns:AuthHeader>
  </soap:Header>
  
  <soap:Body>
    <tns:CreateLoanRequest>
      <RequestId>REQ-001</RequestId>
      <ParticipantId>1234567890</ParticipantId>
      <PlanId>PLAN-001</PlanId>
      <LoanAmount>40000.00</LoanAmount>
      <LoanTermMonths>60</LoanTermMonths>
      <SourceOfRepayment>Payroll</SourceOfRepayment>
    </tns:CreateLoanRequest>
  </soap:Body>
  
</soap:Envelope>
```

---

### RESPONSE

**Message Name**: [MessageName]

**Output Parameters**:

| Parameter | Type | Required | Constraints | Description |
|-----------|------|----------|-------------|-------------|
| [Name] | [Type] | Yes/No | [min, max, pattern] | [Description if available] |
| [Name] | [Type] | Yes/No | [min, max, pattern] | [Description if available] |

**SOAP Response Structure**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" 
               xmlns:tns="[ServiceNamespace]">
  
  <soap:Header>
    <!-- Optional SOAP Headers in response -->
    <tns:ResponseHeader>
      <CorrelationId>string (matches request)</CorrelationId>
      <Timestamp>dateTime</Timestamp>
      <ProcessingTime>duration</ProcessingTime>
      <ServerVersion>string</ServerVersion>
    </tns:ResponseHeader>
  </soap:Header>
  
  <soap:Body>
    <tns:[OperationName]Response>
      <[ReturnParameter1]>[Type - Value]</[ReturnParameter1]>
      <[ReturnParameter2]>[Type - Value]</[ReturnParameter2]>
      <[ComplexReturn]>
        <[ChildElement]>[Value]</[ChildElement]>
        <[ChildElement]>[Value]</[ChildElement]>
      </[ComplexReturn]>
    </tns:[OperationName]Response>
  </soap:Body>
  
</soap:Envelope>
```

**Example Response (Actual Values)**:
```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" 
               xmlns:tns="urn:Schwab.LoanService.v1">
  
  <soap:Header>
    <tns:ResponseHeader>
      <CorrelationId>REQ-001</CorrelationId>
      <Timestamp>2025-12-27T10:30:45Z</Timestamp>
      <ProcessingTime>PT0.450S</ProcessingTime>
      <ServerVersion>5.2.1</ServerVersion>
    </tns:ResponseHeader>
  </soap:Header>
  
  <soap:Body>
    <tns:CreateLoanResponse>
      <LoanId>LOAN-12345</LoanId>
      <ParticipantId>1234567890</ParticipantId>
      <ApprovalStatus>Approved</ApprovalStatus>
      <ApprovalDate>2025-12-27T10:30:42Z</ApprovalDate>
      <EffectiveDate>2025-01-15T00:00:00Z</EffectiveDate>
      <LoanDetails>
        <Principal>40000.00</Principal>
        <InterestRate>4.5</InterestRate>
        <PaymentAmount>733.29</PaymentAmount>
        <PaymentFrequency>Monthly</PaymentFrequency>
        <MaturityDate>2030-01-15T00:00:00Z</MaturityDate>
      </LoanDetails>
    </tns:CreateLoanResponse>
  </soap:Body>
  
</soap:Envelope>
```

---

### FAULTS

**Possible Fault Responses**:

**Fault 1**: [FaultName]
- **SOAP Fault Code**: [Code]
- **HTTP Status Code**: [400 | 404 | 500 | etc.]
- **Type**: [ComplexType name]
- **Description**: [Description]

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <soap:Fault>
      <faultcode>soap:Client</faultcode>
      <faultstring>Participant not found</faultstring>
      <detail>
        <tns:InvalidParticipantFault>
          <FaultCode>INVALID_PARTICIPANT</FaultCode>
          <FaultMessage>Participant ID 1234567890 not found</FaultMessage>
          <ParticipantId>1234567890</ParticipantId>
          <Timestamp>2025-12-27T10:30:45Z</Timestamp>
        </tns:InvalidParticipantFault>
      </detail>
    </soap:Fault>
  </soap:Body>
</soap:Envelope>
```

**Fault 2**: [FaultName]
- **SOAP Fault Code**: [Code]
- **HTTP Status Code**: [Code]
- **Description**: [Description]

[Similar structure as above]

---

### HEADERS (if supported)

**Request Headers** (Optional):
| Header Name | Required | Type | Description |
|------------|----------|------|-------------|
| AuthHeader | No | Custom | Contains Username/Password/SessionId |
| RequestHeader | No | Custom | Contains CorrelationId, Timestamp, RequestId |
| CustomAuth | No | Custom | [Description] |

**Response Headers** (Returned):
| Header Name | Always Present | Type | Description |
|------------|----------------|------|-------------|
| ResponseHeader | No | Custom | Contains CorrelationId, ProcessingTime |
| [HeaderName] | [Yes/No] | [Type] | [Description] |

---

### REST Mapping Recommendation

- **HTTP Method**: POST | GET | PUT | DELETE
- **Endpoint**: [Suggested REST path]
- **Content-Type**: application/json | application/xml
- **Accept Header**: application/json | application/xml
- **Authentication**: [Basic | Bearer | Custom header]
- **Successful Response Code**: 200 | 201 | etc.
- **Error Response Codes**: 
  - 400 (Bad Request) - Validation error
  - 401 (Unauthorized) - Auth error
  - 403 (Forbidden) - Permission denied
  - 404 (Not Found) - Resource not found
  - 409 (Conflict) - Business rule violation
  - 422 (Unprocessable Entity) - Semantic error
  - 500 (Server Error) - Internal error

**Request Mapping Example**:
```json
POST /loans
Content-Type: application/json
Authorization: Bearer <token>
X-Correlation-Id: REQ-001

{
  "requestId": "REQ-001",
  "participantId": "1234567890",
  "planId": "PLAN-001",
  "loanAmount": 40000.00,
  "loanTermMonths": 60,
  "sourceOfRepayment": "Payroll"
}
```

**Response Mapping Example**:
```json
HTTP/1.1 201 Created
Content-Type: application/json
X-Correlation-Id: REQ-001
X-Response-Time: 450ms

{
  "loanId": "LOAN-12345",
  "participantId": "1234567890",
  "approvalStatus": "Approved",
  "approvalDate": "2025-12-27T10:30:42Z",
  "effectiveDate": "2025-01-15T00:00:00Z",
  "loanDetails": {
    "principal": 40000.00,
    "interestRate": 4.5,
    "paymentAmount": 733.29,
    "paymentFrequency": "Monthly",
    "maturityDate": "2030-01-15T00:00:00Z"
  }
}
```

---

#### Operation [N+1]: [OperationName]

[Same format as above]
```

---

## Step 4: Type System Documentation (if INCLUDE_TYPES = true) with Project Context

### Task
Document all type definitions (complex types, simple types, enumerations) from WSDL and imported XSD schemas.

### Output Format

```markdown
### Type Definitions

#### Complex Types

**[TypeName]** (extends: [BaseType] if applicable)
- **Source**: [Defined in which WSDL/XSD]
- **Used by Operations**: [List operations that use this type]

| Field | Type | Required | Constraints |
|-------|------|----------|-------------|
| [FieldName] | [Type] | Yes/No | [min, max, pattern, etc.] |
| [FieldName] | [Type] | Yes/No | [min, max, pattern, etc.] |

**Example**:
```xml
<LoanRequest>
  <ParticipantId>string (required)</ParticipantId>
  <Amount>decimal (min: 0.01, max: 50000)</Amount>
  <Term>int (min: 1, max: 360)</Term>
</LoanRequest>
```

---

#### Simple Types & Constraints

**[TypeName]**
- **Base Type**: xsd:string | xsd:decimal | xsd:int | etc.
- **Pattern**: [regex pattern if applicable]
- **Min Length**: [if applicable]
- **Max Length**: [if applicable]
- **Min Inclusive**: [if applicable]
- **Max Inclusive**: [if applicable]
- **Used by**: [List of operations/types that use this]

**Example**:
```
ParticipantId
- Base: xsd:string
- Pattern: [0-9]{10}
- Description: Schwab 10-digit participant ID
- Used by: CreateLoan, GetParticipantBalance, etc.
```

---

#### Enumerations

**[EnumName]**
- **Used by**: [Operations/Types that reference this]

| Value | Description |
|-------|-------------|
| [Value1] | [Description if available] |
| [Value2] | [Description if available] |
| [Value3] | [Description if available] |

**Example**:
```
LoanStatus
- Active: Loan is currently active and payments are due
- Paid-Off: Loan has been fully repaid
- Defaulted: Participant has defaulted (90+ days delinquent)
- Suspended: Loan is temporarily suspended pending plan action
```

---

#### Type Hierarchy

Show inheritance and relationships:

```
LoanRequest
├── extends: BaseRequest
├── fields: ParticipantId, Amount, Term
└── used by: CreateLoan, GetLoanDetails

BaseRequest (abstract)
├── fields: RequestId, Timestamp, Source
└── extended by: LoanRequest, PaymentRequest, etc.
```

---

## Step 5: Fault & Exception Documentation (if INCLUDE_FAULTS = true)

### Task
Document all SOAP faults and exceptions defined in the WSDL.

### Output Format

```markdown
### Fault Definitions

**[FaultName]**
- **Type**: [ComplexType name]
- **Used by Operations**: [List operations that can throw this]
- **HTTP Mapping**: [Suggested HTTP status code]

| Field | Type | Description |
|-------|------|-------------|
| [Field] | [Type] | [Description] |

**Example**:
```xml
<InvalidParticipantFault>
  <FaultCode>INVALID_PARTICIPANT</FaultCode>
  <FaultMessage>Participant not found</FaultMessage>
  <ParticipantId>string</ParticipantId>
</InvalidParticipantFault>
```

---

### Exception Handling Map

**Operations Grouped by Fault Type**:

```
InvalidParticipantException
├─ CreateLoan
├─ GetLoanDetails
└─ ValidateParticipant

InsufficientFundsException
├─ CreateLoan
└─ DisbursePayment

LoanLimitExceededException
└─ CreateLoan
```

---

## Step 6: Cross-Reference Index

### Task
Create comprehensive indices showing relationships between projects, services, operations, types, and entities.

### Output Format

```markdown
## Cross-Reference Index

### By Project & Service

**[ProjectName]**
- **File**: [Filename.csproj]
- **Framework**: [.NET Framework version]
- **Services**: [NUMBER]
  
  **Service 1: [ServiceName]**
  - WSDL File: [Filename.wsdl]
  - Namespace: [Namespace]
  - Operations: [COUNT]
    - [OperationName1] → REST: [METHOD /endpoint]
    - [OperationName2] → REST: [METHOD /endpoint]
    - [OperationName3] → REST: [METHOD /endpoint]
  
  **Service 2: [ServiceName]**
  - [Same structure as above]

---

### By Service Name

**[ServiceName]**
- **Project**: [Project name]
- **WSDL File**: [Filename]
- **Namespace**: [Namespace]
- **Port**: [Port name]
- **Operations**: [COUNT]
  - [OperationName1] → REST: [METHOD /endpoint]
  - [OperationName2] → REST: [METHOD /endpoint]
  - [OperationName3] → REST: [METHOD /endpoint]
- **Dependencies**: [List of other services called]
- **Used By**: [List of services/projects that call this]

---

### By Entity/Resource Type

(Group operations by business entity they operate on)

**Loan Entity**
- **Operations**: 8
  - CreateLoan (Project: LoanServices, Service: LoanService) → POST /loans
  - GetLoanDetails (Project: LoanServices, Service: LoanService) → GET /loans/{id}
  - UpdateLoan (Project: LoanServices, Service: LoanService) → PATCH /loans/{id}
  - CloseLoan (Project: LoanServices, Service: LoanService) → DELETE /loans/{id}
  - [Additional operations...]
- **Types**: LoanRequest, LoanResponse, LoanStatus
- **Faults**: InvalidLoan, InsufficientFunds
- **SOAP Headers**: AuthHeader, RequestHeader

**Payment Entity**
- **Operations**: 5
  - RecordPayment (Project: PaymentServices, Service: PaymentService) → POST /payments
  - GetPaymentHistory (Project: PaymentServices, Service: PaymentService) → GET /payments?loanId={id}
  - ReversePayment (Project: PaymentServices, Service: PaymentService) → DELETE /payments/{id}
  - [Additional operations...]
- **Types**: PaymentRequest, PaymentResponse
- **Faults**: InvalidPayment, PaymentNotFound

---

### By Operation Type

**Create Operations** ([COUNT])
- CreateLoan (LoanServices) → POST /loans
- CreatePayment (PaymentServices) → POST /payments
- [Additional...]

**Read/Query Operations** ([COUNT])
- GetLoanDetails (LoanServices) → GET /loans/{id}
- GetPaymentHistory (PaymentServices) → GET /payments
- [Additional...]

**Update Operations** ([COUNT])
- UpdateLoan (LoanServices) → PATCH /loans/{id}
- UpdatePayment (PaymentServices) → PATCH /payments/{id}
- [Additional...]

**Delete Operations** ([COUNT])
- CloseLoan (LoanServices) → DELETE /loans/{id}
- ReversePayment (PaymentServices) → DELETE /payments/{id}
- [Additional...]

---

### Project Dependencies

**[ProjectName]** depends on:
- [Project2] (via service ServiceX, operation Y)
- [Project3] (via service ServiceZ, operation W)

**[ProjectName]** is used by:
- [ProjectA] (in service ServiceX)
- [ProjectB] (in service ServiceY)

---

### Service-to-Service Communication Map

```
LoanService → PaymentService (calls RecordPayment)
            → ReportingService (calls LogTransaction)

PaymentService → ReportingService (calls LogPayment)
              → ComplianceService (calls CheckCompliance)

ReportingService → [No outbound calls]
```

---

### Type Usage Map

**[TypeName]** (Defined in Project: [ProjectName])
- **Used in Operations**:
  - [Service1].OperationA (input)
  - [Service2].OperationB (output)
  - [Service3].OperationC (fault)
- **Inherited By**: [List of derived types]
- **Used By Projects**: [List of projects that reference this]
```
```

---

## Step 7: REST Mapping Recommendations with Project Organization

### Task
For each SOAP operation, suggest appropriate REST endpoint mapping, including project organization context.

### Output Format

```markdown
## SOAP to REST Mapping Guide

### Mapping Principles
- **CREATE Operations** → POST /resources
- **READ Operations** → GET /resources/{id}
- **UPDATE Operations** → PATCH /resources/{id}
- **DELETE Operations** → DELETE /resources/{id}
- **QUERY Operations** → GET /resources?filters

### Project-Based API Structure

```
/api/v1/
├── /loans/                    [LoanServices project]
│   ├── POST /                 (CreateLoan)
│   ├── GET /{id}              (GetLoanDetails)
│   ├── PATCH /{id}            (UpdateLoan)
│   ├── DELETE /{id}           (CloseLoan)
│   └── /history               (GetLoanHistory)
│
├── /payments/                 [PaymentServices project]
│   ├── POST /                 (RecordPayment)
│   ├── GET ?loanId={id}       (GetPaymentHistory)
│   └── DELETE /{id}           (ReversePayment)
│
└── /reports/                  [ReportingServices project]
    ├── GET /loans             (GetLoanReport)
    └── GET /payments          (GetPaymentReport)
```

### Mapped Endpoints by Project & Service

**From [Project: LoanServices] - [Service: LoanService]**:

| SOAP Operation | Service | HTTP Method | REST Endpoint | Status Codes |
|----------------|---------|-------------|---------------|--------------|
| CreateLoan | LoanService | POST | /api/v1/loans | 201, 400, 409 |
| GetLoanDetails | LoanService | GET | /api/v1/loans/{loanId} | 200, 404 |
| UpdateLoan | LoanService | PATCH | /api/v1/loans/{loanId} | 200, 404, 409 |
| CloseLoan | LoanService | DELETE | /api/v1/loans/{loanId} | 204, 404 |
| GetLoanHistory | LoanService | GET | /api/v1/loans/{loanId}/history | 200, 404 |

**From [Project: PaymentServices] - [Service: PaymentService]**:

| SOAP Operation | Service | HTTP Method | REST Endpoint | Status Codes |
|----------------|---------|-------------|---------------|--------------|
| RecordPayment | PaymentService | POST | /api/v1/payments | 201, 400, 409 |
| GetPaymentHistory | PaymentService | GET | /api/v1/loans/{loanId}/payments | 200, 404 |
| ReversePayment | PaymentService | DELETE | /api/v1/payments/{paymentId} | 204, 404 |

### Query Parameter Mapping with Headers

**GetLoansByParticipant operation** (from LoanServices project):
- **SOAP**: GetLoansByParticipantRequest with ParticipantId, Status filters
- **REST**: GET /api/v1/loans?participantId={id}&status={status}
- **Headers Required**:
  - Authorization: Bearer {token}
  - X-Correlation-Id: {UUID}
  - X-Request-Timestamp: {ISO-8601 datetime}

### SOAP Header to REST Header Mapping

**Authentication Flow**:
```
SOAP Request Header (WCF):
├── Username: admin
├── Password: encrypted
└── SessionId: ABC123

REST Equivalent:
├── Authorization: Bearer {JWT-token}
└── X-Session-Id: ABC123
```

**Tracing/Correlation**:
```
SOAP Request Header:
├── CorrelationId: UUID
├── Timestamp: ISO-8601
└── RequestId: REQ-001

REST Equivalent (HTTP Headers):
├── X-Correlation-Id: UUID
├── X-Request-Timestamp: ISO-8601
└── X-Request-Id: REQ-001
```

### Error Code Mapping with SOAP Faults

| SOAP Fault | Fault Code | HTTP Status | HTTP Response Body |
|-----------|-----------|------------|-------------------|
| InvalidParticipant | INVALID_PARTICIPANT | 404 Not Found | {"error": "INVALID_PARTICIPANT", "message": "..."} |
| InsufficientFunds | INSUFFICIENT_FUNDS | 409 Conflict | {"error": "INSUFFICIENT_FUNDS", "message": "..."} |
| ValidationError | VALIDATION_ERROR | 400 Bad Request | {"error": "VALIDATION_ERROR", "errors": [...]} |
| LoanNotFound | LOAN_NOT_FOUND | 404 Not Found | {"error": "LOAN_NOT_FOUND", "message": "..."} |

### Service-to-Service Calls (Inter-Project Communication)

**SOAP (Synchronous WCF calls)**:
```
LoanService.CreateLoan() 
  → Calls PaymentService.ValidatePaymentMethod()
  → Calls ReportingService.LogTransaction()
```

**REST (Asynchronous with fallback)**:
```
POST /api/v1/loans
  → 202 Accepted (returns Location header with tracking URL)
  → Async: Call PaymentService /api/v1/validate-method
  → Async: Call ReportingService /api/v1/logs (fire-and-forget or queue)
  → Client polls GET /api/v1/loans/{loanId}/status until ready
```

---

## Mapped Endpoints Consolidated

**All SOAP Operations → REST Endpoints**:

| Project | SOAP Service | SOAP Operation | HTTP Method | REST Endpoint | Headers Required |
|---------|--------------|----------------|-------------|---------------|-----------------|
| LoanServices | LoanService | CreateLoan | POST | /api/v1/loans | Auth, Correlation |
| LoanServices | LoanService | GetLoanDetails | GET | /api/v1/loans/{loanId} | Auth |
| LoanServices | LoanService | UpdateLoan | PATCH | /api/v1/loans/{loanId} | Auth, Correlation |
| PaymentServices | PaymentService | RecordPayment | POST | /api/v1/payments | Auth, Correlation |
| PaymentServices | PaymentService | GetPaymentHistory | GET | /api/v1/loans/{loanId}/payments | Auth |
```

---

## Step 8: Summary & Statistics with Project Analysis

### Task
Generate summary statistics and key findings with project context.

### Output Format

```markdown
## Discovery Summary & Statistics

### Solution Overview
- **Solution Path**: [Path to .sln]
- **Framework Target**: [.NET Framework version]
- **Total Projects**: [NUMBER]
- **WCF/Service Projects**: [NUMBER]

### Overall Metrics
- **Total WSDL Files**: [NUMBER]
- **Total Projects with WSDLs**: [NUMBER]
- **Total Services**: [NUMBER]
- **Total Operations**: [NUMBER]
- **Total Service Ports**: [NUMBER]
- **Total Unique Types**: [NUMBER]
- **Total Enumerations**: [NUMBER]
- **Total Fault Types**: [NUMBER]
- **SOAP Headers Defined**: [Yes/No]
- **WS-Addressing Support**: [Yes/No]

### Project-Level Statistics

| Project Name | Framework | WSDLs | Services | Operations | Types | Complexity |
|--------------|-----------|-------|----------|------------|-------|------------|
| LoanServices | .NET 4.7.2 | 2 | 2 | 18 | 25 | High |
| PaymentServices | .NET 4.7.2 | 1 | 1 | 12 | 15 | Medium |
| ReportingServices | .NET 4.7.2 | 1 | 1 | 8 | 10 | Low |

### Service Distribution
- **Loan-Related**: 2 services (CreateLoan, UpdateLoan, CloseLoan, GetLoanDetails, etc.)
- **Payment-Related**: 1 service (RecordPayment, ReversePayment, GetPaymentHistory)
- **Reporting**: 1 service (GenerateReport, GetReportHistory)

### Operation Distribution
- **Create Operations**: [N] ([%])
- **Read Operations**: [N] ([%])
- **Update Operations**: [N] ([%])
- **Delete Operations**: [N] ([%])
- **Async Operations**: [N] ([%])
- **One-Way Operations**: [N] ([%])

### SOAP Binding Styles Found
- **Document/Literal**: [N] operations
- **RPC/Encoded**: [N] operations
- **Other**: [N] operations

### Top Statistics
- **Largest Service** (by operations): [ServiceName] with [N] operations
- **Most-Used Type**: [TypeName] used in [N] operations
- **Most-Common Fault**: [FaultName] in [N] operations
- **Services with Headers**: [NUMBER]

### Cross-Project Communication
- **Services Called Between Projects**: [NUMBER]
- **Direct Dependencies**: [List of project→project calls]
- **Potential Circular Dependencies**: [List if any]

### Key Findings

1. **Architectural Observations**:
   - [Observation about service design patterns found in projects]
   - [Observation about type reuse patterns across projects]
   - [Observation about dependency structure between projects]
   - [Observation about SOAP header usage patterns]

2. **Security & Authentication**:
   - SOAP Headers for auth detected: [Yes/No]
   - Header structures found: [List of auth headers]
   - Recommendation: Migrate to [Bearer tokens | OAuth2 | etc.]

3. **Migration Considerations**:
   - [Potential challenges identified]
   - [Opportunities for optimization]
   - [Risks to watch for]
   - [Service composition patterns to replicate]

4. **Interoperability Patterns**:
   - Cross-service calls detected: [NUMBER]
   - Async patterns found: [One-way | Fire-and-forget | Polling]
   - Header propagation required: [Yes/No]

5. **Recommended Next Steps**:
   - Step 1: [Recommendation based on findings]
   - Step 2: [Recommendation based on findings]
   - Step 3: [Recommendation based on findings]

### Phase 1 Discovery Assessment
✅ WSDL Discovery Complete (all projects analyzed)
✅ SOAP Operations Documented ([NUMBER] operations)
✅ Project Dependencies Mapped
✅ Service Communication Patterns Identified
→ Ready for Phase 2: Business Requirements Documentation
→ Recommend creating [NUMBER] bounded contexts for BRD generation
→ Key services to focus on: [Service1, Service2, Service3]
→ Projects requiring parallel modernization: [Project1, Project2]
```

---

## Generation Instructions

### For DETAIL_LEVEL = "full"
Include all sections:
- Folder structure with all files
- Service information for each WSDL
- All operations with full details
- All type definitions
- All fault definitions
- All cross-references
- Complete REST mapping
- Statistics and findings

**Output Length**: 50-100+ pages (depends on number of WSDLs and operations)

### For DETAIL_LEVEL = "summary"
Include abbreviated sections:
- Folder structure (summary)
- Service information (names and counts)
- Operations list (names only, counts per service)
- Type definitions (summary only)
- Cross-reference index (simplified)
- REST mapping (endpoint summary)
- Statistics

**Output Length**: 10-20 pages

### For DETAIL_LEVEL = "minimal"
Include only key sections:
- WSDL files found (names and paths)
- Services discovered (names and operation counts)
- Total statistics
- Top operations list
- REST endpoint summary

**Output Length**: 2-5 pages

---

## Output Delivery

### If OUTPUT_FORMAT = "markdown"

Provide complete report in markdown format, copy-paste ready.

Save as: `discovery-report.md`

### If OUTPUT_FORMAT = "json"

Provide complete report in JSON format, structured for tooling/automation.

```json
{
  "metadata": {
    "discoveryDate": "2025-12-27",
    "folderPath": "FOLDER_PATH",
    "recursiveScan": true,
    "detailLevel": "DETAIL_LEVEL"
  },
  "summary": {
    "totalWsdls": NUMBER,
    "totalServices": NUMBER,
    "totalOperations": NUMBER,
    "totalTypes": NUMBER
  },
  "wsdls": [
    {
      "filename": "string",
      "path": "string",
      "namespace": "string",
      "services": ["array of service names"],
      "operations": [
        {
          "name": "string",
          "input": { "message": "string", "parameters": [...] },
          "output": { "message": "string", "parameters": [...] },
          "faults": ["array of fault names"],
          "restMapping": { "method": "GET|POST|PATCH|DELETE", "endpoint": "string" }
        }
      ],
      "types": [...]
    }
  ]
}
```

Save as: `discovery-report.json`

---

## Now Generate the Report

You have all the instructions above. Generate a comprehensive WSDL Discovery Report for the folder path: `FOLDER_PATH`

Use `DETAIL_LEVEL` to determine depth of analysis.

Use `OUTPUT_FORMAT` to determine output format.

Begin with Step 1 (Folder Structure Discovery) and proceed through all steps in order.

Ensure your output is complete, well-organized, and ready for stakeholder review and/or tool integration.

**Start now:**
