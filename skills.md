# 401k Loans BRD Generation with Source Code Reference
 
Copy and paste this entire prompt into GitHub Copilot Chat. Update `SOURCE_CODE_FOLDER` and `VARIATION` to generate BRDs enriched with source code analysis.
 
---
 
```
You are a Retirement Plan Domain Architect specializing in 401k loan administration systems. Generate a comprehensive Business Requirements Document (BRD) for legacy 401k Loans SOAP services migration to .NET 8.0 Clean Architecture, enriched with source code analysis.
 
SYSTEM: Legacy 401k Loans C# SOAP Services → .NET 8.0 Clean Architecture
DOMAIN: Retirement 401k Loans Administration
TARGET FRAMEWORK: .NET 8.0+
ARCHITECTURE: Clean Architecture + DDD + BDD (Reqnroll/Gherkin)
COMPLIANCE: IRS Loan Rules, Plan Document Requirements
SOURCE_CODE_FOLDER: [PROVIDE PATH - e.g., "C:\MyLegacyServices\401kLoans" or "https://github.com/org/401k-loans-service"]
VARIATION: [SELECT 1-5]
 
---
 
## SOURCE CODE FOLDER STRUCTURE (Expected)
 
Organize your source code reference folder as follows:
 
```
401k-loans-source/
├── WSDL/
│   ├── LoanService.wsdl          (Legacy SOAP contract)
│   ├── LoanPaymentService.wsdl
│   └── LoanReportingService.wsdl
├── Schema/
│   ├── LoanDomain.sql            (Database schema for loans)
│   ├── StoredProcedures.sql      (All sp_* procedures)
│   └── Tables.sql
├── Source/
│   ├── LoanService.cs            (SOAP service implementation)
│   ├── LoanPaymentService.cs
│   ├── LoanReportingService.cs
│   └── Models/
│       ├── LoanRequest.cs
│       ├── LoanResponse.cs
│       ├── PaymentRequest.cs
│       └── PaymentResponse.cs
├── BusinessRules/
│   ├── LoanEligibilityRules.cs   (IRS 72(p) enforcement)
│   ├── LoanCalculations.cs       (Amortization, interest)
│   └── ComplianceValidation.cs
└── Tests/
    ├── LoanServiceTests.cs
    └── LoanCalculationTests.cs
```
 
If you don't have some files, omit them and we'll note gaps in the BRD.
 
---
 
## VARIATION MATRIX FOR 401K LOANS WITH SOURCE CODE ANALYSIS
 
| VARIATION | BOUNDED_CONTEXT | FOCUS_AREA | EXTRACTION_PRIORITY | WSDL_OPERATIONS | STORED_PROCEDURES_TO_FIND | BUSINESS_LOGIC_PATTERNS | TEST_PRIORITY |
|-----------|-----------------|-----------|-------------------|-----------------|---------------------------|------------------------|------------------|
| **1** | Loan Origination & Approval | New loan applications, eligibility verification, approval workflow | Extract WSDL operations + eligibility rules + loan amount calculations | GetLoanEligibility, CreateLoan, ApproveLoan, CalculateMaxLoanAmount | sp_GetParticipantVestingBalance, sp_CheckOutstandingLoans, sp_CalculateMaxLoanAmount, sp_ValidateLoanEligibility, sp_CreateLoan | Eligibility validation logic, max loan calculation algorithm, participant status checks | Loan approval accuracy, eligibility parity |
| **2** | Loan Repayment & Payment Processing | Payment collection, interest accrual, amortization, delinquency management | Extract payment logic + amortization calculations + interest formulas | RecordPayment, GetAmortizationSchedule, CalculatePayoff, ProcessDelinquency | sp_GetLoanAmortizationSchedule, sp_RecordPayment, sp_AccrueInterest, sp_UpdateLoanBalance, sp_GetDelinquentLoans, sp_CalculatePayoffAmount | Amortization schedule generation, compound interest calculation, payment application order | Payment processing accuracy, schedule parity |
| **3** | Loan Offset & Distribution Handling | Distribution-triggered loan offset, setoff processing, 1099-R tax reporting | Extract distribution logic + offset calculations + tax reporting | CalculateLoanOffset, ProcessDistribution, GenerateTaxReport | sp_CalculateLoanOffset, sp_GetActiveLoansByParticipant, sp_ProcessDistributionSetoff, sp_UpdateLoanStatusToOffset, sp_Generate1099RWithSetoff | Setoff calculation algorithm, distribution net amount computation, tax withholding logic | Offset accuracy, tax reporting parity |
| **4** | Loan Termination & Status Changes | Loan closure, participant status impact, default handling, plan termination | Extract status transition logic + closure procedures + default rules | CloseLoan, UpdateLoanStatus, ProcessDefault, HandleParticipantStatusChange | sp_CloseLoan, sp_RecordLoanDefault, sp_MonitorParticipantStatus, sp_HandleDeathDisability, sp_ProcessMandatoryCashout | Status state machine, default trigger logic, participant transition rules | Status change accuracy, termination parity |
| **5** | Loan Reporting & Compliance | Loan reporting, compliance validation, audit trails, regulatory filing, statements | Extract reporting logic + compliance checks + audit trail implementation | GenerateLoanReport, ValidateCompliance, GenerateParticipantStatement | sp_GetLoanLedger, sp_ValidateLoanCompliance, sp_GenerateLoanReport, sp_GetPaymentHistory, sp_Generate5500LoanSchedule, sp_GenerateParticipantStatement | Compliance validation rules, report generation logic, audit trail recording | Report accuracy, compliance parity |
 
---
 
## ENHANCED BRD GENERATION TEMPLATE WITH SOURCE CODE ANALYSIS
 
Generate the following sections using values from [VARIATION] PLUS extracted source code insights.
 
### 0. SOURCE CODE ANALYSIS FINDINGS
Before generating main BRD sections, analyze provided source code and report:
 
**WSDL Operations Discovered:**
- For each WSDL file found, list:
  - SOAP operation name (from portType)
  - Input message structure (parameters)
  - Output message structure (return type)
  - Fault types (error handling)
  - Operation documentation/comments
 
Example:
```
LoanService.wsdl:
  Operation: GetLoanEligibility
    Input: GetLoanEligibilityRequest { participantId, planId, requestedAmount }
    Output: GetLoanEligibilityResponse { isEligible, maxAmount, reasonCode }
    Faults: InvalidParticipant, PlanNotFound, CalculationError
