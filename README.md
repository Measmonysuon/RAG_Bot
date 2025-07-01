# AI Customer Management Bot

## Overview

A Telegram bot for managing customer data, sending automated notifications, and leveraging AI-powered search and embedding. Designed for ISPs, CCTV companies, and similar service providers.

---

## Features

- Customer management (add, edit, search, export)
- Automated service expiration notifications
- Admin/worker/customer roles
- AI-powered search and RAG (Retrieval-Augmented Generation)
- Embedding management with status tracking and recovery
- Configurable settings (hotline, notification days/time, AI models)
- Audit logging and statistics

---

## Setup & Installation

1. **Clone the repository:**
   ```bash
   git clone <repo-url>
   cd AI_RAG
   ```
2. **Create and activate a virtual environment:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```
3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```
4. **Configure settings:**
   - Set your Telegram bot token, Gemini API key, and other settings using the provided bot commands (see below).
5. **Run the bot:**
   ```bash
   python3 bot/main.py
   ```

---

## Configuration

- All settings are stored in the database and can be updated via bot commands.
- Key settings:
  - `telegram_bot_token`: Your Telegram bot token
  - `gemini_api_key`: API key for Gemini AI
  - `gemini_model_name`: AI model name (e.g., `gemini-1.5-pro`)
  - `vector_db_path`: Path to vector database (default: `./vectors/chroma_db/`)
  - `notification_days_before`: Days before expiration to notify customers (default: 7)
  - `notification_time`: Daily notification time (default: 09:00)
  - `hotline`: Emergency contact info

---

## Usage: Bot Commands

### System
- `/start` — Initialize the bot and show keyboard

### User Management (Admin Only)
- `/add_admin <uid>` — Promote user to admin
- `/add_worker <uid>` — Promote user to worker
- `/demote_customer <uid>` — Demote admin/worker to customer

### Search
- `/search <term>` — Search customers by name, UID, national ID, or phone

### Settings (Admin Only)
- `/sethotline <text>` — Set emergency contact info
- `/gethotline` — View current hotline
- `/setnotificationdays <days>` — Set days before expiration for notifications
- `/setnotificationtime <HH:MM>` — Set daily notification time
- `/setgeminiapikey <key>` — Set Gemini API key
- `/setgeminimodel <model>` — Set Gemini model
- `/setvectordbpath <path>` — Set vector DB path

### Reporting
- `/export` — Export customer data to CSV
- `/notificationstats` — View notification statistics

### Testing
- `/testnotification <uid>` — Send test notification

### Embedding Management
- `/reembed` — Re-embed all customers with 'pending' or 'failed' status
- `/force-reembed` — Delete the vector DB and re-embed all customers from scratch

---

## Embedding Workflow & Status

- New customers are marked as `embedding_status = 'pending'`.
- The system auto-embeds new customers. On success, status becomes `success`; on failure, `failed`.
- `/reembed` retries embedding for all customers with `pending` or `failed` status.
- `/force-reembed` deletes the vector DB and rebuilds all embeddings for all customers.
- Embedding status is tracked for monitoring and troubleshooting.

---

## Notification System (Summary)

- **Automated daily checks** for expiring services
- **Customer notifications**: Personalized reminders with service details
- **Admin reports**: Daily summary of expiring services
- **Configurable**: Notification days, time, hotline
- **Logging**: All notification attempts and results are logged
- **Statistics**: `/notificationstats` for performance monitoring
- **Test**: `/testnotification <uid>` to verify system

### How it works
- Runs daily at configured time (default: 09:00 Phnom Penh time)
- Notifies customers with services expiring in X days
- Sends summary to all admins
- Logs all attempts and results

---

## Example Usage

### Setting up the system
```
/sethotline ☎️ Emergency: 012 345 678\n📞 Support: 098 765 432
/setnotificationdays 7
/setnotificationtime 09:00
/setgeminiapikey <your-gemini-api-key>
/setgeminimodel gemini-1.5-pro
```

### Managing users
```
/add_admin 75516649
/add_worker 987654321
/demote_customer 555666777
```

### Searching customers
```
/search John
/search 123456789
/search 0123456789
```

### Embedding management
```
/reembed
/force-reembed
```

### Testing and monitoring
```
/testnotification 123456789
/notificationstats
/export
```

---

## Troubleshooting

- **Permission denied**: Check your user role
- **Invalid UID**: Ensure UID is a valid number
- **Command not found**: Check command spelling and syntax
- **API errors**: Verify API keys and settings
- **Notifications not sending**: Check bot token, notification time, and customer expiration dates
- **High failure rate**: Ensure users have started the bot and not blocked it
- **Wrong timezone**: Verify Phnom Penh timezone and system clock

---

## Project Structure

- `bot/` — Bot logic, command handlers, notifications
- `db/` — Database and schema
- `rag/` — Embedding and vector store logic
- `utils/` — Config, logging, embedding manager
- `vectors/` — Vector database files

---

## License

MIT License 