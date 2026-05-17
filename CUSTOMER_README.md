# Customer Ticket Reselling Platform

## 🎫 Overview

This is the **Customer-Facing** page for the Ticketmaster ticket reselling platform. Users can:
- 🔍 Browse available events
- 🎟️ View ticket details (price, availability, location)
- 💳 Purchase tickets securely
- 📱 Responsive mobile experience

The events are **dynamically loaded** from the admin panel via GitHub.

---

## 🚀 Quick Start

### Local Development

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/ticketmaster-customer.git
cd ticketmaster-customer

# 2. Start local server
python -m http.server 8001

# 3. Open in browser
# Visit http://localhost:8001/resell.html
```

### Vercel Deployment

```bash
# Deploy to Vercel
vercel

# Live at
# https://ticketmaster-customer.vercel.app
```

---

## 📋 Features

### Browse Events
- Display all available events
- Filter by category
- Search functionality
- Infinite scroll

### Event Cards
Each event shows:
- Event image
- Event title
- Location
- Date & time
- Ticket price
- Available tickets count
- "Buy Now" button

### Ticket Purchase Modal
- Event details popup
- Quantity selector (1-8 tickets)
- Seat selection
- Price breakdown
- Email input for receiver
- Message seller option
- Secure payment

### Payment
- Stripe integration
- Credit card payment
- Transaction confirmation

---

## 🔄 How Events Load

### Data Flow

```
1. Page loads (resell.html)
   ↓
2. script.js runs loadDynamicEvents()
   ↓
3. Check localStorage for cached events
   ↓
4. If no cache, load from events.json
   ↓
5. Render event cards dynamically
   ↓
