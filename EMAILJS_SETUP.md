# EmailJS Setup Guide for Joyouth

This guide will help you activate the email system to send volunteer applications to both email addresses.

## Step 1: Create an EmailJS Account

1. Visit [emailjs.com](https://www.emailjs.com)
2. Click **Sign Up** and create a free account
3. Complete email verification

## Step 2: Create an Email Service

1. In your EmailJS dashboard, go to **Services**
2. Click **Add Service**
3. Select your email provider (Gmail, Outlook, etc.)
4. Follow the setup instructions for your provider
5. **Save your SERVICE_ID** (you'll need this later)

### For Gmail Users:
- You may need to generate an [App Password](https://support.google.com/accounts/answer/185833)
- Use that password in the EmailJS setup

## Step 3: Create an Email Template

1. Go to **Email Templates**
2. Click **Create New Template**
3. Configure the template as follows:

### Template Settings:
- **Template Name**: `volunteer_application` (or any name)
- **Service**: Select the service you created above
- **Recipient Email**: Add both addresses using this approach:
  - In the "To Email" field, use: `{{to_email_1}},{{to_email_2}}`
  - Or create two separate templates, one for each recipient

### Template Email Body Example:

```
Subject: New Volunteer Application from {{from_name}}

---

New Volunteer Application Received:

Name: {{from_name}}
Email: {{from_email}}
Phone: {{phone}}
Role: {{role}}
Availability: {{availability}}

Message:
{{message}}

---
This email was sent from the Joyouth volunteer form.
```

### Template Variables (must match these exactly):
- `{{from_name}}` - Applicant's name
- `{{from_email}}` - Applicant's email
- `{{phone}}` - Applicant's phone number
- `{{role}}` - Selected area of interest
- `{{availability}}` - Selected availability
- `{{message}}` - Motivation/message
- `{{to_email_1}}` - First recipient
- `{{to_email_2}}` - Second recipient

4. **Save your TEMPLATE_ID** (you'll see it in the template list)

## Step 4: Get Your Public Key

1. Go to **Account** → **API Keys**
2. Copy your **Public Key**
3. Save this key

## Step 5: Update the Website

Open `index.html` and find this section (around line 880):

```javascript
// Initialize EmailJS
emailjs.init('YOUR_PUBLIC_KEY'); 
```

Replace the values in the form submission section (search for `emailjs.send`):

```javascript
emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', {
    to_email_1: 'Joyouth.org.gh@gmail.com',
    to_email_2: 'manafnashiru55@gmail.com',
    from_name: data.name,
    from_email: data.email,
    phone: data.phone,
    role: data.role,
    availability: data.availability,
    message: data.motivation
})
```

### Replace these three values:
1. `YOUR_PUBLIC_KEY` → Your Public Key from Step 4
2. `YOUR_SERVICE_ID` → Your Service ID from Step 2
3. `YOUR_TEMPLATE_ID` → Your Template ID from Step 3

### Example (DO NOT use - replace with your actual keys):
```javascript
emailjs.init('abc123xyz789'); 
// ...
emailjs.send('gmail_service_123', 'my_template_456', {
    to_email_1: 'Joyouth.org.gh@gmail.com',
    to_email_2: 'manafnashiru55@gmail.com',
    // ... rest of template variables
})
```

## Step 6: Test the Form

1. Fill out the volunteer form on your website
2. Submit it
3. Check both email addresses to confirm you received the applications

## Troubleshooting

### "Invalid service ID" error
- Check that you copied `YOUR_SERVICE_ID` correctly
- Make sure the service is active in EmailJS dashboard

### "Invalid template ID" error
- Check that you copied `YOUR_TEMPLATE_ID` correctly
- Verify the template exists in your emailjs account

### Emails not being sent
- Check spam/junk folders
- Verify email addresses in template are correct
- Check EmailJS dashboard for error logs

### Free Plan Limits
- EmailJS free plan allows 200 emails per month
- Upgrade to a paid plan if needed: https://www.emailjs.com/pricing

## Security Note

⚠️ **Important**: Your Public Key is visible in your website code, which is safe. Service IDs and Template IDs are also visible in client-side code, this is expected and secure in EmailJS architecture.

However, never expose:
- Private Keys
- Access Tokens
- Account credentials

## Support

- EmailJS Documentation: https://www.emailjs.com/docs/
- Contact: support@emailjs.com

---

**Once configured, the volunteer system will:**
✅ Automatically send emails to both recipients
✅ Save volunteer data locally in the browser
✅ Display a success message
✅ Create a card in the Conference Members section
✅ Scroll smoothly to show the new member
