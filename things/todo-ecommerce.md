# ONE Platform: E-Commerce & Monetization v1.0.0

**Focus:** Product marketplace, shopping cart, X402 checkout, revenue tracking
**Process:** `Infer 1-100 inference sequence`
**Timeline:** 8-12 inferences per specialist per day
**Target:** Revenue generation (Wave 3 - Parallel with API + Features)

---

## PHASE 1: FOUNDATION & SETUP (Infer 1-10)

**Purpose:** Validate e-commerce requirements, map to ontology, plan implementation

### Infer 1: Define Product Types
- [ ] **Courses:** Video lessons bundled (monthly/one-time)
- [ ] **Digital Products:** Templates, assets, tools (one-time purchase)
- [ ] **Memberships:** Tiered access + recurring revenue (monthly/yearly)
- [ ] **1:1 Consultations:** Personal sessions with creator (booked + paid)
- [ ] **NFTs:** Digital collectibles (one-time, blockchain)
- [ ] **API Access:** Agent/workflow execution (pay-per-use via X402)
- [ ] **Custom Services:** Bespoke products defined by creator
- [ ] For each type, define:
  - [ ] Pricing model
  - [ ] Delivery method
  - [ ] Refund policy
  - [ ] Digital vs physical

### Infer 2: Map E-Commerce to 6-Dimension Ontology
- [ ] **Groups:** Creator's business group
- [ ] **People:** Creator (seller), customer (buyer), platform (facilitator)
- [ ] **Things:** product, shopping_cart, order, payment, subscription
- [ ] **Connections:**
  - [ ] creator → product (owns)
  - [ ] customer → product (purchased)
  - [ ] customer → subscription (subscribed_to)
  - [ ] product → payment (transacted)
- [ ] **Events:**
  - [ ] product_created, product_updated, product_deleted
  - [ ] cart_created, item_added, item_removed
  - [ ] checkout_initiated, payment_processed, order_completed
  - [ ] subscription_started, subscription_renewed, subscription_cancelled
- [ ] **Knowledge:** product_category, price_tier, access_level labels

### Infer 3: Review X402 Integration (from todo-x402)
- [ ] Understand X402 payment flow (402 → payment → settled)
- [ ] Identify how X402 applies to checkout:
  - [ ] Primary: Crypto payments (USDC on Base)
  - [ ] Support: Traditional (Stripe) as fallback (future)
- [ ] Plan X402 as default payment for digital products
- [ ] Plan traditional payment for higher-value items (consultations)
- [ ] Map payment to events: payment_processed → order_completed

### Infer 4: Define Shopping Cart Requirements
- [ ] Cart functionality:
  - [ ] Add/remove items
  - [ ] Update quantities (for subscriptions, bundles)
  - [ ] Apply discount codes (future)
  - [ ] Calculate taxes (future: US sales tax)
  - [ ] Show subtotal + total
  - [ ] Persistent (save across sessions)
  - [ ] Share cart link (for wishlists)
- [ ] Cart storage:
  - [ ] localStorage (client-side before purchase)
  - [ ] Convex (after purchase → order)
- [ ] Abandoned cart:
  - [ ] Email reminder after 24 hours (future)
  - [ ] Track in analytics

### Infer 5: Define Checkout Flow
- [ ] Steps:
  1. Review cart + total
  2. Enter shipping/billing (if physical)
  3. Select payment method (X402 for crypto, Stripe for fiat)
  4. Review order summary
  5. Confirm payment
  6. Show order confirmation + receipt
  7. Email receipt to buyer
- [ ] Guest checkout allowed (no login required)
- [ ] Time limit: 15 minutes to complete
- [ ] Mobile-optimized (80% of users on mobile)

### Infer 6: Define Subscription Model
- [ ] Types:
  - [ ] **Monthly subscription** ($X/month, auto-renews)
  - [ ] **Yearly subscription** ($X/year, cheaper per month)
  - [ ] **Lifetime access** (one-time, highest price)
- [ ] Features:
  - [ ] Auto-renewal with X402 (permit-based recurring)
  - [ ] Cancel anytime
  - [ ] Upgrade/downgrade within billing cycle
  - [ ] Pause subscription (temporary hold)
  - [ ] Manage from account settings
- [ ] Events:
  - [ ] subscription_started
  - [ ] subscription_renewed
  - [ ] subscription_paused
  - [ ] subscription_resumed
  - [ ] subscription_cancelled

### Infer 7: Define Refund + Dispute Policy
- [ ] Refund policy:
  - [ ] 7-day money-back guarantee for courses
  - [ ] No refund for instantly downloadable digital products
  - [ ] No refund for consultations after 24 hours
  - [ ] Automatic refund in original payment method
