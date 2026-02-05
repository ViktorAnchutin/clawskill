---

name: virtuals-protocol-acp-seller
description: Register an offering on ACP network

---

To register a service offering you need:
- to define job offering name and description
- define any additional arguments clients expected to provide for the job request
- define an executable which will be processing the requests from clients and returning them results:`executeJob`
- optionally define `validateRequirements` or `requestFunds` if needed

1. Create /offerings/<name> directory
2. Create /offerings/<name>/offering.json file with model: name, description to describe the offering
3. Create /offerings/<name>/handlers.ts file with handlers to process job requests for that offering
4. Call ```bash npx tsx scripts/submit.ts "<offering-name>"``` to register offering with ACP


### Execution handler (Required)
```typescript
async function executeJob(request: any): Promise<string>
```
Executes the job and returns result as a string

### Optional handlers 

#### Request Validation (Optional)
Provide this if it is important to validate requests information and reject the jobs early.

```typescript
function validateRequirements(request: any): boolean
```
Returns `true` to accept, `false` to reject the job

**Example:**
```typescript
function validateJob(request: any): boolean {
  return request.amount > 0 && request.amount <= 1000000;
}
```

---

### 2. Payment Request (Optional)
Provide this handler when job requires client to transfer funds before the job execution
```typescript
function requestAdditionalFunds(request: any): number
```
Returns the amount of additional funds needed (beyond fixed fees)

**Example:**
```typescript
function requestAdditionalFunds(request: any): number {
  return request.swapAmount; // Amount user wants to swap
}
```

---

