# MarketHub - Standalone HTML Version

A complete e-commerce marketplace that runs directly in the browser using Firebase backend services.

## Features

- User authentication (signup/login)
- Seller dashboard with product management
- **Image upload** for product photos (multiple images per product)
- **Document upload** for product specifications, certificates, and other files
- Firebase Storage integration for file hosting
- Real-time product listing
- Admin payment approval system
- Withdrawal management
- Binary tree commission system
- Referral links with position selection
- Responsive design with Tailwind CSS
- No build process required - runs directly from HTML files

## Setup Instructions

### 1. Deploy to GitHub Pages

1. Create a new GitHub repository
2. Upload all HTML files to the repository
3. Go to Settings → Pages
4. Select "Deploy from a branch" and choose your main branch
5. Save and wait for deployment

### 2. Configure Firebase

1. Visit `config-helper.html` on your deployed site
2. Follow the instructions to get your Firebase credentials:
   - Go to [Firebase Console](https://console.firebase.google.com/)
   - Create a new project or select existing one
   - Enable **Authentication** (Email/Password)
   - Create a **Firestore Database**
   - Enable **Firebase Storage** (required for image/document uploads)
   - Get your web app credentials from Project Settings
3. Enter the credentials in the config helper
4. Click "Save Configuration"

### 3. Firebase Storage Rules

In Firebase Console → Storage → Rules, use these rules:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /products/{userId}/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    match /documents/{userId}/{allPaths=**} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    match /payment-proofs/{userId}/{allPaths=**} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

### 4. Firestore Security Rules

In Firebase Console → Firestore → Rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId || get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
    
    match /products/{productId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    
    match /payments/{paymentId} {
      allow read: if request.auth != null;
      allow create: if request.auth.uid == request.resource.data.userId;
      allow update: if get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role in ['admin', 'supplier'];
    }
    
    match /withdrawals/{withdrawalId} {
      allow read: if request.auth != null;
      allow create: if request.auth.uid == request.resource.data.userId;
      allow update: if get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role in ['admin', 'supplier'];
    }
    
    match /orders/{orderId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

## File Structure

```
├── index.html              # Landing page
├── login.html              # Login page
├── signup.html             # Registration page
├── seller-dashboard.html   # Seller dashboard with upload features
├── admin-payments.html     # Admin payment approval & code generation
├── withdrawals.html        # Admin withdrawal management
├── withdraw.html           # User withdrawal request
├── add-funds.html          # GCash deposit with proof upload
├── config-helper.html      # Firebase configuration setup
└── README.md              # This file
```

## Features in Seller Dashboard

### Product Images
- Drag and drop multiple images
- Preview before upload
- Automatic upload to Firebase Storage
- Public URLs generated for each image

### Product Documents
- Upload PDFs, Word docs, Excel files, text files
- Up to 10MB per file
- Secure storage in Firebase
- Download links stored with product

### Product Information
- Name, price, category
- Stock quantity
- Detailed description
- All data stored in Firestore

## Payment System Features

### For Sellers
- **Add Funds (add-funds.html)**: Upload GCash payment proof with reference number
- **Withdraw (withdraw.html)**: Request withdrawal to GCash account
- **Transaction History**: View all deposits and withdrawals with status

### For Admins
- **Payment Review (admin-payments.html)**: 
  - View pending GCash deposits
  - Approve or reject payments
  - Generate activation codes for binary tree placement
- **Withdrawal Management (withdrawals.html)**:
  - Review withdrawal requests
  - Confirm and process payments
  - Reject with reason (auto-refund to wallet)

### Commission System
- Binary tree structure with positions A and B
- 10% matching commission when both downlines upload products
- Automatic commission calculation and wallet crediting
- Referral links with position selection

## Usage

### For Sellers
1. Open `index.html` (or deployed URL)
2. Sign up and login
3. Add funds via GCash (add-funds.html)
4. Wait for admin approval
5. Upload products with images and documents
6. Manage orders and earnings
7. Request withdrawals when ready

### For Admins
1. Login with admin account
2. Visit admin-payments.html to review deposits
3. Approve payments and generate activation codes
4. Visit withdrawals.html to process withdrawal requests
5. Monitor user activities and transactions

## Technologies Used

- HTML5
- Tailwind CSS (CDN)
- Firebase 10.8.0 (Authentication, Firestore, Storage)
- Vanilla JavaScript (ES6 modules)

## Browser Compatibility

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Notes

- Firebase configuration is stored in browser localStorage
- All files are uploaded to Firebase Storage with public URLs
- Images are limited to 5MB each
- Documents are limited to 10MB each
- Payment proofs stored securely in Firebase Storage
- No server or build process required
- Works entirely client-side

## Admin Setup

To create an admin account:

1. Sign up normally through signup.html
2. Open browser console (F12)
3. Run this code (replace USER_ID with your user ID from Firebase):

```javascript
firebase.firestore().collection('users').doc('USER_ID').update({
  role: 'admin'
});
```

Or manually edit the user document in Firebase Console → Firestore.

## Support

For issues with Firebase setup, refer to the official [Firebase Documentation](https://firebase.google.com/docs).
