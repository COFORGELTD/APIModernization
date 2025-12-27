# WSDL Analyzer & Documentor - Usage Guide

**Getting Started in 5 Minutes**

---

## 📋 Quick Start

### What It Does
Analyzes a folder of WSDL files and generates complete documentation of:
- All SOAP operations found
- All input/output parameters
- All type definitions
- All fault definitions
- REST endpoint mappings

### Time Investment
- **Preparation**: 1 minute (set folder path)
- **Generation**: 2-5 minutes (depending on WSDL count)
- **Total**: 5-10 minutes for complete documentation

### Output
- Complete discovery report (markdown or JSON)
- All SOAP operations documented
- Suggested REST mappings
- Cross-reference index
- Statistics and findings

---

## 🚀 How to Use

### Step 1: Identify Your Folder (1 minute)

Find the folder containing your WSDL files:

**Examples**:
```
C:\LegacyServices\401kLoans
C:\Projects\Schwab\Services
C:\Code\LegacyWCF\
\\NetworkShare\Services
```

Note the **exact path**.

### Step 2: Open Copilot Chat (30 seconds)

In VS Code:
```
Ctrl + Shift + I
```

The chat panel opens on the right side.

### Step 3: Set Your Parameters (1 minute)

At the top of your message to Copilot, type:

```
FOLDER_PATH = "C:\YourExactPath\401kLoans"
RECURSIVE = true
INCLUDE_TYPES = true
OUTPUT_FORMAT = markdown
NAMESPACE_FILTER = ""
DETAIL_LEVEL = full
```

**Explanation**:
- `FOLDER_PATH`: Where your WSDL files are (use exact path)
- `RECURSIVE = true`: Search all subfolders
- `INCLUDE_TYPES = true`: Document all type definitions
- `OUTPUT_FORMAT = markdown`: Generate readable report
- `NAMESPACE_FILTER = ""`: Include all namespaces (or specific one like "urn:Schwab")
- `DETAIL_LEVEL = full`: Generate complete documentation (or "summary"/"minimal")

### Step 4: Copy the Prompt (1 minute)

Open: `d:\code\migration\.github\skills\migration-expert\COPILOT_WSDL_ANALYZER_PROMPT.md`

Copy **all content** between the instructions (everything after "## Instruction Set")

### Step 5: Paste & Send (1 minute)

In Copilot Chat:
1. Type your parameters (from Step 3)
2. Press Enter
3. Paste the prompt (from Step 4)
4. Type: "Now analyze this folder and generate the WSDL discovery report"
5. Press Enter/Send

### Step 6: Wait for Report (2-5 minutes)

Copilot processes and generates complete report with:
- All WSDL files discovered
- Every SOAP operation documented
- All parameters documented
- REST mapping recommendations
- Cross-reference index
- Statistics

### Step 7: Save Output (1 minute)

**Manual save**:
1. Select all Copilot output (Ctrl+A)
2. Copy (Ctrl+C)
3. New file in VS Code (Ctrl+N)
4. Paste (Ctrl+V)
5. Save as `discovery-report.md` (Ctrl+S)

**PowerShell save**:
```powershell
$report = @"
[Paste entire Copilot output here]
"@

$report | Out-File "discovery-report.md" -Encoding UTF8
```

---

## ⚙️ Configuration Options

### Basic Configuration

**Fast Discovery** (no types):
```
FOLDER_PATH = "C:\YourPath"
RECURSIVE = true
INCLUDE_TYPES = false
OUTPUT_FORMAT = markdown
DETAIL_LEVEL = summary
```
- Faster generation (2 min)
- Less detail
- Still shows all operations

**Full Analysis** (complete):
```
FOLDER_PATH = "C:\YourPath"
RECURSIVE = true
INCLUDE_TYPES = true
INCLUDE_FAULTS = true
OUTPUT_FORMAT = json
DETAIL_LEVEL = full
```
- Complete documentation
- Type definitions included
- Machine-readable JSON
- Longer generation (5 min)

### Detail Levels

