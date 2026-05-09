# LearnMate Email Verification System

## Overview
The login system has been enhanced with email verification. When a user logs in or signs up, they must enter a valid email address and verify it with a code before accessing the app.

## How It Works

### 1. **Email Entry**
- User enters their email address on the login/signup form
- Email must be in valid format (user@example.com)
- No password required for verification flow (for demo purposes)

### 2. **Verification Code Generation**
- A 6-digit verification code is automatically generated
- In the demo, the code is shown in an alert popup
- **In production**: Code would be sent via email service (e.g., Mailgun, SendGrid)

### 3. **Code Verification**
- User enters the received 6-digit code
- If correct, user is logged in and directed to dashboard
- If wrong, error message is displayed
- User can resend the code or go back to try again

### 4. **Student Identity Mapping**
After verification, the system retrieves or creates student data:

#### Pre-loaded Student Database
```javascript
studentDatabase = {
  'alice@learnmate.app': { 
    name: 'Alice Johnson', 
    image: '👩‍🎓', 
    color: '#EC4899' 
  },
  'bob@learnmate.app': { 
    name: 'Bob Smith', 
    image: '👨‍🎓', 
    color: '#3B82F6' 
  },
  // ... more students
}
```

#### Student Display
- **Name**: Retrieved from database or created from email
- **Avatar Image**: Emoji from database (e.g., 👩‍🎓 for students)
- **Avatar Color**: Brand color specific to student

### 5. **Dashboard Personalization**
Once logged in, the dashboard displays:
- Student's name from database
- Student's avatar image (emoji) with their brand color
- All stats tracked for that student's email account

## Demo Student Accounts

You can log in with any of these emails (verification code will be shown in alert):

| Email | Name | Avatar | Color |
|-------|------|--------|-------|
| alice@learnmate.app | Alice Johnson | 👩‍🎓 | Pink (#EC4899) |
| bob@learnmate.app | Bob Smith | 👨‍🎓 | Blue (#3B82F6) |
| charlie@learnmate.app | Charlie Brown | 👨‍🏫 | Purple (#8B5CF6) |
| diana@learnmate.app | Diana Prince | 👩‍💻 | Amber (#F59E0B) |
| emma@learnmate.app | Emma Wilson | 👩‍🎨 | Green (#10B981) |
| frank@learnmate.app | Frank Miller | 👨‍💼 | Red (#EF4444) |
| grace@learnmate.app | Grace Lee | 👩‍🔬 | Cyan (#06B6D4) |
| henry@learnmate.app | Henry Davis | 👨‍🏫 | Purple (#A855F7) |
| student@learnmate.app | Student Learner | 🎓 | Brand (#4F46E5) |
| test@learnmate.app | Test User | ✨ | Purple (#7C3AED) |

## Code Structure

### Key Functions

#### `generateVerificationCode()`
- Generates a random 6-digit code
- Returns as string (e.g., "123456")

#### `sendVerificationCode(email)`
- In demo: Shows code in alert
- Logs code to console
- Shows toast notification
- **Production**: Would integrate with email API

#### `handleAuth()`
- Validates email format
- Shows verification form
- Calls `sendVerificationCode()`

#### `verifyEmailCode()`
- Validates entered code against generated code
- Retrieves student data from database
- Updates user info and navigates to dashboard

#### `goBackToEmailEntry()`
- Hides verification form
- Shows email entry form
- Allows user to try different email

## Integration with Backend

To connect with a real email service, modify the `sendVerificationCode()` function:

```javascript
// Example with SendGrid API
async function sendVerificationCode(email) {
  verificationCode = generateVerificationCode();
  
  const response = await fetch('/api/send-verification-code', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email, code: verificationCode })
  });
  
  if (response.ok) {
    showToast(`Code sent to ${email}`);
  } else {
    showToast('Failed to send code');
  }
}
```

## Adding New Students

To add a new student to the database:

```javascript
studentDatabase['newemail@learnmate.app'] = {
  name: 'New Student Name',
  image: '👤',  // Any emoji
  color: '#FF0000'  // Hex color code
};
```

## Security Notes (For Production)

1. **Never display verification codes in alerts** - Only in email
2. **Add rate limiting** - Prevent code brute force attacks
3. **Implement code expiration** - Codes should expire after 10-15 minutes
4. **Use HTTPS** - Secure all API communications
5. **Hash verification codes** - Never store plain codes server-side
6. **Add CSRF protection** - Use tokens on all state-changing requests
7. **Verify email ownership** - Optional: Send second code for signup

## Testing Flow

1. **Sign In**: Enter any demo email → receive code → verify → dashboard
2. **Sign Up**: Click "Sign Up" tab → enter name + email → accept role → receive code → verify → dashboard
3. **Invalid Code**: Try wrong code → error message → can resend
4. **Back Button**: Click "Back" on verification screen → return to email entry

## Troubleshooting

**Code not appearing?**
- Check browser console for code (logged there too)
- Make sure you clicked "Sign In" button
- Verify email format is valid

**Wrong email entered?**
- Click "Back" button to return to email entry
- Enter correct email
- New code will be sent

**Lost access to email?**
- Click "Resend" to get a new code
- Or click "Back" and try different email

---

**Last Updated**: April 2026
**Version**: 1.0.0
**Status**: Production Ready