```
 
**Legacy C# Classes Discovered:**
- For each .cs file found, extract:
  - Class name and namespace
  - Public methods (business operations)
  - Key properties (data model)
  - Business logic code snippets (business rules)
  - Comments/documentation about rules
 
Example:
```
Namespace: Schwab.RetirementLoans.Services
Class: LoanService
  Method: CalculateMaxLoanAmount(participantId, vestedBalance)
    Logic: return Math.Min(vestedBalance * 0.5, 50000);
    Comment: "IRS 72(p) limit enforcement"
```
 
**Stored Procedures Discovered:**
- For each sp_* found, extract:
  - Procedure signature (parameters, return type)
  - SQL logic (business rule implementation)
  - Comments indicating IRS/Plan requirements
  - Performance notes (indexes, execution time)
 
Example:
```
sp_CalculateMaxLoanAmount
  Parameters: @ParticipantId INT, @VestedBalance DECIMAL
  Logic: RETURN CASE WHEN (@VestedBalance * 0.5) < 50000
           THEN (@VestedBalance * 0.5) ELSE 50000 END
  Rule: IRS Section 72(p) - Lesser of 50% or $50k
```
 
**Business Logic Patterns Found:**
- Validation patterns (input checking)
- Calculation patterns (formulas, algorithms)
- State machine patterns (loan status transitions)
- Error handling patterns (exception/error codes)
- Audit trail patterns (logging approach)
 
---
 
### 1. EXECUTIVE SUMMARY
- **Bounded Context**: [BOUNDED_CONTEXT from VARIATION]
- **Business Priority**: P0-Core/Critical or P1-High/Medium (based on source code usage patterns)
- **Complexity Level**: [FOCUS_AREA complexity]
- **Primary Focus**: [FOCUS_AREA]
- **Source Code Analysis**: Summary of what was discovered in provided source code
- Write 3-4 sentences on migration scope, regulatory criticality, and complexity based on source code findings.
 
---
 
### 2. DOMAIN DECOMPOSITION & AGGREGATES (Source Code Informed)
Extract from source code:
- **Classes Used**: List C# classes from source code that define loan domain
- **Business Flows**: Decompose [FOCUS_AREA] using method calls found in source code
- **Aggregate Roots**: Loan (primary), LoanPayment, LoanAmortizationSchedule - validate against source code models
- **Value Objects**: Extract from source code property types (LoanAmount class, InterestRate class, etc.)
- **Domain Events**: Infer from method names and business logic (PaymentRecorded, InterestAccrued, etc.)
- **Key Calculations**: Extract actual formulas from source code (e.g., interest accrual algorithm)
 
---
 
### 3. WSDL → REST ENDPOINT MAPPING (Source Code Validated)
For each WSDL operation found in source code:
- **Legacy SOAP Operation**: [Operation name from WSDL]
- **Input Parameters**: [Actual parameters from WSDL/C# request class]
- **Output Structure**: [Actual response from WSDL/C# response class]
- **Modern REST Endpoint**: Derived HTTP verb + path
- **Request DTO**: Mapped from WSDL request structure
- **Response DTO**: Mapped from WSDL response structure
- **HTTP Status Codes**: 200, 201, 400, 409, 422, 500 (with 401k meanings)
 
Example from source code:
```
SOAP (from LoanService.wsdl):
  Operation: GetLoanEligibility
  Request: <GetLoanEligibilityRequest><ParticipantId>123</ParticipantId>...</GetLoanEligibilityRequest>
  Response: <GetLoanEligibilityResponse><IsEligible>true</IsEligible>...</GetLoanEligibilityResponse>
 
