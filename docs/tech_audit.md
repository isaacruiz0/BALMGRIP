# Full-Stack Website Tech Audit: BalmGrip

## 1. **Project Overview**
E-commerce website to sell my original product, BalmGrip, designed to solve the problem of losing your chapstick by keeping it conveniently attached to your keychain.

## 2. **Frontend**
### Framework: Next.js

### Why Use Next.js?
This is an e-commerce website, so fast page loads are crucial — every second a customer waits increases the likelihood of them leaving. In addition, I need to be capable of optimizing the SEO of the website for gaining organic traffic. 

Next.js's hybrid rendering paradigm is ideal as it ensures optimal initial page loads and SEO extensibility by serving fully fleshed out HTML via server-side rendering. It also provides seamless page navigation via client-side routing which makes for a smooth User Experience.

- **Server-Side Rendering (SSR)**:
  - Ships the entire HTML with with assets such as images and videos already embedded in the markup
- **Improved SEO**:
  - By serving the HTML from the server for specific routes, this makes pages more crawlable by search engines, boosting organic visibility.
- **Built-in Image Optimization**:
  - Automatically serves images in modern formats and sizes, improving load times.

### Pages
---
### Product/Home Page
#### Frontend Components

- #### Text Fade-in-out Looper
  - Loops over a list of copy by fading it out and fading in the next item

- #### Product Image Carousels 
  - Presents a variety of Balm Grip in-use photos
  - Users can swipe through photos
  - Rotates automatically 

- #### Embedded Stripe Checkout Modal
  - Triggered by "Buy Now" button click
  - Pre-configured with:
    - Product details and pricing
    - Shipping address collection fields
    - Email collection field with marketing opt-in checkbox
  - On Submission automatic redirect to success/cancel pages
  - Form validation by Stripe
  - Secure card processing by Stripe

- #### Reviews ( Post MVP )
  - Review submission form
    - Star rating (1-5)
    - Photo upload option
    - Verified purchase badge
  - Review display component
    - Pagination

### FAQ Page
#### Frontend Components
- #### Dropdown Menus
  - Lists questions and answers

### Track order Page
#### Frontend Components
  - #### Order tracking form
    - Input field for order 
    - Email verification
    - Hits `/orders/track/:orderId` internal API endpoint to get order status
  - #### Order status display
    - Current status (Pending/Processing/Shipped/Delivered)
    - USPS tracking number with direct link
    - Shipping updates timeline
    - Estimated delivery date

### Global Components
#### Navigation
- Navigation bar and side menu
  - Logo
  - Home
  - FAQ
  - Track Order
#### Footer
  ### Privacy Policy
  - Data Collection and Usage
    - Marketing email opt-in/out policy
    - Cookie usage ()
  - Third-party Data Sharing
    - Stripe (payment processing)
    - USPS (shipping)
  - GDPR/CCPA Compliance
    - Right to access personal data
    - Right to opt out of marketing emails
  
  ### Refunds and Returns
  - Return Window
    - 30-day return policy
    - Unused condition requirement
    - Original packaging preferred
  - Return Process
    - How to initiate return
    - Return shipping instructions
    - Return tracking
  - Refund Details
    - Processing time (e.g., 5-7 business days)
    - Original payment method refund
    - Shipping cost policy

### Hooks
---
### Balmgrip & Chapstick Scroll Animation
As the user scrolls, the balmgrip and chapstick come together eventually snapping into each other

## 3. **Backend**
### Runtime: Node.js
### Framework: Express.js

### External APIs - APIs that the BE interacts with
- Stripe API Integration
  - Checkout Session
    - Use Stripe Checkout's built-in address collection and validation
    - Enable email collection and marketing consent checkbox
    - Hand off card transactions to Stripe
    - Handle abandoned checkouts (Post-MVP)
  - Post-purchase
    - Store Stripe Customer ID for future reference
    - Use Stripe's webhook to handle successful payments
    - Send order confirmation emails via Stripe
- USPS
  - Get tracking information

### Internal APIs - Interacts directly with FE
### Stripe
  - `stripe/checkout`: Middleman for initializing a Checkout Form session.
