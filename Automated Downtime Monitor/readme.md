# Automated Downtime Monitor

## Overview
This n8n workflow provides automated website downtime monitoring with real-time alerting and data logging capabilities. It continuously monitors website availability and sends instant notifications when downtime is detected.

## Features
- 🔄 **Automated Monitoring**: Scheduled downtime checks every minute
- 🚨 **Discord Alerts**: Instant notifications when sites go down
- 📊 **Data Logging**: Stores downtime/uptime data in external database
- ⚡ **Fast Detection**: 10-second timeout for quick downtime detection
- 🛡️ **Error Handling**: Comprehensive downtime scenario detection and reporting

## Workflow Components

### 1. Schedule Trigger
- **Type**: Schedule Trigger
- **Frequency**: Every minute
- **Purpose**: Initiates the downtime monitoring cycle

### 2. Check Status
- **Type**: HTTP Request
- **Target**: https://alexlonon.com
- **Method**: GET
- **Timeout**: 10 seconds
- **Headers**: Custom User-Agent (`n8n-downtime-monitor/1.0`)
- **Purpose**: Performs the actual website availability check

### 3. Handle Status
- **Type**: IF Condition
- **Logic**: Routes traffic based on HTTP response status
- **Condition**: Status code < 200 (error condition)
- **Purpose**: Determines if downtime has occurred

### 4. Code Processing
- **Type**: Code Node (JavaScript)
- **Function**: Processes HTTP response data
- **Output**: Structured downtime/uptime data object
- **Fields**:
  - URL
  - Status code
  - Response time
  - Up/down status
  - Error message (if any)
  - Timestamp

### 5. Error Handling
- **Type**: Code Node (JavaScript)
- **Purpose**: Handles network errors and downtime scenarios
- **Output**: Downtime data with appropriate status information

### 6. Send to Database
- **Type**: HTTP Request
- **Method**: POST
- **Endpoint**: Database webhook URL
- **Purpose**: Logs all downtime/uptime data for historical tracking

### 7. Downtime Detection
- **Type**: IF Condition
- **Logic**: Checks if `isUp` field is false
- **Purpose**: Triggers downtime alerts only when site is down

### 8. Discord Notification
- **Type**: HTTP Request
- **Method**: POST
- **Purpose**: Sends formatted downtime alert to Discord channel
- **Features**:
  - Rich embed formatting
  - Color-coded downtime alerts (red for down)
  - Detailed downtime error information
  - Timestamp tracking

## Environment Variables

The workflow uses environment variables to securely store sensitive information:

```env
DISCORD_WEBHOOK_URL=your_discord_webhook_url_here
DATABASE_WEBHOOK_URL=your_database_webhook_url_here
```

### Setting Up Environment Variables

1. Create a `.env` file in your workflow directory
2. Add the required variables with your actual URLs
3. Ensure the `.env` file is added to `.gitignore` for security

## Discord Downtime Alert Format

When downtime is detected, the workflow sends a Discord message with:

- 🚨 **Alert Header**: "WEBSITE DOWN ALERT"
- **Site Status**: Current HTTP status code
- **Response Time**: Last measured response time
- **Downtime Details**: Specific error message
- **Timestamp**: When the downtime was detected

## Data Structure

The workflow generates structured data for each downtime check:

```json
{
  "url": "https://alexlonon.com",
  "status": 200,
  "responseTime": 150,
  "isUp": true,
  "errorMessage": null,
  "timestamp": "2025-01-01T12:00:00.000Z"
}
```

## Setup Instructions

1. **Import Workflow**: Import the JSON file into your n8n instance
2. **Configure Environment**: Set up the `.env` file with your webhook URLs
3. **Test Connections**: Verify Discord and database webhooks are working
4. **Activate Workflow**: Enable the workflow to start downtime monitoring

## Downtime Monitoring Targets

Currently monitors:
- **Primary Site**: https://alexlonon.com

To monitor additional sites for downtime:
1. Duplicate the "Check Status" node
2. Update the URL parameter
3. Connect to the processing chain

## Downtime Scenarios Detected

- **Network timeouts** (>10 seconds)
- **HTTP errors** (4xx, 5xx status codes)
- **DNS resolution failures**
- **Connection refused errors**
- **SSL/TLS certificate issues**

## Customization Options

### Monitoring Frequency
Modify the Schedule Trigger interval in the workflow configuration.

### Timeout Settings
Adjust the timeout value in the "Check Status" node options.

### Downtime Alert Thresholds
Modify the IF conditions to change when downtime alerts are triggered.

### Discord Message Format
Customize the embed structure in the "Send to discord" node.

## Troubleshooting

### Common Issues

1. **No Downtime Alerts**
   - Verify `DISCORD_WEBHOOK_URL` is correct
   - Check Discord webhook permissions

2. **Database Logging Fails**
   - Verify `DATABASE_WEBHOOK_URL` is accessible
   - Check API endpoint authentication

3. **False Downtime Alerts**
   - Adjust timeout settings
   - Review error handling logic

### Debugging

Enable debug logging in the Code nodes to troubleshoot issues:
```javascript
console.log('Debug info:', downtimeData);
```

## Security Considerations

- ✅ Webhook URLs stored in environment variables
- ✅ No sensitive data in workflow configuration
- ✅ Custom User-Agent for identification
- ✅ Timeout limits to prevent hanging requests

## License

This workflow is provided as-is for downtime monitoring purposes. Modify as needed for your specific requirements.