REST Mapping:
  Endpoint: POST /api/loans/eligibility-check
  Request: { participantId, planId, requestedLoanAmount }
  Response: { isEligible, maxLoanAmount, vestedBalance, activeLoans }
```
 
---
 
### 4. BUSINESS RULES FROM SOURCE CODE ANALYSIS
Extract actual business logic from C# code and SQL:
 
**Rule: [RULE_NAME from source code]**
- **Source Location**: [File name and method/procedure where rule is implemented]
- **Code Implementation**: [Actual code snippet from source code showing the rule]
- **IRS/Plan Reference**: Which IRS section or plan clause this implements
- **Validation Logic**: Conditions checked, constraints enforced
- **Data Required**: Parameters and data lookups
- **Impact**: Which business flows are affected
 
Example:
```
Rule: Maximum Loan Amount Limit
Source: LoanService.cs, CalculateMaxLoanAmount() method
Code: return Math.Min(vestedBalance * 0.5, 50000);
IRS: Section 72(p) - Lesser of 50% of vested balance or $50,000
Validation: Check both vested balance and hard $50k cap
Impact: Loan Origination flow - rejects loans exceeding this amount
```
 
---
 
### 5. STORED PROCEDURE MAPPING FROM SOURCE CODE
List [STORED_PROCEDURES_TO_FIND] extracted from source code:
 
**Procedure**: [sp_* name from SQL or source code reference]
- **Source File**: [SQL file or C# method calling it]
- **Purpose**: [Business operation it supports - from SQL comments or code context]
- **Parameters**: [Input/output parameters from stored procedure signature]
- **Business Rule**: [Which rules from section 4 it enforces]
- **Key SQL Logic**: [Actual SQL from stored procedure showing calculations/logic]
- **Performance**: [Any indexes, execution time notes, or caching strategy]
- **Audit Trail**: [How changes are recorded - from source code logging]
 
Example:
```
Procedure: sp_CalculateMaxLoanAmount
Source File: StoredProcedures.sql
Parameters:
  @ParticipantId INT
  @VestedBalance DECIMAL
  RETURN DECIMAL
SQL Logic:
  RETURN CASE
    WHEN (@VestedBalance * 0.5) < 50000 THEN (@VestedBalance * 0.5)
    ELSE 50000
  END
Rule Enforced: IRS 72(p) maximum loan limit
```
 
---
 
### 6. VALIDATION RULES FROM SOURCE CODE
Extract each validation from [VALIDATION_RULES]:
 
**Validation**: [Rule name from source code]
- **Source Code Location**: [File and method implementing validation]
- **Implementation**: [Code snippet showing how validation is performed]
- **Constraints**: [Values/ranges/formulas from source code]
- **Error Handling**: [Actual error codes/messages from source code]
- **Legacy Parity**: [How current system implements same validation]
 
---
 
### 7. GHERKIN TEST SCENARIOS (From Source Code Patterns)
Generate feature files based on actual source code test patterns:
 
**Feature: [BOUNDED_CONTEXT] - Test cases from source code**
```gherkin
Scenario: [Test scenario name from source code tests if available]
  Given [preconditions extracted from test setup]
  When [action from source code business logic]
  Then [expected outcome from test assertions]
  And [any audit trail requirements from source code]
