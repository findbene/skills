# n8n Workflow Patterns Reference

## Expression Syntax

### Referencing Data
```javascript
// Current item
{{ $json.fieldName }}
{{ $json["field-with-dashes"] }}
{{ $json.nested.field }}

// Previous node by name
{{ $node["HTTP Request"].json.data }}
{{ $node["HTTP Request"].json.data.items[0].id }}

// All items from a node
{{ $items("NodeName") }}
{{ $items("NodeName")[0].json.id }}

// Input items count
{{ $input.all().length }}

// Current workflow/execution info
{{ $workflow.id }}
{{ $execution.id }}
{{ $execution.resumeUrl }}    // for wait nodes

// Environment variables
{{ $env.SUPABASE_URL }}
```

### Built-in Variables
```javascript
{{ $now }}                    // current DateTime object
{{ $today }}                  // today's date
{{ $now.toISO() }}            // ISO 8601 string
{{ $now.format("yyyy-MM-dd") }}  // formatted date
{{ $now.plus({ days: 7 }) }}  // date arithmetic
{{ $runIndex }}               // current run index in loop
{{ $itemIndex }}              // current item index
```

### JavaScript in Expressions
```javascript
// String manipulation
{{ $json.name.toLowerCase() }}
{{ $json.text.split(" ").slice(0, 10).join(" ") }}  // first 10 words

// Math
{{ Math.round($json.score * 10) / 10 }}

// Conditionals
{{ $json.status === 'active' ? 'yes' : 'no' }}

// Array operations
{{ $json.tags.includes('ai') }}
{{ $json.items.filter(i => i.active).length }}
```

---

## Node Reference

### Webhook Node
```json
{
  "httpMethod": "POST",
  "path": "my-webhook",
  "responseMode": "onReceived",
  "options": {
    "rawBody": false,
    "allowedOrigins": "*"
  }
}
```
Response modes:
- `onReceived` — immediately returns 200 OK, continues async
- `lastNode` — returns last node's output (blocks until complete)
- `responseNode` — use "Respond to Webhook" node for custom response

### Schedule Trigger
```json
{
  "rule": {
    "interval": [
      {
        "field": "cronExpression",
        "expression": "0 6 * * *"
      }
    ]
  }
}
```
Common cron patterns:
```
0 * * * *       -- every hour
*/15 * * * *    -- every 15 minutes
0 6 * * *       -- 6am daily
0 6 * * 0       -- 6am every Sunday
0 9 1 * *       -- 9am first of month
```

### HTTP Request Node
```json
{
  "method": "POST",
  "url": "https://api.supabase.co/rest/v1/pipeline_runs",
  "authentication": "genericCredentialType",
  "genericAuthType": "httpHeaderAuth",
  "sendBody": true,
  "contentType": "json",
  "body": {
    "run_id": "={{ $json.run_id }}",
    "status": "={{ $json.status }}"
  },
  "options": {
    "timeout": 30000,
    "retry": {
      "enabled": true,
      "maxRetries": 3,
      "retryInterval": 2000
    }
  }
}
```

### IF Node (Branching)
```json
{
  "conditions": {
    "boolean": [],
    "number": [
      {
        "value1": "={{ $json.quality_score }}",
        "operation": "smaller",
        "value2": 7
      }
    ],
    "string": []
  }
}
```
Outputs: `true` branch and `false` branch.

### Switch Node (Multi-branch)
```json
{
  "mode": "expression",
  "output": "={{ $json.tier }}",
  "rules": {
    "rules": [
      { "value2": "1" },
      { "value2": "2" },
      { "value2": "3" }
    ]
  }
}
```

### Function Node (Custom JavaScript)
```javascript
// Access input items
const items = $input.all();

// Process each item
const results = items.map(item => {
  const data = item.json;

  return {
    json: {
      ...data,
      processed_at: new Date().toISOString(),
      score_category: data.score >= 8 ? 'high' : data.score >= 6 ? 'medium' : 'low'
    }
  };
});

return results;
```

### Split In Batches Node
```json
{
  "batchSize": 10,
  "options": {
    "reset": false
  }
}
```
Use with "Loop Over Items" pattern: SplitInBatches -> Process -> back to SplitInBatches until done.

### Merge Node
Modes:
- `append` — combine outputs from multiple branches sequentially
- `mergeByIndex` — merge item[0] from branch1 with item[0] from branch2
- `mergeByKey` — join by matching field value (like SQL JOIN)
- `combine` — all items from both branches combined

### Execute Workflow Node
```json
{
  "source": "database",
  "workflowId": "={{ $env.ENRICH_LEAD_WORKFLOW_ID }}",
  "mode": "each",
  "workflowInputs": {
    "values": {
      "lead_id": "={{ $json.lead_id }}",
      "company": "={{ $json.company }}"
    }
  }
}
```

---

## Error Handling Patterns

### Global Error Workflow
```
Settings -> Error Workflow -> select error handler workflow

Error Handler Workflow:
  Error Trigger -> Set (format message) -> Slack (notify) -> Supabase (log)
```

Available error data in error trigger:
```javascript
{{ $json.execution.error.message }}
{{ $json.execution.error.stack }}
{{ $json.execution.id }}
{{ $json.workflow.name }}
{{ $json.workflow.id }}
```

### Try/Catch Pattern (per-node)
Enable "Continue On Fail" on risky nodes: right-click -> Settings -> Continue On Fail.
On failure, the node outputs `{ "error": "error message" }` instead of throwing.

### Retry Logic in HTTP Request
```json
"options": {
  "retry": {
    "enabled": true,
    "maxRetries": 3,
    "retryInterval": 5000,
    "onStatusCodes": "429,500,502,503,504"
  }
}
```

### Wait / Resume Pattern
```json
{
  "resume": "webhook",
  "options": {
    "webhookSuffix": "/resume",
    "responseData": "lastNode"
  }
}
```
Resume by calling: `POST {{ $execution.resumeUrl }}`

---

## n8n + AI Agency Integration Patterns

### Pipeline Complete Webhook
```
Webhook (POST /pipeline-complete) ->
IF (status = error) -> Slack notify Biniyam
IF (avg_quality_score < 6.5) -> Slack notify Biniyam
-> Supabase INSERT pipeline_runs
```

### Qualified Lead Alert
```
Webhook (POST /lead-qualified) ->
Set (format message) ->
Slack (notify Biniyam with lead details + Calendly link)
-> HubSpot (create/update contact)
```

### Daily Content Calendar
```
Schedule (6am daily) ->
HTTP Request (Supabase: get today's content plan) ->
IF (has_content) ->
Execute Workflow: run_pipeline_for_each_niche
```

### Quality Score Monitor
```
Schedule (every 30min) ->
HTTP Request (Supabase: get avg score last 3 runs) ->
IF (avg < 6.5) ->
Slack alert -> HTTP Request (trigger Optimizer Agent)
```