6. User sees live events
```

### Event Sources (Priority)

1. **LocalStorage** - Fast (browser cache)
2. **events.json** - Fallback (bundled)
3. **GitHub API** - Optional (real-time)

---

## 📁 File Structure

```
ticketmaster-customer/
├── resell.html              # Main customer page
├── script.js                # Event loader & interactions
├── index.html               # Homepage
├── cancel.html              # Payment cancelled page
├── success.html             # Payment success page
├── style.css                # Styles
├── styles.css               # Additional styles
├── events.json              # Events data (fallback)
├── assets/                  # Images and media
├── vercel.json              # Vercel config
├── package.json             # Project metadata
├── .gitignore               # Git ignore rules
└── README.md                # This file
```

---

## 💾 Event Data Format

Events load in this format:

```json
{
  "events": [
    {
      "id": 1,
      "title": "Fred Again at Bancomer",
      "location": "México, CDMX, Expo Santa Fe",
      "dateTime": "Fri • Dec 12, 2025 • 8:00 PM",
      "price": 200,
      "ticketsAvailable": 4,
      "imageUrl": "Fred.jpg",
      "imageAlt": "Fred Again at Bancomer",
      "category": "Music"
    }
  ]
}
```

---

## 🎯 User Journey

### 1. Browse Events
User visits `resell.html` → Events load dynamically

### 2. Select Event
User clicks "Buy Now" on any event card

### 3. Loading Animation
3-second spinner animation → Sets mood

### 4. Purchase Modal
Modal shows:
- Event title
- Price per ticket
- Quantity selector
- Seat information
- Email input
- Message seller button
- Pay Now button

### 5. Quantity Selection
User can select 1-8 tickets
- Total price updates in real-time
- Seats are assigned automatically

### 6. Payment
User enters email and clicks "Pay Now"
- Stripe payment processing
- Success/cancel pages

---

## 🔧 How to Integrate Admin Events

### Option 1: Automatic Loading (Recommended)

Events load automatically from:
1. Browser localStorage (if admin synced locally)
2. events.json (bundled fallback)

No additional config needed!

### Option 2: Manual GitHub Pull

In `script.js`, add GitHub API call:

```javascript
async function pullFromGitHub() {
  const owner = 'YOUR_USERNAME';
  const repo = 'ticketmaster-admin';
  const response = await fetch(
    `https://api.github.com/repos/${owner}/${repo}/contents/events.json`
  );
  return response.json();
}
```

### Option 3: Webhook Integration

Admin panel can POST to customer API:
```javascript
// When admin syncs events
await fetch('https://customer-api.vercel.app/events', {
  method: 'POST',
  body: JSON.stringify(events)
});
```

---

## 🛒 Purchase Flow

### Quantity Selector
- Minus button: Decrease quantity
- Plus button: Increase quantity
- Input: Shows current count
- Max: 8 tickets per transaction

### Seat Assignment
```
1 ticket:  Row A, Seat 68
2 tickets: Row A, Seats 68, 69
3 tickets: Row A, Seats 68, 69, 70
...
```

### Price Calculation
```
Total = Price per Ticket × Quantity
Example: $200 × 2 = $400
```

---

## 💳 Payment Integration

### Stripe Configuration

1. Add Stripe public key to HTML:
```html
<script src="https://js.stripe.com/v3/"></script>
```

2. Initialize Stripe in script:
```javascript
const stripe = Stripe('pk_live_YOUR_KEY');
```

3. Handle payment:
```javascript
const result = await stripe.confirmCardPayment(clientSecret);
```

---

## 🎨 Customization

### Change Event Categories

Edit categories in `events.json`:
```json
"category": "Music"  // Change to: Football, Festival, etc.
```

### Customize Modal Colors

Edit `script.js`:
```javascript
cardModalContent.style.cssText = `
  background: linear-gradient(135deg, #0000FF 0%, #00FFFF 100%);
  ...
`;
```

### Update Ticket Limits

Edit `script.js`:
```javascript
max: 8  // Change to 10, 15, etc.
```

---

## 📱 Responsive Design

### Breakpoints
- Desktop: 1200px+
- Tablet: 768px - 1199px
- Mobile: < 768px

### Mobile Optimizations
- Single column layout
- Touch-friendly buttons
- Full-width modals
- Readable fonts

---

## 🔐 Security

### Data Privacy
- No personal info stored on client
- Email only used for confirmation
- HTTPS only (Vercel auto-enabled)
- Stripe handles payment security

### Best Practices
1. Use HTTPS everywhere
2. Validate input on client & server
3. Never log sensitive data
4. Rotate API keys regularly

---

## 🐛 Troubleshooting

### Events Not Loading

**Problem**: Blank page or "No events" message

**Solution**:
1. Open browser console (F12)
2. Check for JavaScript errors
3. Verify `events.json` exists
4. Check file paths (case-sensitive)
5. Ensure JSON is valid

### Modal Not Opening

**Problem**: "Buy Now" button doesn't work

**Solution**:
1. Check console for errors
2. Verify script.js is loaded
3. Check event listener setup
4. Test with different browser

### Styling Issues

**Problem**: Layout broken or colors wrong

**Solution**:
1. Clear cache (Ctrl+Shift+Delete)
2. Check CSS file paths
3. Verify CSS is loaded (F12 → Network)
4. Check for conflicting styles

### Images Not Showing

**Problem**: Event images not loading

**Solution**:
1. Verify image paths in `events.json`
2. Check file names (case-sensitive)
3. Ensure images are in `assets/` folder
4. Test image URLs directly in browser

---

## 📊 Monitoring

### Check if Events Load
1. Open page
2. Press F12 (Developer Tools)
3. Go to Console tab
4. Type: `localStorage.getItem('events')`
5. Should show event data

### Check Network
1. Open Network tab (F12)
2. Reload page
3. Look for `events.json` request
4. Should show 200 status

---

## 🚀 Deployment Checklist

- [ ] Clone from GitHub
- [ ] All files present
- [ ] `events.json` configured
- [ ] Images in `assets/` folder
- [ ] script.js loaded correctly
- [ ] Stripe keys configured
- [ ] vercel.json configured
- [ ] Deploy to Vercel
- [ ] Test on mobile
- [ ] Test event loading
- [ ] Test "Buy Now" flow

---

## 🔗 Related Repos

- **Admin Panel**: https://github.com/YOUR_USERNAME/ticketmaster-admin
- **Customer Page**: https://github.com/YOUR_USERNAME/ticketmaster-customer

---

## 📞 Support

For issues:
1. Check browser console (F12)
2. Verify `events.json` is valid JSON
3. Test image paths
4. Check Vercel deployment logs
5. Review script errors

---

## 📝 Changelog

### v1.0.0 - Initial Release
- ✅ Dynamic event loading
- ✅ Purchase modal
- ✅ Quantity selector
- ✅ Responsive design
- ✅ Stripe integration
- ✅ Mobile optimized

---

**Created**: May 2026  
**Version**: 1.0.0  
**Status**: Production Ready ✅
