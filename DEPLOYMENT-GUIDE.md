# MarketHub - GitHub Pages Deployment Guide

## Complete File List

Your repository should contain these HTML files:

### Core Pages
- `index.html` - Landing page
- `login.html` - User login
- `signup.html` - User registration
- `admin-login.html` - Admin/supplier login
- `config-helper.html` - Firebase configuration setup

### Seller Pages
- `seller-dashboard-full.html` - Complete seller dashboard
- `upload-product.html` - Product upload form
- `marketplace.html` - Browse products
- `orders.html` - Order management
- `checkout.html` - Purchase checkout
- `add-funds.html` - GCash deposit submission
- `withdraw.html` - Withdrawal requests

### Admin Pages
- `supplier-dashboard-full.html` - Admin analytics dashboard
- `admin-payments.html` - Payment approval panel
- `withdrawals.html` - Withdrawal approval panel

### Documentation
- `README.md` - Project overview
- `DEPLOYMENT-GUIDE.md` - This file

## Quick Start (5 minutes)

### Step 1: Create GitHub Repository

```bash
# Create a new repository on GitHub
# Name it: markethub-platform (or your choice)
```

### Step 2: Upload Files

1. Go to your repository on GitHub
2. Click "Add file" > "Upload files"
3. Drag and drop all HTML files
4. Commit changes

### Step 3: Enable GitHub Pages

1. Go to Settings > Pages
2. Source: Deploy from a branch
3. Branch: `main` / `(root)`
4. Click Save

Your site will be live at: `https://[username].github.io/[repo-name]/`

### Step 4: Configure Firebase

1. Visit your deployed site
2. You'll be redirected to `config-helper.html`
3. Enter your Firebase credentials:
   - API Key
   - Auth Domain
   - Project ID
   - Storage Bucket
   - Messaging Sender ID
   - App ID
4. Click "Save Configuration"

### Step 5: Set Up Firebase

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project
3. Enable Authentication (Email/Password)
4. Create Firestore Database
5. Enable Firebase Storage
6. Copy configuration to your site

## Firebase Setup Details

### Authentication
```
1. Firebase Console > Authentication
2. Sign-in method > Email/Password > Enable
3. Done!
```

### Firestore Database
```
1. Firebase Console > Firestore Database
2. Create database (Start in production mode)
3. Rules tab > Add security rules (see README.md)
4. Done!
```

### Storage
```
1. Firebase Console > Storage
2. Get started
3. Rules tab > Add storage rules (see README.md)
4. Done!
```

## First Admin Account

### Create Admin User
1. Visit your site and sign up normally at `/signup.html`
2. Sign in and note your user email
3. Go to Firebase Console > Firestore
4. Find your user document in `users` collection
5. Add these fields:
   ```
   isAdmin: true
   role: "supplier"
   ```
6. Now you can log in at `/admin-login.html`

## Testing Your Deployment

### Test Checklist
- [ ] Landing page loads
- [ ] Can sign up new user
- [ ] Can log in as seller
- [ ] Can access seller dashboard
- [ ] Can upload product (with image)
- [ ] Can submit GCash payment
- [ ] Admin can log in
- [ ] Admin can approve payments
- [ ] Admin can approve withdrawals

## Common Issues

### "Firebase not configured"
**Solution:** Visit `/config-helper.html` and enter your Firebase credentials

### "Access denied" on admin pages
**Solution:** Make sure your user document has `role: "supplier"` in Firestore

### Images not uploading
**Solution:** Check Firebase Storage rules are set correctly

### Payment proofs not showing
**Solution:** Verify Storage CORS settings and public read access

## Custom Domain (Optional)

1. Go to Repository Settings > Pages
2. Custom domain: `yourdomain.com`
3. Add CNAME record in your DNS:
   ```
   CNAME @ [username].github.io
   ```
4. Wait for DNS propagation (up to 24 hours)
5. Enable "Enforce HTTPS"

## File Structure

```
markethub-platform/
├── index.html                      # Landing
├── login.html                      # Auth
├── signup.html                     # Registration
├── admin-login.html                # Admin auth
├── config-helper.html              # Setup
├── seller-dashboard-full.html      # Seller main
├── supplier-dashboard-full.html    # Admin main
├── upload-product.html             # Product form
├── marketplace.html                # Shop
├── orders.html                     # Orders
├── checkout.html                   # Checkout
├── add-funds.html                  # Deposits
├── withdraw.html                   # Withdrawals
├── admin-payments.html             # Payment review
├── withdrawals.html                # Withdrawal review
├── README.md                       # Docs
└── DEPLOYMENT-GUIDE.md            # This file
```

## Security Notes

1. **Never commit Firebase credentials to public repos**
   - Use config-helper.html instead
   - Users configure their own Firebase

2. **Firestore Security Rules**
   - Set proper read/write permissions
   - Validate data types and fields
   - See README.md for complete rules

3. **Storage Rules**
   - Allow authenticated uploads only
   - Public read for product images
   - See README.md for complete rules

## Performance Optimization

1. **Enable caching** - Add to your HTML headers:
```html
<meta http-equiv="Cache-Control" content="max-age=31536000">
```

2. **Compress images** - Use tools like TinyPNG before uploading

3. **CDN usage** - Tailwind and Firebase are loaded from CDN

## Support

For issues:
1. Check Firebase Console for errors
2. Open browser DevTools > Console
3. Verify all HTML files are uploaded
4. Check GitHub Pages is enabled
5. Test Firebase configuration

## Updates

To update your site:
1. Edit HTML files locally
2. Upload to GitHub (replace existing)
3. Changes go live automatically
4. No rebuild needed!

## Backup

Regular backups:
1. Download all HTML files from GitHub
2. Export Firestore data (Firebase Console > Firestore > Import/Export)
3. Download Storage files if needed

---

Built with ❤️ for GitHub Pages deployment
