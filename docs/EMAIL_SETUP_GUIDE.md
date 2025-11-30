# Gmail Email Notification Setup Guide

## Quick Setup

### Option 1: Using Gmail Password (Less Secure - Not Recommended)

1. **Enable Less Secure Apps:**
   - Go to: https://myaccount.google.com/lesssecureapps
   - Turn ON "Allow less secure apps"
   - ⚠️ **Note:** This is less secure and Google may disable this feature

### Option 2: Using App Password (Recommended) ⭐

1. **Enable 2-Step Verification:**
   - Go to: https://myaccount.google.com/security
   - Enable "2-Step Verification" if not already enabled

2. **Generate App Password:**
   - Go to: https://myaccount.google.com/apppasswords
   - Select "Mail" and "Other (Custom name)"
   - Enter "MOTORENT" as the name
   - Click "Generate"
   - Copy the 16-character password

3. **Update Configuration:**
   - Open: `api/email_config.php`
   - Set `SMTP_PASSWORD` to the generated App Password

## Configuration

Edit `api/email_config.php`:

```php
define('SMTP_USERNAME', 'infoadminmotorent@gmail.com'); // Your Gmail
define('SMTP_PASSWORD', 'your-app-password-here');      // App Password
```

## Email Notifications

The system sends emails for:

1. **Account Approval** - When super admin approves a rental provider
2. **Account Denial** - When super admin denies an account
3. **Booking Confirmation** - To customer when booking is confirmed
4. **New Booking Notification** - To rental provider when they receive a booking
5. **Ticket Submission** - Confirmation to user who submitted a ticket

## Testing

### Test Email Configuration

1. Create a test file: `api/test_email.php`
2. Add this code:

```php
<?php
require_once 'email_config.php';
require_once 'send_email.php';

$test = testEmailConfig();
if ($test['success']) {
    $result = sendEmailAdvanced(
        'your-email@gmail.com',
        'Test Email',
        '<h1>Test Email</h1><p>If you receive this, email is working!</p>'
    );
    echo json_encode($result);
} else {
    echo json_encode($test);
}
?>
```

3. Access: `http://localhost/motorent/api/test_email.php`

## Using PHPMailer (More Reliable)

For better reliability, install PHPMailer:

1. Download PHPMailer: https://github.com/PHPMailer/PHPMailer
2. Extract to: `api/PHPMailer/`
3. The system will automatically use PHPMailer if available

## Troubleshooting

### "Email could not be sent"
- Check SMTP credentials
- Verify App Password is correct
- Check if 2-Step Verification is enabled
- Try using PHPMailer instead

### "SMTP password not configured"
- Set `SMTP_PASSWORD` in `email_config.php`

### Emails going to spam
- Check spam folder
- Verify sender email is correct
- Consider using a custom domain email

## Security Notes

- ⚠️ Never commit `email_config.php` with real passwords to version control
- ✅ Use App Passwords instead of your main Gmail password
- ✅ Keep App Passwords secure and rotate them periodically

## Disable Email Notifications

To disable emails temporarily, edit `api/email_config.php`:

```php
define('EMAIL_ENABLED', false);
```