### USPS
  - `/orders/track/:orderId`
    - Validates order exists
    - Returns order status and tracking info
    - Fetches latest USPS tracking updates
### Reviews (Post-MVP)
- API Endpoints:
  - POST /reviews/create
  - GET /reviews/list
- Validation:
  - Verify purchase before allowing review
  - Filter inappropriate content
  - Rate limiting
- Image processing for uploaded photos

## Database
### Type
MySQL
### Schemas
#### Order Details table:
  - id (primary key)
  - stripe_session_id
  - stripe_customer_id
  - email
  - total_cost
  - quantity
  - created_at
  - payment_status
  - fulfillment_status
  - usps_tracking_number
  - marketing_consent_status


#### Reviews table:
  - id (primary key)
  - order_id (for purchase verification)
  - email
  - rating (1-5)
  - review_text
  - image_url
  - created_at

---

## 5. **Hosting & Deployment**
FE: Vercel
BE: Heroku
Database: PlanetScale

---

## 6. **Performance Audit**
### 6.1 Metrics
- Page Load Speed (Lighthouse, WebPageTest)
- Time to First Byte (TTFB)
- Core Web Vitals (CLS, FID, LCP)
- Asset Size Optimization (images, scripts)
- Bounce Rate ( google analytics )

### 6.2 Tools Used
- Google Lighthouse
- WebPageTest
- Chrome DevTools

## 7. **Error Handling & Monitoring**
- Payment failure recovery flow
- API error responses standardization

## 8. **Security**
- Rate limiting for all API endpoints
- CORS policy configuration
- XSS protection
- Input sanitization
- Environment variables management

---

| **Task Description**                                      | **Estimated Time** |
|-----------------------------------------------------------|--------------------|
| **Frontend Development**                                  |                    |
| Set up Next.js project with initial configuration         | 2 hours            |
| Implement Product/Home Page                               |                    |
| - Text Fade-in-out Looper                                 | 3 hours            |
| - Product Image Carousels                                 | 4 hours            |
| - Embedded Stripe Checkout Modal                         | 3 hours            |
| Implement FAQ Page with Dropdown Menus                   | 3 hours            |
| Implement Track Order Page                                |                    |
| - Order tracking form                                     | 3 hours            |
| - Order status display with USPS updates                 | 3 hours            |
| Develop Navigation Bar and Footer                        | 3 hours            |
| Implement Balmgrip & Chapstick Scroll Animation Hook          | 3 hours            |
| Style pages and components with responsive design        | 3 hours            |
| **Backend Development**                                   |                    |
| Set up Node.js and Express.js backend                    | 2 hours            |
| Implement Stripe API integration                         |                    |
| - Checkout session creation                              | 4 hours            |
| - Webhook for payment success                            | 3 hours            |
| Integrate USPS API for tracking updates                  |                    |
| - Fetch tracking information                             | 4 hours            |
| - Handle USPS API errors                                 | 2 hours            |
| Create Internal API endpoints                            |                    |
| - `/stripe/checkout` for frontend integration            | 3 hours            |
| - `/orders/track/:orderId` for tracking updates          | 4 hours            |
| **Database Development**                                  |                    |
| Set up MySQL database on PlanetScale                     | 2 hours            |
| Design and implement Order Details table schema          | 2 hours            |
| Design and implement Reviews table schema (Post-MVP)     | 2 hours            |
| **Hosting & Deployment**                                  |                    |
| Deploy frontend to Vercel                                | 1 hour             |
| Deploy backend to Heroku                                 | 1 hour             |
| Connect backend to PlanetScale database                  | 1 hour             |
| **Performance Optimization**                              |                    |
| Set up caching for API responses                         | 3 hours            |
| Conduct Lighthouse and WebPageTest audits                | 2 hours            |
| **Security Implementation**                               |                    |
| Set up CORS policy                                        | 1 hour             |
| Add XSS protection and input sanitization                | 2 hours            |
| Configure rate limiting for API endpoints                | 2 hours            |
| Secure environment variables                             | 1 hour             |
| **Error Handling & Monitoring**                           |                    |
| Implement API error handling with consistent responses   | 3 hours            |
| Add payment failure recovery flow                        | 2 hours            |
| **Total Time**                                            | **77 hours**       |