```
 
If source code tests exist, extract test scenarios and convert to Gherkin format.
 
---
 
### 8. CLEAN ARCHITECTURE MAPPING (Based on Source Code Structure)
Analyze source code organization and map to Clean Architecture:
 
**Domain Layer** (from source code domain models):
- **Entities**: Classes with identity from source code
- **Value Objects**: Immutable classes from source code models
- **Domain Services**: Service classes in source code
- **Specifications**: Validation/business rule classes from source code
 
**Application Layer** (from source code service/handler patterns):
- **Commands**: State-changing operations from source code methods
- **Queries**: Read operations from source code methods
- **Handlers**: Implementation from source code SOAP services
 
**Infrastructure Layer** (from source code data access):
- **Repository**: Data access patterns from source code
- **Dapper Mapping**: SQL-to-object mapping from source code
- **External Services**: Dependencies in source code
 
**Presentation Layer** (from WSDL/SOAP contracts):
- **Controllers**: REST endpoints derived from WSDL operations
- **DTOs**: Data transfer objects from WSDL request/response
 
---
 
### 9. LEGACY PARITY VERIFICATION (With Source Code Baseline)
For each business operation in [FOCUS_AREA]:
 
**Operation**: [Name from WSDL or source code method]
- **Legacy Implementation**: [Actual source code logic]
- **New REST Equivalent**: [Proposed REST implementation]
- **Parity Test**: [How to validate both produce same result]
- **Source Code Baseline**: [Exact code snippet from legacy system to match]
 
---
 
### 10. DECISION RECORDS WITH SOURCE CODE JUSTIFICATION
Document architectural decisions based on source code analysis:
 
- **Why this bounded context**: Evidence from source code dependencies/usage
- **MediatR pattern choice**: Based on source code transaction boundaries
- **Dapper vs EF Core**: Source code already uses SQL procedures extensively
- **Async/Await strategy**: Informed by source code performance patterns
- **Error handling**: Patterns found in source code error management
 
---
 
## EXECUTION INSTRUCTIONS
 
### Step 1: Prepare Source Code Reference
```
Create a folder with legacy 401k loans source code:
- Copy WSDL files to WSDL/
- Copy legacy C# classes to Source/
- Copy stored procedures SQL to Schema/
- Copy business rule classes to BusinessRules/
- Copy existing unit tests to Tests/
 
Do NOT include compilation artifacts, .obj files, bin/obj directories.
Include only: *.wsdl, *.cs, *.sql, *.xml (WSDL)
```
 
### Step 2: Provide Source Code Location
When running this prompt, specify:
```
SOURCE_CODE_FOLDER = "C:\Legacy401kLoans" (local folder)
OR
SOURCE_CODE_FOLDER = "https://github.com/myorg/401k-loans/tree/main/legacy"
```
 
### Step 3: Run BRD Generation
```
1. Copy this entire prompt
2. Paste into GitHub Copilot Chat
3. Replace [SOURCE_CODE_FOLDER] with actual path/URL
4. Replace [VARIATION] with number 1-5
5. Copilot will analyze source code + generate BRD
```
 
### Step 4: Generate All 5 Variations
Repeat steps 2-3 for each VARIATION (1-5)
 
---
 
## SOURCE CODE ANALYSIS EXPECTATIONS
 
Copilot will analyze and extract:
 
✅ **From WSDL Files**:
- All SOAP operations (portType, message definitions)
- Request/response structures
- Data types and constraints
- Fault definitions
 
✅ **From C# Source Code**:
- Business logic implementations
- Calculation algorithms
- Validation rules
- State machines and workflows
- Error handling patterns
- Comments explaining IRS/plan rules
 
✅ **From Stored Procedures**:
- SQL business logic
- Parameter definitions
- Transaction boundaries
- Performance characteristics
- Audit trail implementation
 
✅ **From Unit Tests** (if present):
- Real test scenarios
- Expected behavior
- Edge cases and error conditions
- Data setup/teardown logic
 
❌ **Will NOT Extract**:
- Compiled binaries (.dll, .exe, .obj)
- Build artifacts
- Configuration secrets or credentials
- Third-party library source (only your code)
- Source code from current workspace (as requested)
 
---
 
## OUTPUT REQUIREMENTS
 
✅ **Must Include**:
- Section 0: Actual source code analysis findings (what was discovered)
- WSDL operations mapped from actual legacy contracts
- Real C# business logic converted to domain rules
- Actual stored procedures from legacy schema
- Source code references in every applicable section
- Validation rules extracted from legacy code
 
✅ **Must Reference**:
- File names and method names from source code
- Actual SQL/C# code snippets
- Real error codes and messages from source code
- Test scenarios from source code tests (if available)
 
❌ **Must NOT**:
- Invent business logic not in source code
- Reference your current workspace files
- Include compilation artifacts or build output
- Copy entire source files (only snippets needed)
 
---
 
## GENERATE 401K LOANS BRD WITH SOURCE CODE NOW
 
1. Prepare your legacy 401k loans source code in a folder
2. Copy this entire prompt above
3. Paste into GitHub Copilot Chat
4. Specify: SOURCE_CODE_FOLDER = [your path] and VARIATION = [1-5]
5. Copilot will analyze source code + generate rich BRD
 
Example chat message:
```
SOURCE_CODE_FOLDER = "C:\MyLegacy401kLoans"
VARIATION = 1
 
[Paste full prompt above]
 
Generate BRD for 401k Loans Loan Origination & Approval, analyzing the source code provided.
```
```
 
---
 
## Notes
 
- **Source code is optional**: If you don't have legacy code, remove sections 0-1 and use the variation matrix defaults
- **Partial source code OK**: If you only have WSDL or only SQL, still paste what you have
- **Security**: Don't include credentials, secrets, or sensitive data in source code folder
- **Size**: If source code is >100MB, provide a GitHub link instead of local folder
 
 