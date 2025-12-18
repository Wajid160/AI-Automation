# WISMO Agent Setup Guide

## Prerequisites

* n8n instance (self-hosted or cloud)
* Gmail account for customer support
* Shopify store with API access
* Google Gemini API key

## Step-by-Step Setup

### 1. Gmail Configuration

1. Enable IMAP in Gmail settings
2. Create app-specific password
3. Configure Gmail trigger in n8n

### 2. Shopify API Setup

1. Navigate to Shopify Admin → Apps → Develop apps
2. Create new app with Orders read permissions
3. Generate Admin API access token
4. Save credentials securely

### 3. n8n Workflow Import

1. Open n8n editor
2. Import `workflow/wismo-agent.n8n.json`
3. Configure credentials for each node

### 4. LLM Configuration

1. Add Google Gemini credentials
2. Configure system prompt from `prompts/system-prompt.md`
3. Set temperature and token limits

### 5. Testing

1. Send test email to support inbox
2. Monitor workflow execution
3. Verify response accuracy

## Troubleshooting

* Check API rate limits
* Verify credential scopes
* Review n8n execution logs