| Detail Level | Content | Time | Pages |
|--------------|---------|------|-------|
| **minimal** | WSDL names, service names, counts, summary stats | 1-2 min | 2-3 |
| **summary** | Services, operation names, basic types, endpoint summary | 2-3 min | 5-10 |
| **full** | Everything: all details, all types, all faults, cross-refs, REST mappings | 4-5 min | 50-100+ |

### Output Formats

| Format | Best For | File Type |
|--------|----------|-----------|
| **markdown** | Stakeholder review, documentation, sharing | `.md` |
| **json** | Tool integration, automation, processing | `.json` |

---

## 📁 Example: Real-World Scenario

### Your Setup
```
C:\LegacyServices\
├── LoanServices\
│   ├── LoanService.wsdl
│   ├── PaymentService.wsdl
│   └── ReportingService.wsdl
└── AccountServices\
    ├── AccountService.wsdl
    └── ComplianceService.wsdl
```

### Your Message to Copilot

```
FOLDER_PATH = "C:\LegacyServices"
RECURSIVE = true
INCLUDE_TYPES = true
OUTPUT_FORMAT = markdown
NAMESPACE_FILTER = ""
DETAIL_LEVEL = full

[Paste entire COPILOT_WSDL_ANALYZER_PROMPT.md content here]

Now analyze this folder and generate the WSDL discovery report.
```

### Result

Copilot generates comprehensive report showing:

```
WSDL DISCOVERY REPORT
Folder Path: C:\LegacyServices
Total WSDL Files: 5
Total Operations: 47
Total Types: 32

FOLDER STRUCTURE:
LoanServices/
├── LoanService.wsdl (12 operations)
├── PaymentService.wsdl (14 operations)
└── ReportingService.wsdl (8 operations)

AccountServices/
├── AccountService.wsdl (10 operations)
└── ComplianceService.wsdl (3 operations)

DETAILED OPERATIONS:

## WSDL: LoanService.wsdl

### Service: LoanService

#### Operation 1: CreateLoan
Input: CreateLoanRequest
- ParticipantId: string (required)
- Amount: decimal (required, min: 0.01, max: 50000)
- Term: int (required, min: 1, max: 360)
Output: CreateLoanResponse
- LoanId: string
- Status: enum {Approved, Pending, Rejected}
Faults: InvalidParticipant, InsufficientFunds
REST Mapping: POST /loans

#### Operation 2: GetLoanDetails
Input: GetLoanDetailsRequest
- LoanId: string (required)
Output: LoanDetails
- LoanId: string
- Principal: decimal
- Status: enum {Active, Paid-Off, Defaulted}
REST Mapping: GET /loans/{loanId}

[... all 47 operations documented ...]

CROSS-REFERENCE INDEX:

By Entity:
- Loan: CreateLoan, GetLoanDetails, UpdateLoan, CloseLoan (4 ops)
- Payment: RecordPayment, GetPaymentHistory, ReversePayment (3 ops)

By Operation Type:
- Create: 8 operations
- Read: 22 operations
- Update: 12 operations
- Delete: 5 operations

SUMMARY STATISTICS:
- Total WSDL Files: 5
- Total Operations: 47
- Total Types: 32
- Average Ops/WSDL: 9.4
- Recommended Bounded Contexts: 5

Ready for Phase 2: BRD Generation
```

---

## 🎯 What Gets Documented

### Per WSDL File:
- ✅ Filename and path
- ✅ Namespace/service name
- ✅ Available ports (SOAP, HTTP, etc.)
- ✅ All operations (names, descriptions)
- ✅ All input parameters with types
- ✅ All output parameters with types
- ✅ Fault/exception definitions
- ✅ SOAP action URIs
- ✅ Binding styles (document/literal, etc.)
- ✅ Parameter constraints (min, max, patterns, etc.)

### Type System (if INCLUDE_TYPES = true):
- ✅ Complex type definitions
- ✅ Simple type definitions and constraints
- ✅ Enumeration values
- ✅ Type inheritance/extension
- ✅ Used-by relationships

