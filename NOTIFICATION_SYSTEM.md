# Notification System Documentation

## Overview

The notification system automatically sends service expiration reminders to customers and summary reports to administrators. It runs as a background service that checks for expiring services daily and sends notifications via Telegram.

## Features

✅ **Automated Daily Checks** - Runs at configurable time (default: 09:00 Phnom Penh time)  
✅ **Customer Notifications** - Personalized expiration reminders with service details  
✅ **Admin Reports** - Summary notifications to all admin users  
✅ **Configurable Settings** - Days before expiration, notification time  
✅ **Notification Logging** - Tracks all notification attempts and success rates  
✅ **Statistics** - View notification performance and success rates  
✅ **Test Notifications** - Send test messages to verify system functionality  

## Database Schema

### Notification Log Table
```sql
CREATE TABLE IF NOT EXISTS notification_log (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    customer_id INTEGER,
    customer_uid INTEGER,
    notification_type TEXT,  -- 'customer', 'admin'
    sent_to INTEGER,         -- UID of recipient
    success BOOLEAN,         -- 1 for success, 0 for failure
    sent_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

### Settings Table
```sql
CREATE TABLE IF NOT EXISTS settings (
    key TEXT PRIMARY KEY,
    value TEXT
);
```

## Configuration Settings

| Setting | Default | Description |
|---------|---------|-------------|
| `notification_days_before` | `7` | Days before expiration to send notifications |
| `notification_time` | `09:00` | Daily notification time (24-hour format) |
| `hotline` | `☎️ 012 345 678\n📞 098 765 432` | Emergency contact information |

## Admin Commands

### Notification Settings

#### Set Notification Days
```
/setnotificationdays <days>
```
Sets how many days before expiration to send notifications.

**Example:**
```
/setnotificationdays 5
```
- Valid range: 1-30 days
- Admin only

#### Set Notification Time
```
/setnotificationtime <time>
```
Sets the daily notification time in 24-hour format.

**Example:**
```
/setnotificationtime 10:30
```
- Format: HH:MM (24-hour)
- Admin only

### Testing and Monitoring

#### Test Notification
```
/testnotification <uid>
```
Sends a test notification to verify the system is working.

**Example:**
```
/testnotification 123456789
```
- Admin only
- Tests bot connectivity and message delivery

#### Notification Statistics
```
/notificationstats
```
Shows notification performance statistics.

**Example Output:**
```
📊 Notification Statistics

📅 Date: 2025-06-29

Today's Notifications:
• Customer: 5/5 (100.0%)
• Admin: 2/2 (100.0%)

Total Notifications:
• Customer: 45/50 (90.0%)
• Admin: 20/20 (100.0%)

👨‍💼 Requested by: admin_user
```

## How It Works

### 1. Daily Schedule
- System runs daily at configured time (default: 09:00 Phnom Penh time)
- Uses `schedule` library for reliable scheduling
- Runs in background thread to avoid blocking bot operations

### 2. Customer Detection
- Queries customers with `expiration_date` matching target date
- Target date = Current date + `notification_days_before` setting
- Only includes customers with `service_status != 'stopped'`

### 3. Customer Notifications
- Sends personalized message to each customer
- Includes service details, payment amount, contact info
- Uses Markdown formatting for better readability

**Example Customer Message:**
```
⚠️ Service Expiration Reminder

Dear John Doe,

Your Home Internet service will expire on:
📅 2025-07-06

💰 Payment Amount: $25.00
📋 Service Plan: Basic Plan

📞 Contact: 0123456789

Please renew your service to avoid interruption.
Thank you for choosing our services!
```

### 4. Admin Reports
- Sends summary to all admin users
- Groups customers by service type
- Shows payment amounts and contact details

**Example Admin Message:**
```
📊 Service Expiration Report

📅 Date: 2025-06-29
⚠️ Customers with expiring services: 3

🛠️ Home Internet (2 customers):
  • John Doe (UID: 123456789) - $25.00
  • Jane Smith (UID: 987654321) - $30.00

🛠️ CCTV/NVR/DVR (1 customers):
  • Bob Wilson (UID: 555666777) - $50.00

Please contact these customers to ensure service renewal.
```

### 5. Logging and Tracking
- Logs all notification attempts
- Tracks success/failure rates
- Stores recipient and timestamp information
- Enables performance monitoring

## Implementation Details

### Core Classes

#### NotificationManager
Main class that handles all notification operations:

```python
class NotificationManager:
    def __init__(self, bot_token)
    def get_expiring_customers(self)
    def send_customer_notification(self, customer_data)
    def send_admin_notification(self, expiring_customers)
    def log_notification(self, customer_id, customer_uid, notification_type, sent_to, success)
    def check_and_send_notifications(self)
    def schedule_notifications(self)
    def start_notification_service(self)
    def stop_notification_service(self)
    def send_test_notification(self, target_uid)
    def get_notification_stats(self)
```

### Timezone Handling
- Uses Phnom Penh timezone (UTC+7)
- All timestamps stored and displayed in local time
- Automatic timezone conversion for consistency

### Error Handling
- Graceful handling of Telegram API errors
- Logs all errors for debugging
- Continues operation even if some notifications fail
- Fallback mechanisms for network issues

### Performance Considerations
- Efficient database queries with proper indexing
- Background thread execution
- Minimal resource usage
- Configurable scheduling intervals

## Installation and Setup

### 1. Install Dependencies
```bash
pip install schedule==1.2.0
```

### 2. Database Migration
The notification system automatically creates required tables:
- `notification_log` - Tracks notification history
- `settings` - Stores configuration (if not exists)

### 3. Initialize System
The notification system is automatically initialized when the bot starts:

```python
# In main.py
from bot.notifications import init_notification_system, start_notifications

# Initialize
notification_manager = init_notification_system(TOKEN)

# Start service (in main function)
start_notifications()
```

### 4. Configure Settings
Set up initial notification settings:

```python
# Set notification days
/setnotificationdays 7

# Set notification time
/setnotificationtime 09:00
```

## Monitoring and Maintenance

### View Statistics
Use `/notificationstats` to monitor:
- Daily notification counts
- Success/failure rates
- Overall system performance

### Test System
Use `/testnotification <uid>` to:
- Verify bot connectivity
- Test message delivery
- Debug notification issues

### Log Analysis
Check logs for:
- Notification delivery status
- Error messages
- Performance metrics

## Troubleshooting

### Common Issues

#### Notifications Not Sending
1. Check bot token is valid
2. Verify notification time setting
3. Check customer expiration dates
4. Review error logs

#### High Failure Rate
1. Check if users have started the bot
2. Verify users haven't blocked the bot
3. Check network connectivity
4. Review Telegram API limits

#### Wrong Timezone
1. Verify Phnom Penh timezone setting
2. Check system clock
3. Review timezone conversion logic

### Debug Commands
```python
# Check current settings
print(get_setting('notification_days_before'))
print(get_setting('notification_time'))

# Test notification manually
send_test_notification(target_uid)

# Get statistics
stats = get_notification_stats()
print(stats)
```

## Security Considerations

- Admin-only access to notification settings
- Secure token handling
- Input validation for all commands
- Error message sanitization
- Audit logging for all changes

## Future Enhancements

Potential improvements:
- Multiple notification channels (email, SMS)
- Custom notification templates
- Advanced scheduling (multiple times per day)
- Notification preferences per customer
- Integration with payment systems
- Automated renewal reminders
- Notification history export

## Support

For issues or questions:
1. Check the logs for error messages
2. Use `/notificationstats` to verify system status
3. Test with `/testnotification` command
4. Review this documentation
5. Contact system administrator 