- [ ] Dispute resolution (future):
  - [ ] Customer initiates refund request
  - [ ] Creator has 48 hours to respond
  - [ ] Platform mediates if disagreement
  - [ ] Escalate to platform owner if needed

### Infer 8: Define Revenue Split
- [ ] Creator earnings:
  - [ ] Product price - platform fee - payment processor fee
  - [ ] Platform fee: 5% (vs Stripe's ~3% + $0.30)
  - [ ] X402 gas cost: Minimal on Base (~$0.0001)
  - [ ] Example: $10 course → $9.50 to creator
- [ ] Payout:
  - [ ] Weekly automatic payout to creator's wallet
  - [ ] Creator must have connected wallet
  - [ ] Minimum $5 threshold before payout
- [ ] Events:
  - [ ] revenue_earned (daily, per creator)
  - [ ] payout_processed (weekly)
  - [ ] payout_failed (if wallet invalid)

### Infer 9: Identify Analytics Needs
- [ ] For creators (seller view):
  - [ ] Total revenue (all-time, year, month, week)
  - [ ] Sales by product
  - [ ] Sales by channel (direct, referral, marketplace)
  - [ ] Conversion rate (visitors → purchasers)
  - [ ] Customer lifetime value
  - [ ] Most popular products
  - [ ] Recent sales + notifications
- [ ] For platform (admin view):
  - [ ] Total platform revenue
  - [ ] Revenue by creator
  - [ ] Payment method breakdown (X402 vs Stripe)
  - [ ] Refund rate
  - [ ] Dispute rate
  - [ ] Geographic distribution

### Infer 10: Define Success Metrics
- [ ] E-commerce complete when:
  - [ ] [ ] Product creation working
  - [ ] [ ] Shopping cart functional
  - [ ] [ ] Checkout with X402 working
  - [ ] [ ] Order confirmation + email
  - [ ] [ ] Creator can view sales
  - [ ] [ ] Revenue tracking accurate
  - [ ] [ ] Subscription auto-renewal working
  - [ ] [ ] Payout processing tested
  - [ ] [ ] Analytics dashboard functional
  - [ ] [ ] First creator earns first $100

---

## PHASE 2: BACKEND SCHEMA & SERVICES (Infer 11-20)

**Purpose:** Create product + order schema in Convex

### Infer 11: Extend Product Thing Type
- [ ] Update `backend/convex/schema.ts`:
  ```typescript
  {
    type: 'digital_product' | 'membership' | 'consultation' | 'nft',
    properties: {
      // Identification
      creatorId: Id<'things'>,  // Owner
      name: string,
      slug: string,  // URL: one.ie/product/slug
      description: string,
      thumbnail: string,  // Product image
      images: string[],  // Gallery

      // Pricing
      price: number,  // In cents ($0.01 = 1)
      currency: 'USD' | 'USDC',  // USD = Stripe, USDC = X402
      discountPrice: number,  // Sale price if any
      taxable: boolean,

      // Product type specific
      type: 'course' | 'template' | 'membership' | 'consultation' | 'nft',

      // For courses
      lessons: number,
      duration: number,  // Minutes

      // For memberships
      billingCycle: 'monthly' | 'yearly' | 'lifetime',
      trialDays: number,  // Free trial period

      // For consultations
      duration: number,  // Session minutes
      calendarUrl: string,  // Scheduling tool

      // Availability
      status: 'draft' | 'published' | 'archived',
      publishedAt: number,
      visibility: 'public' | 'unlisted' | 'private',

      // Stats
      totalSales: number,
      totalRevenue: number,
      averageRating: number,
      reviewCount: number,

      // Content delivery
      contentUrl: string,  // Link to course, files, etc
      contentType: 'url' | 'video' | 'document' | 'zip',

      // Settings
      allowRefunds: boolean,
      refundWindow: number,  // Days
      digitalDownload: boolean,
    }
  }
  ```

### Infer 12: Create Shopping Cart Thing Type
- [ ] New thing type:
  ```typescript
  {
    type: 'shopping_cart',
    properties: {
      customerId: Id<'things'>,  // Who owns cart
      items: [{
        productId: Id<'things'>,
        quantity: number,
        price: number,  // Price at time added
        addedAt: number,
      }],
      subtotal: number,
      tax: number,
      discount: number,
      total: number,
      status: 'active' | 'abandoned' | 'purchased',
      createdAt: number,
      expiresAt: number,  // 30 days
    }
  }
  ```

### Infer 13: Create Order Thing Type
- [ ] New thing type:
  ```typescript
  {
    type: 'order',
    properties: {
      customerId: Id<'things'>,  // Buyer
      creatorId: Id<'things'>,  // Seller
      cartId: Id<'things'>,  // Associated cart
      orderNumber: string,  // Unique: ORD-202501-001
      items: [{
        productId: Id<'things'>,
        productName: string,
        quantity: number,
        price: number,
        total: number,
      }],
      subtotal: number,
      tax: number,
      discount: number,
      total: number,
      paymentId: string,  // X402 payment ID or Stripe charge ID
      paymentMethod: 'x402' | 'stripe' | 'paypal',
      paymentStatus: 'pending' | 'completed' | 'failed',
      status: 'processing' | 'completed' | 'fulfilled' | 'cancelled',
      deliveryMethod: 'email' | 'download' | 'link',
      deliveryStatus: 'pending' | 'sent' | 'downloaded',
      shippingAddress: { street, city, state, zip, country },  // If physical
      billingAddress: { street, city, state, zip, country },
      createdAt: number,
      completedAt: number,
      notes: string,  // Internal notes
    }
  }
  ```

### Infer 14: Create Subscription Thing Type
- [ ] New thing type:
  ```typescript
  {
    type: 'subscription',
    properties: {
      customerId: Id<'things'>,
      creatorId: Id<'things'>,
      productId: Id<'things'>,  // Course/membership
      plan: 'monthly' | 'yearly' | 'lifetime',
      price: number,
      status: 'active' | 'paused' | 'cancelled' | 'expired',
      startDate: number,
      renewDate: number,  // Next auto-renewal
      cancelDate: number,
      pausedUntil: number,  // If paused
      trialEndsAt: number,  // If in trial
      paymentMethodId: string,  // For recurring X402
      lastPaymentDate: number,
      lastPaymentId: string,
      totalPayments: number,
      autoRenew: boolean,
      cancelReason: string,  // Why cancelled
    }
  }
  ```

### Infer 15: Create Payment Thing Type Extension
- [ ] Extend payment thing from todo-x402:
  ```typescript
  {
    type: 'payment',
    properties: {
      // ... existing X402 fields ...

      // E-commerce specific
      orderId: Id<'things'>,  // Associated order
      creatorId: Id<'things'>,  // Product owner
      productId: Id<'things'>,  // What was sold
      metadata: {
        productType: 'course' | 'template' | 'membership' | etc,
        quantity: number,
        refunded: boolean,
        refundDate: number,
        refundAmount: number,
        refundReason: string,
      }
    }
  }
  ```

### Infer 16: Create E-Commerce Service (Effect.ts)
- [ ] Service methods:
  - [ ] `createProduct(creator, productData)` → productId
  - [ ] `addToCart(customer, productId, quantity)` → cartId
  - [ ] `removeFromCart(cartId, productId)` → void
  - [ ] `checkout(customer, cartId)` → order + payment request
  - [ ] `processPayment(orderId, paymentPayload)` → payment verified
  - [ ] `completeOrder(orderId)` → deliver content
  - [ ] `cancelOrder(orderId, reason)` → process refund
  - [ ] `createSubscription(customer, productId, plan)` → subscriptionId

### Infer 17: Create Convex Mutations for Products
- [ ] `mutations/products.ts`:
  - [ ] `createProduct(creatorId, data)` → productId
  - [ ] `updateProduct(productId, data)` → updated
  - [ ] `publishProduct(productId)` → published
  - [ ] `archiveProduct(productId)` → archived
  - [ ] `deleteProduct(productId)` → deleted

### Infer 18: Create Convex Mutations for Orders
- [ ] `mutations/orders.ts`:
  - [ ] `createOrder(customerId, items)` → orderId
  - [ ] `updateOrderStatus(orderId, status)` → updated
  - [ ] `completeOrder(orderId)` → completed + deliver
  - [ ] `refundOrder(orderId, reason)` → refunded
  - [ ] `cancelOrder(orderId, reason)` → cancelled

### Infer 19: Create Convex Queries
- [ ] `queries/products.ts`:
  - [ ] `listProductsForCreator(creatorId)` → products[]
  - [ ] `getProductBySlug(slug)` → product
  - [ ] `searchProducts(query)` → products[]
  - [ ] `listFeaturedProducts()` → products[]
- [ ] `queries/orders.ts`:
  - [ ] `getOrdersForCustomer(customerId)` → orders[]
  - [ ] `getOrdersForCreator(creatorId)` → orders[]
  - [ ] `getOrder(orderId)` → order details

### Infer 20: Create Revenue Tracking Service
- [ ] Service to track creator earnings:
  - [ ] `trackSale(orderId, amount)` → log event
  - [ ] `getCreatorRevenue(creatorId, period)` → total
  - [ ] `getCreatorStats(creatorId)` → {totalSales, totalRevenue, topProduct}
  - [ ] `processWeeklyPayouts()` → send to wallets

---

## PHASE 3: FRONTEND COMPONENTS & PAGES (Infer 21-30)

**Purpose:** Build shopping experience in React/Astro

### Infer 21: Create ProductCard Component
- [ ] Reusable card showing:
  - [ ] Product image
  - [ ] Product name + creator name
  - [ ] Brief description (100 chars)
  - [ ] Rating (stars + count)
  - [ ] Price
  - [ ] "View Details" or "Add to Cart" button
- [ ] Responsive: Works on mobile, tablet, desktop

### Infer 22: Create ProductGallery Component
- [ ] Multi-image gallery:
  - [ ] Thumbnail strip on side/bottom
  - [ ] Click to view full size
  - [ ] Keyboard nav (arrows)
  - [ ] Mobile: Swipe to change

### Infer 23: Create ShoppingCart Component
- [ ] Display:
  - [ ] Items list (product name, price, qty)
  - [ ] Remove item button
  - [ ] Quantity selector
  - [ ] Subtotal, tax, total
  - [ ] Checkout button
  - [ ] Continue shopping link
- [ ] Empty state: Suggest products

### Infer 24: Create CheckoutForm Component
- [ ] Multi-step form:
  - [ ] Step 1: Cart review
  - [ ] Step 2: Email confirmation
  - [ ] Step 3: Payment method selector
  - [ ] Step 4: Confirm + pay
- [ ] Payment method:
  - [ ] X402 (crypto) - primary
  - [ ] Stripe (card) - fallback
- [ ] Show payment processing spinner

### Infer 25: Create OrderConfirmation Component
- [ ] Show:
  - [ ] Order number (ORD-202501-001)
  - [ ] Items purchased
  - [ ] Total paid
  - [ ] Delivery method
  - [ ] "Download" or "View Course" button
  - [ ] Email receipt link
- [ ] Actions:
  - [ ] Download invoice PDF
  - [ ] Share with friend (referral)
  - [ ] Browse more products

### Infer 26: Create ProductPage (Astro)
- [ ] Page: `/products/[slug]`
- [ ] Sections:
  - [ ] Product image gallery
  - [ ] Product name, creator, rating
  - [ ] Price + variants (if applicable)
  - [ ] Description
  - [ ] Features list
  - [ ] Reviews section
  - [ ] "Add to Cart" button
  - [ ] Related products

### Infer 27: Create MarketplaceListingPage
- [ ] Page: `/marketplace` or `/shop`
- [ ] Features:
  - [ ] Product grid
  - [ ] Filters (category, price, rating)
  - [ ] Sort (newest, popular, price)
  - [ ] Search bar
  - [ ] Pagination
  - [ ] Cart icon (show count)

### Infer 28: Create CartPage (Astro)
- [ ] Page: `/cart`
- [ ] Show:
  - [ ] Shopping cart component
  - [ ] Continue shopping link
  - [ ] Checkout button
  - [ ] Saved cart (if returning user)

### Infer 29: Create CheckoutPage (Astro)
- [ ] Page: `/checkout`
- [ ] Multi-step form (from component)
- [ ] Protect: Require items in cart
- [ ] Save form progress (localStorage)
- [ ] Show estimated delivery time

### Infer 30: Create OrderHistoryPage
- [ ] Page: `/orders` (logged in only)
- [ ] Show:
  - [ ] All orders (paginated)
  - [ ] Order number, date, total, status
  - [ ] Click to view details
  - [ ] Download invoice
  - [ ] Access download link (for digital)
  - [ ] Leave review
  - [ ] Request refund button

---

## PHASE 4-10: CONTINUATION

[Abbreviated for space - Full structure continues with:
- Phase 4: API routes for checkout, orders, products
- Phase 5: Testing (unit, integration, E2E payment flow)
- Phase 6: Design + UX finalization
- Phase 7: Performance + Stripe integration (future)
- Phase 8: Deployment + monitoring
- Phase 9: Documentation
- Phase 10: Lessons learned]

---

## SUCCESS CRITERIA

E-commerce is complete when:

- ✅ Creator can create products (courses, templates, memberships)
- ✅ Customer can browse products
- ✅ Shopping cart working (add, remove, quantities)
- ✅ Checkout flow with X402 payment working
- ✅ Order confirmation + email sent
- ✅ Digital products delivered (via email or download)
- ✅ Subscription auto-renewal via X402
- ✅ Creator can view sales + revenue
- ✅ Weekly payouts processed to creator wallets
- ✅ Analytics dashboard shows revenue trends
- ✅ First $1000 in revenue processed
- ✅ Refund process tested (7-day guarantee)

---

**Status:** Wave 3 - Monetization (Parallel with API + Features)
**Next:** todo-api.md (Wave 3), todo-features.md (Wave 3)