### Cross-References:
- ✅ Services-to-operations mapping
- ✅ Operations-to-REST endpoints
- ✅ Types-to-operations
- ✅ Entities-to-operations
- ✅ Service dependencies

### REST Mappings:
- ✅ Suggested HTTP method (GET, POST, PATCH, DELETE)
- ✅ Suggested endpoint paths
- ✅ Status code mappings
- ✅ Error code mappings
- ✅ Query parameter patterns

---

## 🔍 Finding WSDL Files

If you're not sure where your WSDL files are:

### Search for WSDL Files

**PowerShell**:
```powershell
Get-ChildItem -Path "C:\YourProject" -Filter "*.wsdl" -Recurse | Select-Object FullName
```

Output shows all `.wsdl` files:
```
C:\YourProject\Services\LoanService.wsdl
C:\YourProject\Services\PaymentService.wsdl
C:\YourProject\Schemas\CommonTypes.wsdl
```

### Common WSDL Locations

```
Your Project Root/
├── Services/          ← Look here
├── WebServices/       ← Or here
├── WSDL/              ← Or here
├── Schemas/           ← Or here
├── obj/               ← Generated files (skip)
└── bin/               ← Build output (skip)

Typical paths:
C:\Projects\YourApp\Services\
C:\Projects\YourApp\WebReferences\
\\NetworkShare\SharedWSDLs\
```

---

## ✅ Verification Checklist

Before running the analyzer, verify:

- ✅ You have the exact folder path
- ✅ Folder contains `.wsdl` files (you can verify with Get-ChildItem)
- ✅ You have read access to the folder
- ✅ VS Code is open with Copilot Chat available
- ✅ You have the prompt file open (COPILOT_WSDL_ANALYZER_PROMPT.md)

---

## 🆘 Troubleshooting

### Issue: "Folder not found"
**Solution**:
- Verify exact path: `Test-Path "C:\YourPath"`
- Use full path, not relative: ❌ `.\Services` → ✅ `C:\Project\Services`
- Check spelling and capitalization

### Issue: "No WSDL files found"
**Solution**:
- Verify WSDLs exist: `Get-ChildItem -Path "C:\YourPath" -Filter "*.wsdl" -Recurse`
- Check file extension is `.wsdl` (not `.xml`)
- Try parent folder if WSDLs are nested deeper

### Issue: "Copilot response incomplete/cut off"
**Solution**:
- Try "summary" detail level instead of "full"
- Ask Copilot to continue: "Continue from where you left off"
- Generate in smaller batches (by NAMESPACE_FILTER)
- Save partial output and ask Copilot to resume

### Issue: "How do I know what detail level to use?"
**Decision Guide**:
- **Minimal**: Just need a quick overview (1-2 pages)
- **Summary**: Need operation names and basic info (5-10 pages)
- **Full**: Need complete documentation with all details (50+ pages)

---

## 📊 Output Organization

After generating your report, organize it:

```powershell
# Create output folder structure
$output = ".\wsdl-discovery"
mkdir "$output\reports"
mkdir "$output\exports"

# Save main report
Copy-Item discovery-report.md -Destination "$output\reports\"

# Save JSON export (if you generated it)
Copy-Item discovery-report.json -Destination "$output\exports\"

# Create summary for stakeholders
notepad "$output\README.txt"
```

### File Naming Convention
```
discovery-report.md              ← Main markdown report
discovery-report.json            ← Machine-readable version
discovery-summary.txt            ← Quick statistics
discovery-rest-mapping.csv       ← REST endpoint list
```

---

## 🔗 Next Steps After Discovery

### Immediate (After Report Generated)
1. **Review** the report with team
2. **Validate** SOAP operations match your system
3. **Identify** business contexts/bounded contexts
4. **Get stakeholder** sign-off

### Short-term (Next 1-2 days)
1. **Extract** key services and operations
2. **Identify** 5 bounded contexts for BRD generation
3. **Plan** REST API design using REST mapping suggestions
4. **Prepare** for Phase 2 (BRD Generation)

