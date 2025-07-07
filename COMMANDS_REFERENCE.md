# Bot Commands Reference

## Overview
This document lists all available commands for the Telegram bot, organized by user role and functionality.

## User Roles
- **Admin**: Full access to all commands
- **Worker**: Limited access to customer management and search
- **Customer**: View-only access to their own information

---

## 🔧 **System Commands**

### `/start`
**Access:** All users  
**Description:** Initialize the bot and show role-specific keyboard  
**Usage:** `/start`

---

## 👥 **User Management Commands** (Admin Only)

### `/add_admin <uid>`
**Access:** Admin only  
**Description:** Promote a user to admin role  
**Usage:** `/add_admin 123456789`  
**Example:** `/add_admin 75516649`

### `/add_worker <uid>`
**Access:** Admin only  
**Description:** Promote a user to worker role  
**Usage:** `/add_worker 123456789`  
**Example:** `/add_worker 987654321`

### `/demote_customer <uid>`
**Access:** Admin only  
**Description:** Demote admin/worker to customer role (for resignations/terminations)  
**Usage:** `/demote_customer 123456789`  
**Example:** `/demote_customer 555666777`

### `/addcost <customer_uid> <amount>`
**Access:** Admin only  
**Description:** Add a cost entry for a customer  
**Usage:** `/addcost <customer_uid> <amount>`  
**Example:** `/addcost 123456789 25.00`

### `/viewcosts <customer_uid>`
**Access:** Admin only  
**Description:** View all cost entries for a customer  
**Usage:** `/viewcosts <customer_uid>`  
**Example:** `/viewcosts 123456789`

---

## 🔍 **Search Commands**

### `/search <search_term>`
**Access:** Admin, Worker  
**Description:** Search customers by name, UID, national ID, or phone number  
**Usage:** `/search <term>`  
**Examples:**
- `/search John`
- `/search 123456789`
- `/search 0123456789`

---

## ⚙️ **Settings Commands** (Admin Only)

### `/sethotline <text>`
**Access:** Admin only  
**Description:** Set emergency contact information  
**Usage:** `/sethotline <contact_info>`  
**Example:** `/sethotline ☎️ 012 345 678\n📞 098 765 432`

### `/gethotline`
**Access:** Admin only  
**Description:** View current emergency contact information  
**Usage:** `/gethotline`

### `/setnotificationdays <days>`
**Access:** Admin only  
**Description:** Set days before expiration to send notifications  
**Usage:** `/setnotificationdays <1-30>`  
**Example:** `/setnotificationdays 7`

### `/setnotificationtime <time>`
**Access:** Admin only  
**Description:** Set daily notification time (24-hour format)  
**Usage:** `/setnotificationtime <HH:MM>`  
**Example:** `/setnotificationtime 09:00`

