# 🔐 Admin Portal Access Guide

## Secure Admin Portal

### Portal URL
```
/secure-admin-portal.html
```

### Default Credentials
```
Email: admin@admin.com
Password: admin123
```

### Security Features
✅ Separate secure login page  
✅ 24-hour session tokens  
✅ Auto-logout on expiry  
✅ Access control on all admin pages  
✅ Firebase Authentication  
✅ No admin links on public site  

### Creating First Admin Account

1. Open browser console on `secure-admin-portal.html`
2. Run:
   ```javascript
   setupAdmin()
   ```
3. Login with credentials above
4. **IMPORTANT**: Change password immediately after first login

### Admin Dashboard Features

#### 1. Apps Management Tab
- Add new apps with metadata
- Edit existing apps
- Delete apps
- Upload technical specifications
- Manage screenshots
- Track download counts

#### 2. Hero Banners Tab
- Create slider banners
- Upload images (1200x400 recommended)
- Set navigation links
- Control display order
- Edit/Delete banners

#### 3. Site Settings Tab (NEW)
- Set site name & tagline
- Upload custom logo
- Configure contact emails
- View security status
- Manage site configuration

### Session Management

**Session Duration**: 24 hours  
**Auto-logout**: Yes, after expiry  
**Token Storage**: sessionStorage (cleared on logout)

### Security Best Practices

1. **Change Default Password**: Immediately after setup
2. **Use Strong Password**: Minimum 12 characters
3. **Enable 2FA**: Through Firebase Console
4. **Regular Audits**: Check Firebase Auth logs
5. **Firestore Rules**: Implement strict security rules

### Firebase Security Rules (Recommended)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Apps collection - Public read, Admin write
    match /apps/{appId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.token.email == 'admin@admin.com';
    }
    
    // Banners collection - Public read, Admin write
    match /banners/{bannerId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.token.email == 'admin@admin.com';
    }
    
    // Settings collection - Public read (for site config), Admin write
    match /settings/{docId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.token.email == 'admin@admin.com';
    }
  }
}
```

### Troubleshooting

**Issue**: Can't access admin.html  
**Solution**: Go to `secure-admin-portal.html` first to login

**Issue**: Session expired error  
**Solution**: Token expired after 24h, login again

**Issue**: Unauthorized access alert  
**Solution**: Clear sessionStorage and login again

**Issue**: Admin not found  
**Solution**: Run `setupAdmin()` in console to create

### Access Flow

```
User → secure-admin-portal.html
  ↓ (Login with Firebase Auth)
sessionStorage.setItem('admin_access_token')
  ↓ (Token stored)
Redirect → admin.html
  ↓ (Token verified)
Dashboard Access Granted
```

### Emergency Access

If locked out:
1. Clear all browser storage
2. Go to `secure-admin-portal.html`
3. Run `setupAdmin()` in console
4. Login with default credentials

### Production Deployment

Before deploying:
1. ✅ Change admin email/password
2. ✅ Implement Firestore security rules
3. ✅ Enable Firebase App Check
4. ✅ Set up monitoring/alerts
5. ✅ Configure CORS policies
6. ✅ Enable rate limiting

---

**Note**: Keep this file secure and don't commit to public repositories!