### Medium-term (Phase 2)
1. Use discovery report to generate BRDs
2. Use SOAP operation list to create REST endpoints
3. Use type definitions for domain modeling
4. Create architecture documentation

### Long-term (Phase 3+)
1. Use BRDs to scaffold modern code
2. Use REST mappings to implement APIs
3. Use type definitions to create domain entities
4. Implement Reqnroll tests based on discovered operations

---

## 📞 Related Skills & Tools

**Phase 1 - Discovery** (This skill):
- ✅ WSDL Analyzer & Documentor
- 📄 SQL Analyzer (documents stored procedures)
- 📄 C# Code Analyzer (documents classes/methods)

**Phase 2 - Requirements** (Next):
- 📄 401k Loans BRD Generator
- 📄 Requirements Documentation Tools

**Phase 3 - Implementation** (Future):
- 📄 Clean Architecture Code Scaffolding
- 📄 MediatR/Dapper Pattern Generator

**Phase 4 - Testing** (Future):
- 📄 Reqnroll Test Generator
- 📄 Legacy Parity Test Suite

---

## 📁 File Reference

| File | Location | Purpose |
|------|----------|---------|
| Skill Definition | `.github/skills/migration-expert/wsdl-analyzer-documentor-SKILL.md` | Full skill documentation |
| Copilot Prompt | `.github/skills/migration-expert/COPILOT_WSDL_ANALYZER_PROMPT.md` | Prompt to paste in Copilot Chat |
| This Usage Guide | `.github/skills/migration-expert/WSDL_ANALYZER_USAGE_GUIDE.md` | Step-by-step instructions |

---

## ⏱️ Time Investment Summary

| Task | Time | Notes |
|------|------|-------|
| Set parameters | 1 min | Copy-paste your folder path |
| Copy prompt | 1 min | Get from COPILOT_WSDL_ANALYZER_PROMPT.md |
| Send to Copilot | 1 min | Ctrl+Shift+I, paste, send |
| Wait for generation | 2-5 min | Depends on WSDL count and detail level |
| Save output | 1 min | Ctrl+A, Ctrl+C, new file, save |
| **Total** | **6-10 min** | For complete WSDL discovery and documentation |

**Per WSDL Documented**: ~30 seconds to 2 minutes (depending on operation count)

---

## 🎓 Example: Step-by-Step Real Execution

### Your Scenario
You have 3 WSDL files in `C:\MyLegacyApp\Services\` and want complete documentation.

### Exact Steps

**Step 1** (1 min): Open Copilot Chat
```
Ctrl+Shift+I
```

**Step 2** (1 min): Type parameters
```
FOLDER_PATH = "C:\MyLegacyApp\Services"
RECURSIVE = true
INCLUDE_TYPES = true
OUTPUT_FORMAT = markdown
NAMESPACE_FILTER = ""
DETAIL_LEVEL = full
```

**Step 3** (1 min): Paste prompt
- Open: `.github/skills/migration-expert/COPILOT_WSDL_ANALYZER_PROMPT.md`
- Copy all content
- Paste in chat after parameters

**Step 4** (1 min): Add instruction
```
Now analyze this folder and generate the WSDL discovery report.
```

**Step 5** (1 min): Send
- Press Enter
- Wait (2-5 minutes)

**Step 6** (1 min): Save
- Ctrl+A (select all in Copilot output)
- Ctrl+C (copy)
- Ctrl+N (new file)
- Ctrl+V (paste)
- Ctrl+S (save as "discovery-report.md")

### Result
✅ Complete WSDL documentation in 6-10 minutes
✅ All SOAP operations documented
✅ All types documented
✅ REST mapping recommendations included
✅ Ready to share with team

---

## Ready to Start?

1. **Find your WSDL folder** (or use Search from above)
2. **Note the exact path**
3. **Follow Quick Start** (7 steps, 5-10 minutes)
4. **Save your report**
5. **Share with team**

**You're done!** You now have complete WSDL documentation.

Next: Use this report for BRD generation (Phase 2) or architecture planning.
