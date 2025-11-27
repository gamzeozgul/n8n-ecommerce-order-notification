# E-commerce Order Notification Workflow

Professional automated order processing workflow for e-commerce stores. Sends beautifully formatted HTML order confirmation emails and logs orders to Google Sheets for analytics.

## 🎯 Features

- ✅ **Webhook Integration** - Compatible with WooCommerce, Shopify, and custom e-commerce platforms
- ✅ **HTML Email Notifications** - Professional, responsive email templates
- ✅ **Google Sheets Logging** - Automatic order tracking and analytics
- ✅ **Error Handling** - Comprehensive validation and error responses
- ✅ **Data Normalization** - Handles multiple input formats
- ✅ **CORS Support** - Ready for frontend integration

## 📋 Workflow Architecture

```
Webhook → Normalize Order Data → Check Error
  ↓ (success)
Send HTML Email → Log to Google Sheets → Respond Success
  ↓ (error)
Respond Error (400/500)
```

## 🚀 Quick Start

### Prerequisites

- n8n instance (self-hosted or cloud)
- Gmail account with OAuth2 access
- Google Sheets with OAuth2 access

### Setup

1. **Import Workflow**
   - Open n8n → "Import from File" → Select `workflow.json`

2. **Configure Credentials**
   - Add Gmail OAuth2 credential (name: "Gmail Account")
   - Add Google Sheets OAuth2 credential (name: "Google Sheets Account")

3. **Setup Google Sheet**
   - Create a new Google Sheet
   - Add headers: `orderid`, `customeremail`, `ordertotal`, `orderstatus`, `timestamp`
   - Copy Sheet ID from URL (between `/d/` and `/edit`)
   - Update "Log to Google Sheets" node with your Sheet ID

4. **Activate**
   - Toggle "Active" switch
   - Copy webhook URL from Webhook node

## 📡 API Reference

**Endpoint:** `POST /webhook/order-notification`

**Request:**
```json
{
  "id": "12345",
  "customer": {
    "name": "John Doe",
    "email": "customer@example.com"
  },
  "total": "99.99",
  "status": "confirmed",
  "items": [
    {"name": "Product 1", "quantity": 2}
  ]
}
```

**Response (Success):**
```json
{
  "success": true,
  "message": "Order notification sent",
  "orderId": "12345"
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": "Missing required order data"
}
```

> **Note:** The workflow also supports Shopify-style input format (`order_id`, `customer_email`, `line_items`, etc.)

## 📊 Google Sheets Structure

| Column | Description |
|--------|-------------|
| `orderid` | Unique order identifier |
| `customeremail` | Customer email address |
| `ordertotal` | Order total amount |
| `orderstatus` | Order status (pending, confirmed, shipped, etc.) |
| `timestamp` | ISO 8601 timestamp |

## 🔒 Security Notes

- Add webhook authentication for production use
- Update CORS header (`Access-Control-Allow-Origin`) to your specific domain
- Store sensitive data in n8n environment variables
- Consider rate limiting for production

## 📸 Screenshots

![Workflow Overview](screenshots/workflow.png)

*Complete n8n workflow diagram showing all nodes and connections*

## 🐛 Troubleshooting

**Email Not Sending**
- Verify Gmail OAuth2 credentials
- Check email address format
- Review n8n execution logs

**Google Sheets Not Updating**
- Verify Sheet ID is correct
- Ensure column headers match exactly
- Check OAuth2 credentials have write access

**Webhook Returns 400 Error**
- Verify required fields: `id` (or `order_id`) and `customer.email` (or `customer_email`)
- Check request format matches expected structure

## 📝 Use Cases

- E-commerce order management and confirmations
- Automated customer communications
- Order tracking and analytics
- Multi-platform support (WooCommerce, Shopify, custom)

## 📄 License

MIT License - Feel free to use in your projects.