### `/setgeminiapikey <key>`
**Access:** Admin only  
**Description:** Set Gemini API key for AI functionality  
**Usage:** `/setgeminiapikey <api_key>`  
**Example:** `/setgeminiapikey FEsdfwerDere5234Sffadf

### `/setgeminimodel <model>`
**Access:** Admin only  
**Description:** Set Gemini model for AI responses  
**Usage:** `/setgeminimodel <model_name>`  
**Example:** `/setgeminimodel gemini-1.5-pro`

### `/setvectordbpath <path>`
**Access:** Admin only  
**Description:** Set vector database path  
**Usage:** `/setvectordbpath <path>`  
**Example:** `/setvectordbpath ./vectors/chroma_db/`

---

## 📊 **Reporting Commands**

### `/export`
**Access:** Admin only  
**Description:** Export customer data to CSV file  
**Usage:** `/export`  
**Output:** CSV file with customer information

### `/notificationstats`
**Access:** Admin only  
**Description:** View notification system statistics  
**Usage:** `/notificationstats`  
**Output:** Success rates and notification counts

---

## 🧪 **Testing Commands** (Admin Only)

### `/testnotification <uid>`
**Access:** Admin only  
**Description:** Send test notification to verify system  
**Usage:** `/testnotification <uid>`  
**Example:** `/testnotification 123456789`

---

## 🧠 **Embedding Management Commands**

### `/reembed`
**Access:** Admin, Worker  
**Description:** Re-embed all customers whose embedding status is 'pending' or 'failed'. Use this to recover from failed or missed embeddings.  
**Usage:** `/reembed`

---

## 🧬 **Embedding Workflow & Status**

- When a new customer is added, their `embedding_status` is set to `pending`.
- The system automatically attempts to embed the customer. If successful, status becomes `success`; if not, it becomes `failed`.
- The `/reembed` command will retry embedding for all customers with `pending` or `failed` status.
- The `/force-reembed` command will delete the vector DB and rebuild all embeddings for all customers, regardless of status.
- Embedding status can be used to monitor and troubleshoot embedding issues.

---

## 📋 **Command Summary by Role**

| Command | Admin | Worker | Customer |
|---------|-------|--------|----------|
| `/start` | ✅ | ✅ | ✅ |
| `/add_admin` | ✅ | ❌ | ❌ |
| `/add_worker` | ✅ | ❌ | ❌ |
| `/demote_customer` | ✅ | ❌ | ❌ |
| `/search` | ✅ | ✅ | ❌ |
| `/sethotline` | ✅ | ❌ | ❌ |
| `/gethotline` | ✅ | ❌ | ❌ |
| `/setnotificationdays` | ✅ | ❌ | ❌ |
| `/setnotificationtime` | ✅ | ❌ | ❌ |
| `/setgeminiapikey` | ✅ | ❌ | ❌ |
| `/setgeminimodel` | ✅ | ❌ | ❌ |
| `/setvectordbpath` | ✅ | ❌ | ❌ |
| `/export` | ✅ | ❌ | ❌ |
| `/notificationstats` | ✅ | ❌ | ❌ |
| `/testnotification` | ✅ | ❌ | ❌ |
| `/reembed` | ✅ | ✅ | ❌ |
| `/force-reembed` | ✅ | ✅ | ❌ |
| `/addcost` | ✅ | ❌ | ❌ |
| `/viewcosts` | ✅ | ❌ | ❌ |

---

## 🎯 **Quick Reference by Function**

### **Customer Management**
- Add customers: Use "Add Customer" button
- Edit customers: Use "Edit Customer" button
- Search customers: `/search <term>`
- Export data: `/export`

### **User Management**
- Add admin: `/add_admin <uid>`
- Add worker: `/add_worker <uid>`
- Demote user: `/demote_customer <uid>`

### **System Configuration**
- Set hotline: `/sethotline <text>`
- Set notification days: `/setnotificationdays <days>`
- Set notification time: `/setnotificationtime <time>`
- Set AI settings: `/setgeminiapikey`, `/setgeminimodel`, `/setvectordbpath`

### **Monitoring & Testing**
- View notification stats: `/notificationstats`
- Test notifications: `/testnotification <uid>`
- Export reports: `/export`

### **Embedding Management**
- Re-embed customers: `/reembed`
- Force re-embed: `/force-reembed`

### **Cost Management**
- Add cost entry: `/addcost <customer_uid> <amount>`
- View cost entries: `/viewcosts <customer_uid>`

---

## 📝 **Usage Examples**

### **Setting up the system:**
```
/sethotline ☎️ Emergency: 012 345 678\n📞 Support: 098 765 432
/setnotificationdays 7
/setnotificationtime 09:00
/setgeminiapikey FEsdfwerDere5234Sffadf
/setgeminimodel gemini-1.5-pro
```

### **Managing users:**
```
/add_admin 75516649
/add_worker 987654321
/demote_customer 555666777
```

### **Searching customers:**
```
/search John
/search 123456789
/search 0123456789
```

### **Testing and monitoring:**
```
/testnotification 123456789
/notificationstats
/export
```

### **Embedding management:**
```
/reembed
/force-reembed
```

### **Cost management:**
```
/addcost 123456789 25.00
/viewcosts 123456789
```

---

## ⚠️ **Important Notes**

1. **Admin Commands**: Only administrators can use system configuration commands
2. **Worker Access**: Workers can search and manage customers but cannot change system settings
3. **Customer Access**: Customers can only view their own information via buttons
4. **Validation**: All commands include input validation and error handling
5. **Logging**: All command usage is logged for audit purposes

---

## 🔧 **Troubleshooting**

### **Common Issues:**
- **Permission denied**: Check your user role
- **Invalid UID**: Ensure UID is a valid number
- **Command not found**: Check command spelling and syntax
- **API errors**: Verify API keys and settings

### **Getting Help:**
- Use `/start` to see available options
- Check command usage with incorrect syntax
- Review logs for detailed error information 
