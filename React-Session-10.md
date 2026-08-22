# الجلسة العاشرة: Final Project — Food Ordering Application

## 📋 نظرة عامة
- **المدة**: 3 ساعات (180 دقيقة)
- **المستوى**: Advanced — Production Ready
- **النوع**: Final Project عملي
- **التقنيات**: React + TypeScript + Vite + React Router + Zustand + React Query + Axios

---

## ⏰ تقسيم الجلسة بالدقائق

### Part 1 — Architecture + Setup + Routing (60 دقيقة)
- **0-10 دقيقة**: Requirements & User Stories
- **10-15 دقيقة**: Pages & Components Planning
- **15-20 دقيقة**: API Endpoints & Data Models
- **20-25 دقيقة**: State Management Strategy
- **25-30 دقيقة**: Tech Stack Justification
- **30-40 دقيقة**: Project Setup & Configuration
- **40-50 دقيقة**: Folder Structure & Routing
- **50-60 دقيقة**: Layout & Navigation

### Part 2 — API + Authentication + Products + Cart (60 دقيقة)
- **60-70 دقيقة**: API Services & React Query Setup
- **70-80 دقيقة**: Authentication Implementation
- **80-90 دقيقة**: Home Page & Categories
- **90-100 دقيقة**: Meals List with Search & Filter
- **100-110 دقيقة**: Meal Details Page
- **110-120 دقيقة**: Cart Implementation

### Part 3 — Checkout + Orders + Refactoring + Performance (60 دقيقة)
- **120-130 دقيقة**: Checkout Implementation
- **130-140 دقيقة**: Orders Management
- **135-140 دقيقة**: Profile Page
- **140-150 دقيقة**: Error Handling & Loading States
- **150-155 دقيقة**: Performance Optimization
- **155-160 دقيقة**: Code Review & Refactoring
- **160-180 دقيقة**: Final Checklists & Deployment

---

## 🎯 Project Overview

### المشروع: Food Ordering Application

تطبيق طلب طعام كامل يشبه UberEats أو DoorDash، يتيح للمستخدمين:
- تصفح المطاعم والوجبات
- البحث والتصفية
- إضافة للسلة
- إتمام الطلب
- تتبع الطلبات
- إدارة الملف الشخصي

---

## 📝 Requirements

### Functional Requirements

#### 1. Authentication
- ✅ المستخدم يمكنه تسجيل الدخول
- ✅ المستخدم يمكنه تسجيل الخروج
- ✅ حماية الصفحات المحمية
- ✅ عرض معلومات المستخدم

#### 2. Home Page
- ✅ Header مع navigation
- ✅ Hero section
- ✅ عرض التصنيفات
- ✅ عرض الوجبات الشائعة

#### 3. Meals
- ✅ قائمة الوجبات
- ✅ البحث بالاسم
- ✅ التصفية حسب التصنيف
- ✅ Pagination
- ✅ Loading state
- ✅ Error state
- ✅ Empty state

#### 4. Meal Details
- ✅ صورة الوجبة
- ✅ الاسم والوصف
- ✅ السعر
- ✅ التحكم بالكمية
- ✅ إضافة للسلة

#### 5. Cart
- ✅ عرض العناصر
- ✅ تعديل الكمية
- ✅ حذف العناصر
- ✅ حساب المجموع
- ✅ مسح السلة

#### 6. Checkout
- ✅ معلومات العميل
- ✅ العنوان
- ✅ ملخص الطلب
- ✅ إرسال الطلب

#### 7. Orders
- ✅ قائمة الطلبات
- ✅ تفاصيل الطلب
- ✅ حالة الطلب

#### 8. Profile
- ✅ معلومات المستخدم
- ✅ تحديث الملف الشخصي

### Non-Functional Requirements

#### Performance
- ⚡ Initial load < 3 seconds
- ⚡ API response < 500ms
- ⚡ Smooth animations (60fps)
- ⚡ Bundle size < 500KB (gzipped)

#### Security
- 🔒 HTTPS في production
- 🔒 JWT tokens for authentication
- 🔒 Input validation
- 🔒 XSS prevention
- 🔒 CORS configured

#### Accessibility
- ♿ WCAG AA compliant
- ♿ Keyboard navigation
- ♿ Screen reader compatible
- ♿ Color contrast

#### Responsiveness
- 📱 Mobile-first design
- 📱 Responsive breakpoints
- 📱 Touch-friendly UI

---

## 📖 User Stories

### Epic 1: Authentication

**US-1: Login**
```
بصفتي مستخدم
أريد أن أسجل الدخول
لكي أتمكن من طلب الطعام
```

**Acceptance Criteria:**
- يمكن للمستخدم إدخال email و password
- يتم التحقق من البيانات
- عند النجاح، يتم حفظ token
- يتم توجيه المستخدم للصفحة الرئيسية
- عند الفشل، يتم عرض رسالة خطأ

**US-2: Logout**
```
بصفتي مستخدم مسجل
أريد أن أسجل الخروج
لكي أحمي معلوماتي
```

**Acceptance Criteria:**
- يمكن للمستخدم تسجيل الخروج
- يتم مسح token
- يتم توجيه المستخدم لصفحة login

### Epic 2: Browse Meals

**US-3: View Categories**
```
بصفتي مستخدم
أريد أن أرى التصنيفات
لكي أجد نوع الطعام المطلوب
```

**Acceptance Criteria:**
- عرض جميع التصنيفات
- يمكن للمستخدم اختيار تصنيف
- يتم تصفية الوجبات حسب التصنيف

**US-4: Search Meals**
```
بصفتي مستخدم
أريد أن أبحث عن وجبة
لكي أجد ما أريده بسرعة
```

**Acceptance Criteria:**
- يمكن للمستخدم إدخال نص للبحث
- يتم تصفية النتائج أثناء الكتابة
- عرض النتائج المطابقة

**US-5: View Meal Details**
```
بصفتي مستخدم
أريد أن أرى تفاصيل الوجبة
لكي أتأكد قبل الطلب
```

**Acceptance Criteria:**
- عرض صورة الوجبة
- عرض الاسم والوصف
- عرض السعر
- عرض المكونات

### Epic 3: Cart & Checkout

**US-6: Add to Cart**
```
بصفتي مستخدم
أريد أن أضيف وجبة للسلة
لكي أطلبها لاحقاً
```

**Acceptance Criteria:**
- يمكن للمستخدم إضافة وجبة للسلة
- يتم تحديث عدد العناصر
- يتم حفظ السلة في localStorage

**US-7: Manage Cart**
```
بصفتي مستخدم
أريد أن أعدل السلة
لكي أتحكم في طلبي
```

**Acceptance Criteria:**
- يمكن للمستخدم زيادة الكمية
- يمكن للمستخدم تقليل الكمية
- يمكن للمستخدم حذف عنصر
- يتم تحديث المجموع تلقائياً

**US-8: Checkout**
```
بصفتي مستخدم
أريد أن أكمل الطلب
لكي أستلم طعامي
```

**Acceptance Criteria:**
- يمكن للمستخدم إدخال معلوماته
- يمكن للمستخدم إدخال العنوان
- عرض ملخص الطلب
- عند الإرسال، يتم إنشاء الطلب

### Epic 4: Orders

**US-9: View Orders**
```
بصفتي مستخدم
أريد أن أرى طلباتي
لكي أتتبعها
```

**Acceptance Criteria:**
- عرض قائمة الطلبات
- عرض حالة كل طلب
- يمكن للمستخدم رؤية التفاصيل

**US-10: Track Order**
```
بصفتي مستخدم
أريد أن أتتبع طلبي
لكي أعرف متى يصل
```

**Acceptance Criteria:**
- عرض حالة الطلب الحالية
- عرض تاريخ الطلب
- عرض تفاصيل الطلب

### Epic 5: Profile

**US-11: View Profile**
```
بصفتي مستخدم
أريد أن أرى ملفي الشخصي
لكي أرى معلوماتي
```

**Acceptance Criteria:**
- عرض اسم المستخدم
- عرض email
- عرض رقم الهاتف

**US-12: Update Profile**
```
بصفتي مستخدم
أريد أن أحدث ملفي الشخصي
لكي أحدث معلوماتي
```

**Acceptance Criteria:**
- يمكن للمستخدم تحديث اسمه
- يمكن للمستخدم تحديث رقم هاتفه
- يتم حفظ التغييرات

---

## 📄 Pages

### 1. Login Page (`/login`)
- نموذج تسجيل الدخول
- Email و password fields
- Remember me checkbox
- Forgot password link
- Sign up link

### 2. Home Page (`/`)
- Header مع navigation
- Hero section
- Categories section
- Popular meals section
- Call to action

### 3. Meals Page (`/meals`)
- Search bar
- Filter by category
- Sort options
- Meal cards grid
- Pagination
- Loading/error/empty states

### 4. Meal Details Page (`/meals/:id`)
- Meal image
- Meal name
- Description
- Price
- Ingredients
- Quantity selector
- Add to cart button
- Related meals

### 5. Cart Page (`/cart`)
- Cart items list
- Quantity controls
- Remove button
- Subtotal
- Tax
- Total
- Clear cart button
- Checkout button

### 6. Checkout Page (`/checkout`)
- Customer information form
- Address form
- Payment method
- Order summary
- Submit order button

### 7. Orders Page (`/orders`)
- Orders list
- Order status badges
- Order dates
- View details button

### 8. Order Details Page (`/orders/:id`)
- Order information
- Items list
- Total amount
- Status timeline
- Delivery information

### 9. Profile Page (`/profile`)
- User information
- Edit profile form
- Change password
- Order history link

### 10. Not Found Page (`/404`)
- Error message
- Go home button

---

## 🧩 Components

### Layout Components

#### 1. `Layout`
- Main layout wrapper
- Header
- Footer
- Content area

#### 2. `Header`
- Logo
- Navigation links
- Cart icon with badge
- User menu
- Mobile menu toggle

#### 3. `Footer`
- Links
- Social media
- Copyright

#### 4. `Sidebar`
- Mobile navigation
- Close button

### Auth Components

#### 5. `LoginForm`
- Email input
- Password input
- Submit button
- Error message

#### 6. `ProtectedRoute`
- Authentication check
- Redirect to login

### Meal Components

#### 7. `MealCard`
- Meal image
- Meal name
- Price
- Rating
- Add to cart button

#### 8. `MealFilters`
- Category dropdown
- Search input
- Sort dropdown

#### 9. `MealCardSkeleton`
- Loading skeleton

#### 10. `EmptyState`
- Illustration
- Message
- Action button

#### 11. `ErrorState`
- Error icon
- Error message
- Retry button

### Cart Components

#### 12. `CartItem`
- Item image
- Item name
- Price
- Quantity controls
- Remove button

#### 13. `CartSummary`
- Subtotal
- Tax
- Total
- Checkout button

#### 14. `CartBadge`
- Badge count
- Animation

### Form Components

#### 15. `Input`
- Label
- Input field
- Error message
- Helper text

#### 16. `Select`
- Label
- Select dropdown
- Options

#### 17. `Button`
- Variants (primary, secondary, danger)
- Sizes (sm, md, lg)
- Loading state
- Disabled state

#### 18. `Form`
- Form wrapper
- Validation
- Error handling

### Order Components

#### 19. `OrderCard`
- Order ID
- Date
- Status
- Total
- View details button

#### 20. `OrderStatusBadge`
- Status color
- Status text

#### 21. `OrderTimeline`
- Status steps
- Current step highlight

### UI Components

#### 22. `LoadingSpinner`
- Spinner animation
- Size variants

#### 23. `Modal`
- Overlay
- Content
- Close button

#### 24. `Toast`
- Success/error/info
- Auto dismiss
- Animation

#### 25. `Pagination`
- Page numbers
- Prev/Next buttons

---

## 🔌 API Endpoints

### Authentication

#### POST `/api/auth/login`
```json
// Request
{
  "email": "user@example.com",
  "password": "password123"
}

// Response
{
  "access_token": "eyJhbG...",
  "refresh_token": "eyJhbG...",
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "user@example.com",
    "phone": "+1234567890"
  }
}
```

#### POST `/api/auth/register`
```json
// Request
{
  "name": "John Doe",
  "email": "user@example.com",
  "password": "password123",
  "phone": "+1234567890"
}

// Response
{
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "user@example.com",
    "phone": "+1234567890"
  }
}
```

#### POST `/api/auth/logout`
```json
// Request
{
  "refresh_token": "eyJhbG..."
}

// Response
{
  "message": "Logged out successfully"
}
```

#### POST `/api/auth/refresh`
```json
// Request
{
  "refresh_token": "eyJhbG..."
}

// Response
{
  "access_token": "eyJhbG..."
}
```

### Categories

#### GET `/api/categories`
```json
// Response
[
  {
    "id": 1,
    "name": "Burgers",
    "image": "/categories/burgers.jpg",
    "description": "Delicious burgers"
  }
]
```

#### GET `/api/categories/:id`
```json
// Response
{
  "id": 1,
  "name": "Burgers",
  "image": "/categories/burgers.jpg",
  "description": "Delicious burgers"
}
```

### Meals

#### GET `/api/meals`
```json
// Query Params: ?page=1&limit=12&category=1&search=burger
// Response
{
  "data": [
    {
      "id": 1,
      "name": "Classic Burger",
      "description": "Juicy beef patty with fresh vegetables",
      "price": 12.99,
      "image": "/meals/burger.jpg",
      "category_id": 1,
      "rating": 4.5,
      "ingredients": ["Beef", "Lettuce", "Tomato", "Cheese"],
      "preparation_time": 15
    }
  ],
  "meta": {
    "total": 50,
    "page": 1,
    "limit": 12,
    "total_pages": 5
  }
}
```

#### GET `/api/meals/:id`
```json
// Response
{
  "id": 1,
  "name": "Classic Burger",
  "description": "Juicy beef patty with fresh vegetables",
  "price": 12.99,
  "image": "/meals/burger.jpg",
  "category_id": 1,
  "rating": 4.5,
  "ingredients": ["Beef", "Lettuce", "Tomato", "Cheese"],
  "preparation_time": 15,
  "nutrition": {
    "calories": 550,
    "protein": 30,
    "carbs": 40,
    "fat": 25
  }
}
```

#### GET `/api/meals/popular`
```json
// Response
[
  {
    "id": 1,
    "name": "Classic Burger",
    "price": 12.99,
    "image": "/meals/burger.jpg",
    "orders_count": 150
  }
]
```

### Cart

#### POST `/api/cart/add`
```json
// Request
{
  "meal_id": 1,
  "quantity": 2
}

// Response
{
  "cart": {
    "id": 1,
    "items": [
      {
        "id": 1,
        "meal_id": 1,
        "quantity": 2,
        "meal": {
          "id": 1,
          "name": "Classic Burger",
          "price": 12.99
        }
      }
    ],
    "total": 25.98
  }
}
```

#### GET `/api/cart`
```json
// Response
{
  "id": 1,
  "items": [
    {
      "id": 1,
      "meal_id": 1,
      "quantity": 2,
      "meal": {
        "id": 1,
        "name": "Classic Burger",
        "price": 12.99
      }
    }
  ],
  "total": 25.98
}
```

#### PUT `/api/cart/items/:id`
```json
// Request
{
  "quantity": 3
}

// Response
{
  "cart": {
    "id": 1,
    "items": [...],
    "total": 38.97
  }
}
```

#### DELETE `/api/cart/items/:id`
```json
// Response
{
  "cart": {
    "id": 1,
    "items": [],
    "total": 0
  }
}
```

#### DELETE `/api/cart`
```json
// Response
{
  "message": "Cart cleared"
}
```

### Orders

#### POST `/api/orders`
```json
// Request
{
  "customer_name": "John Doe",
  "phone": "+1234567890",
  "address": "123 Main St",
  "payment_method": "cash",
  "notes": "Extra ketchup please"
}

// Response
{
  "id": 1,
  "customer_name": "John Doe",
  "phone": "+1234567890",
  "address": "123 Main St",
  "status": "pending",
  "total": 25.98,
  "items": [...],
  "created_at": "2024-01-15T10:30:00Z"
}
```

#### GET `/api/orders`
```json
// Response
{
  "data": [
    {
      "id": 1,
      "customer_name": "John Doe",
      "status": "pending",
      "total": 25.98,
      "created_at": "2024-01-15T10:30:00Z"
    }
  ],
  "meta": {
    "total": 10,
    "page": 1,
    "limit": 10
  }
}
```

#### GET `/api/orders/:id`
```json
// Response
{
  "id": 1,
  "customer_name": "John Doe",
  "phone": "+1234567890",
  "address": "123 Main St",
  "status": "pending",
  "total": 25.98,
  "items": [
    {
      "meal_id": 1,
      "meal_name": "Classic Burger",
      "quantity": 2,
      "price": 12.99
    }
  ],
  "created_at": "2024-01-15T10:30:00Z",
  "estimated_delivery": "2024-01-15T11:00:00Z"
}
```

#### PATCH `/api/orders/:id/status`
```json
// Request
{
  "status": "preparing"
}

// Response
{
  "id": 1,
  "status": "preparing"
}
```

### User Profile

#### GET `/api/user/profile`
```json
// Response
{
  "id": 1,
  "name": "John Doe",
  "email": "user@example.com",
  "phone": "+1234567890",
  "avatar": "/avatars/user1.jpg"
}
```

#### PUT `/api/user/profile`
```json
// Request
{
  "name": "John Smith",
  "phone": "+0987654321"
}

// Response
{
  "id": 1,
  "name": "John Smith",
  "email": "user@example.com",
  "phone": "+0987654321",
  "avatar": "/avatars/user1.jpg"
}
```

#### PUT `/api/user/password`
```json
// Request
{
  "current_password": "oldpassword",
  "new_password": "newpassword"
}

// Response
{
  "message": "Password updated successfully"
}
```

---

## 📊 Data Models

### User
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  phone: string;
  avatar?: string;
  created_at: string;
}
```

### Category
```typescript
interface Category {
  id: number;
  name: string;
  image: string;
  description: string;
}
```

### Meal
```typescript
interface Meal {
  id: number;
  name: string;
  description: string;
  price: number;
  image: string;
  category_id: number;
  category?: Category;
  rating: number;
  ingredients: string[];
  preparation_time: number;
  nutrition?: {
    calories: number;
    protein: number;
    carbs: number;
    fat: number;
  };
}
```

### CartItem
```typescript
interface CartItem {
  id: number;
  meal_id: number;
  quantity: number;
  meal: Meal;
}
```

### Cart
```typescript
interface Cart {
  id: number;
  items: CartItem[];
  total: number;
}
```

### Order
```typescript
interface Order {
  id: number;
  customer_name: string;
  phone: string;
  address: string;
  status: 'pending' | 'preparing' | 'ready' | 'delivered' | 'cancelled';
  total: number;
  items: OrderItem[];
  payment_method: 'cash' | 'card';
  notes?: string;
  created_at: string;
  estimated_delivery?: string;
}

interface OrderItem {
  meal_id: number;
  meal_name: string;
  quantity: number;
  price: number;
}
```

### PaginatedResponse
```typescript
interface PaginatedResponse<T> {
  data: T[];
  meta: {
    total: number;
    page: number;
    limit: number;
    total_pages: number;
  };
}
```

---

## 🎨 State Management Strategy

### Server State (React Query)

**الاستخدام**: البيانات من API

```typescript
// استخدم React Query لـ:
- Users profile
- Categories
- Meals list
- Meal details
- Cart items
- Orders list
- Order details
```

**السبب**:
- ✅ Caching تلقائي
- ✅ Deduplication
- ✅ Auto-refetch
- ✅ Error handling مدمج
- ✅ Optimistic updates
- ✅ Loading states

### Client State (Zustand)

**الاستخدام**: UI state والمحلي

```typescript
// استخدم Zustand لـ:
- Auth state (user, isAuthenticated)
- UI state (sidebar open, modal open)
- Cart state (مؤقتاً قبل sync مع API)
- Form state (مؤقتاً)
- Theme preferences
- Language preferences
```

**السبب**:
- ✅ API بسيط
- ✅ حجم صغير
- ✅ TypeScript ممتاز
- ✅ Persist middleware
- ✅ لا يحتاج Provider

### Local State (useState)

**الاستخدام**: State محلي للمكون

```typescript
// استخدم useState لـ:
- Form inputs (مؤقتاً)
- Modal open/close
- Selection state
- Temporary calculations
```

**السبب**:
- ✅ بسيط وسريع
- ✅ جزء من React
- ✅ مناسب للمكونات المنفصلة

### Context API

**الاستخدام**: Global state بسيط

```typescript
// استخدم Context لـ:
- Theme (light/dark)
- Language (en/ar)
- Toast notifications
```

**السبب**:
- ✅ جزء من React
- ✅ مناسب للـ simple global state
- ✅ لا يحتاج مكتبة

---

## 🧭 Routing Strategy

### Route Structure

```typescript
/                        → Home (public)
/login                   → Login (public)
/register                → Register (public)
/meals                   → Meals list (public)
/meals/:id               → Meal details (public)
/cart                    → Cart (public)
/checkout                → Checkout (protected)
/orders                  → Orders list (protected)
/orders/:id              → Order details (protected)
/profile                 → Profile (protected)
/404                     → Not found
```

### Protected Routes

```typescript
// Protected routes (require authentication):
- /checkout
- /orders
- /orders/:id
- /profile
```

### Public Routes

```typescript
// Public routes (no authentication required):
- /
- /login
- /register
- /meals
- /meals/:id
- /cart
```

---

## 📁 Folder Structure

```
food-ordering-app/
├── public/
│   ├── images/
│   └── favicon.ico
├── src/
│   ├── assets/
│   │   ├── images/
│   │   ├── icons/
│   │   └── styles/
│   ├── components/
│   │   ├── common/
│   │   │   ├── Button/
│   │   │   ├── Input/
│   │   │   ├── Select/
│   │   │   ├── Modal/
│   │   │   ├── Toast/
│   │   │   ├── LoadingSpinner/
│   │   │   └── EmptyState/
│   │   ├── layout/
│   │   │   ├── Layout/
│   │   │   ├── Header/
│   │   │   ├── Footer/
│   │   │   └── Sidebar/
│   │   ├── auth/
│   │   │   ├── LoginForm/
│   │   │   └── ProtectedRoute/
│   │   ├── meals/
│   │   │   ├── MealCard/
│   │   │   ├── MealFilters/
│   │   │   ├── MealCardSkeleton/
│   │   │   └── CategoryCard/
│   │   ├── cart/
│   │   │   ├── CartItem/
│   │   │   ├── CartSummary/
│   │   │   └── CartBadge/
│   │   ├── orders/
│   │   │   ├── OrderCard/
│   │   │   ├── OrderStatusBadge/
│   │   │   └── OrderTimeline/
│   │   └── profile/
│   │       └── ProfileForm/
│   ├── pages/
│   │   ├── Home/
│   │   │   ├── index.tsx
│   │   │   ├── Hero.tsx
│   │   │   ├── Categories.tsx
│   │   │   └── PopularMeals.tsx
│   │   ├── Login/
│   │   │   └── index.tsx
│   │   ├── Register/
│   │   │   └── index.tsx
│   │   ├── Meals/
│   │   │   └── index.tsx
│   │   ├── MealDetails/
│   │   │   └── index.tsx
│   │   ├── Cart/
│   │   │   └── index.tsx
│   │   ├── Checkout/
│   │   │   └── index.tsx
│   │   ├── Orders/
│   │   │   └── index.tsx
│   │   ├── OrderDetails/
│   │   │   └── index.tsx
│   │   ├── Profile/
│   │   │   └── index.tsx
│   │   └── NotFound/
│   │       └── index.tsx
│   ├── layouts/
│   │   ├── MainLayout/
│   │   └── AuthLayout/
│   ├── services/
│   │   ├── api/
│   │   │   ├── auth.ts
│   │   │   ├── categories.ts
│   │   │   ├── meals.ts
│   │   │   ├── cart.ts
│   │   │   ├── orders.ts
│   │   │   └── user.ts
│   │   ├── apiClient.ts
│   │   └── queryClient.ts
│   ├── stores/
│   │   ├── authStore.ts
│   │   ├── cartStore.ts
│   │   └── uiStore.ts
│   ├── hooks/
│   │   ├── useAuth.ts
│   │   ├── useCart.ts
│   │   └── useDebounce.ts
│   ├── types/
│   │   ├── index.ts
│   │   ├── user.ts
│   │   ├── meal.ts
│   │   ├── order.ts
│   │   └── cart.ts
│   ├── utils/
│   │   ├── format.ts
│   │   ├── validation.ts
│   │   └── constants.ts
│   ├── routes/
│   │   ├── index.tsx
│   │   └── protectedRoutes.tsx
│   ├── App.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
├── .env
├── .env.example
├── .gitignore
├── index.html
├── package.json
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
└── README.md
```

---

## 🛠️ Tech Stack Justification

### 1. React

**لماذا React؟**
- ✅ Component-based architecture
- ✅ Large ecosystem و community
- ✅ Virtual DOM للأداء
- ✅ One-way data flow
- ✅ TypeScript support ممتاز
- ✅ Rich documentation

**البديل**: Vue.js
- Vue أبسط لكن React أكثر شعبية في الشركات

### 2. TypeScript

**لماذا TypeScript؟**
- ✅ Type safety في compile time
- ✅ Better IDE support (autocomplete, refactoring)
- ✅ Easier maintenance في large codebases
- ✅ Self-documenting code
- ✅ Catches bugs early
- ✅ Team collaboration أفضل

**البديل**: JavaScript
- JavaScript أسهل لكن TypeScript أكثر أماناً للمشاريع الكبيرة

### 3. Vite

**لماذا Vite؟**
- ✅ Super fast HMR (Hot Module Replacement)
- ✅ Out-of-the-box TypeScript support
- ✅ Optimized build process
- ✅ ESLint/Plugin ecosystem
- ✅ Modern ES modules
- ✅ Faster than Create React App

**البديل**: Create React App
- CRA أبطأ وغير محدث

### 4. React Router

**لماذا React Router؟**
- ✅ Declarative routing
- ✅ Dynamic routing
- ✅ Nested routes
- ✅ Code splitting support
- ✅ Standard في React ecosystem
- ✅ Great TypeScript support

**البديل**: Next.js
- Next.js أفضل لـ SSR لكن نحتاج SPA فقط

### 5. Zustand

**لماذا Zustand؟**
- ✅ Tiny (~1KB)
- ✅ Simple API
- ✅ No Provider needed
- ✅ Great TypeScript support
- ✅ Built-in middleware (persist, devtools)
- ✅ Performance ممتاز

**البديل**: Redux
- Redux معقد جداً لمشروع بهذا الحجم

**البديل**: Context API
- Context يسبب re-renders غير ضرورية

### 6. React Query

**لماذا React Query؟**
- ✅ Server state management built for React
- ✅ Automatic caching
- ✅ Deduplication
- ✅ Auto-refetching
- ✅ Optimistic updates
- ✅ Loading/error states مدمجة
- ✅ Great TypeScript support

**البديل**: Redux Toolkit + RTK Query
- RTK Query جيد لكن React Query أبسط

**البديل**: SWR
- SWR جيد لكن React Query أكثر features

### 7. Axios

**لماذا Axios؟**
- ✅ Automatic JSON transformation
- ✅ Request/response interceptors
- ✅ Error handling built-in
- ✅ Request cancellation
- ✅ XSRF protection
- ✅ Wide browser support

**البديل**: Fetch API
- Fetch أصغر لكن Axios أبسط في الاستخدام

### 8. useState

**لماذا useState؟**
- ✅ Built-in React hook
- ✅ Simple for local state
- ✅ No extra dependencies
- ✅ Perfect for component-level state

**الاستخدام**: Form inputs, modal states, temporary data

### 9. Context API

**لماذا Context API؟**
- ✅ Built-in React feature
- ✅ No extra dependencies
- ✅ Perfect for simple global state
- ✅ Great for theme, language, notifications

**الاستخدام**: Theme, language, toast notifications

---

## 🚀 Part 1: Architecture + Setup + Routing

### Step 1: Project Setup

```bash
# إنشاء المشروع
npm create vite@latest food-ordering-app -- --template react-ts

# الدخول للمجلد
cd food-ordering-app

# تثبيت dependencies
npm install react-router-dom zustand @tanstack/react-query axios
npm install -D @types/node

# تثبيت Tailwind CSS (اختياري)
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### Step 2: Configuration Files

#### `tsconfig.json`
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@/components/*": ["src/components/*"],
      "@/pages/*": ["src/pages/*"],
      "@/services/*": ["src/services/*"],
      "@/stores/*": ["src/stores/*"],
      "@/types/*": ["src/types/*"],
      "@/utils/*": ["src/utils/*"],
      "@/hooks/*": ["src/hooks/*"]
    }
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

#### `vite.config.ts`
```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@/components': path.resolve(__dirname, './src/components'),
      '@/pages': path.resolve(__dirname, './src/pages'),
      '@/services': path.resolve(__dirname, './src/services'),
      '@/stores': path.resolve(__dirname, './src/stores'),
      '@/types': path.resolve(__dirname, './src/types'),
      '@/utils': path.resolve(__dirname, './src/utils'),
      '@/hooks': path.resolve(__dirname, './src/hooks'),
    },
  },
  server: {
    port: 3000,
    proxy: {
      '/api': {
        target: 'http://localhost:5000',
        changeOrigin: true,
      },
    },
  },
});
```

#### `.env`
```env
VITE_API_URL=http://localhost:5000/api
VITE_APP_NAME=Food Ordering App
```

### Step 3: Types Definition

#### `src/types/index.ts`
```typescript
export * from './user';
export * from './meal';
export * from './order';
export * from './cart';
export * from './category';
```

#### `src/types/user.ts`
```typescript
export interface User {
  id: number;
  name: string;
  email: string;
  phone: string;
  avatar?: string;
  created_at: string;
}

export interface LoginCredentials {
  email: string;
  password: string;
}

export interface RegisterData {
  name: string;
  email: string;
  password: string;
  phone: string;
}

export interface AuthResponse {
  access_token: string;
  refresh_token: string;
  user: User;
}
```

#### `src/types/meal.ts`
```typescript
export interface Meal {
  id: number;
  name: string;
  description: string;
  price: number;
  image: string;
  category_id: number;
  category?: Category;
  rating: number;
  ingredients: string[];
  preparation_time: number;
  nutrition?: {
    calories: number;
    protein: number;
    carbs: number;
    fat: number;
  };
}

export interface Category {
  id: number;
  name: string;
  image: string;
  description: string;
}

export interface MealsFilters {
  page?: number;
  limit?: number;
  category_id?: number;
  search?: string;
  sort?: 'name' | 'price' | 'rating' | 'popular';
  order?: 'asc' | 'desc';
}

export interface PaginatedResponse<T> {
  data: T[];
  meta: {
    total: number;
    page: number;
    limit: number;
    total_pages: number;
  };
}
```

#### `src/types/cart.ts`
```typescript
import { Meal } from './meal';

export interface CartItem {
  id: number;
  meal_id: number;
  quantity: number;
  meal: Meal;
}

export interface Cart {
  id: number;
  items: CartItem[];
  total: number;
}

export interface AddToCartData {
  meal_id: number;
  quantity: number;
}
```

#### `src/types/order.ts`
```typescript
export interface Order {
  id: number;
  customer_name: string;
  phone: string;
  address: string;
  status: 'pending' | 'preparing' | 'ready' | 'delivered' | 'cancelled';
  total: number;
  items: OrderItem[];
  payment_method: 'cash' | 'card';
  notes?: string;
  created_at: string;
  estimated_delivery?: string;
}

export interface OrderItem {
  meal_id: number;
  meal_name: string;
  quantity: number;
  price: number;
}

export interface CreateOrderData {
  customer_name: string;
  phone: string;
  address: string;
  payment_method: 'cash' | 'card';
  notes?: string;
}
```

### Step 4: API Client Setup

#### `src/services/apiClient.ts`
```typescript
import axios from 'axios';

const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL || '/api',
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor - إضافة token
apiClient.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('access_token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

// Response interceptor - refresh token
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;
    
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      
      try {
        const refreshToken = localStorage.getItem('refresh_token');
        const response = await axios.post(
          `${import.meta.env.VITE_API_URL}/auth/refresh`,
          { refresh_token: refreshToken }
        );
        
        const { access_token } = response.data;
        localStorage.setItem('access_token', access_token);
        
        originalRequest.headers.Authorization = `Bearer ${access_token}`;
        return apiClient(originalRequest);
      } catch (refreshError) {
        localStorage.removeItem('access_token');
        localStorage.removeItem('refresh_token');
        window.location.href = '/login';
      }
    }
    
    return Promise.reject(error);
  }
);

export default apiClient;
```

#### `src/services/queryClient.ts`
```typescript
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60 * 5, // 5 دقائق
      gcTime: 1000 * 60 * 10,   // 10 دقائق
      retry: 1,
      refetchOnWindowFocus: false,
    },
  },
});
```

### Step 5: API Services

#### `src/services/api/auth.ts`
```typescript
import apiClient from '../apiClient';
import { LoginCredentials, RegisterData, AuthResponse, User } from '@/types';

export const authService = {
  login: async (credentials: LoginCredentials): Promise<AuthResponse> => {
    const { data } = await apiClient.post<AuthResponse>('/auth/login', credentials);
    return data;
  },
  
  register: async (userData: RegisterData): Promise<{ user: User }> => {
    const { data } = await apiClient.post<{ user: User }>('/auth/register', userData);
    return data;
  },
  
  logout: async (refreshToken: string): Promise<void> => {
    await apiClient.post('/auth/logout', { refresh_token: refreshToken });
  },
  
  refresh: async (refreshToken: string): Promise<{ access_token: string }> => {
    const { data } = await apiClient.post<{ access_token: string }>('/auth/refresh', {
      refresh_token: refreshToken,
    });
    return data;
  },
  
  getProfile: async (): Promise<User> => {
    const { data } = await apiClient.get<User>('/user/profile');
    return data;
  },
  
  updateProfile: async (updates: Partial<User>): Promise<User> => {
    const { data } = await apiClient.put<User>('/user/profile', updates);
    return data;
  },
};
```

#### `src/services/api/meals.ts`
```typescript
import apiClient from '../apiClient';
import { Meal, Category, MealsFilters, PaginatedResponse } from '@/types';

export const mealsService = {
  getMeals: async (filters: MealsFilters = {}): Promise<PaginatedResponse<Meal>> => {
    const { data } = await apiClient.get<PaginatedResponse<Meal>>('/meals', {
      params: filters,
    });
    return data;
  },
  
  getMeal: async (id: number): Promise<Meal> => {
    const { data } = await apiClient.get<Meal>(`/meals/${id}`);
    return data;
  },
  
  getPopularMeals: async (): Promise<Meal[]> => {
    const { data } = await apiClient.get<Meal[]>('/meals/popular');
    return data;
  },
  
  getCategories: async (): Promise<Category[]> => {
    const { data } = await apiClient.get<Category[]>('/categories');
    return data;
  },
  
  getCategory: async (id: number): Promise<Category> => {
    const { data } = await apiClient.get<Category>(`/categories/${id}`);
    return data;
  },
};
```

#### `src/services/api/cart.ts`
```typescript
import apiClient from '../apiClient';
import { Cart, CartItem, AddToCartData } from '@/types';

export const cartService = {
  getCart: async (): Promise<Cart> => {
    const { data } = await apiClient.get<Cart>('/cart');
    return data;
  },
  
  addToCart: async (data: AddToCartData): Promise<Cart> => {
    const { data: cart } = await apiClient.post<Cart>('/cart/add', data);
    return cart;
  },
  
  updateCartItem: async (itemId: number, quantity: number): Promise<Cart> => {
    const { data } = await apiClient.put<Cart>(`/cart/items/${itemId}`, { quantity });
    return data;
  },
  
  removeCartItem: async (itemId: number): Promise<Cart> => {
    const { data } = await apiClient.delete<Cart>(`/cart/items/${itemId}`);
    return data;
  },
  
  clearCart: async (): Promise<void> => {
    await apiClient.delete('/cart');
  },
};
```

#### `src/services/api/orders.ts`
```typescript
import apiClient from '../apiClient';
import { Order, CreateOrderData, PaginatedResponse } from '@/types';

export const ordersService = {
  getOrders: async (page = 1, limit = 10): Promise<PaginatedResponse<Order>> => {
    const { data } = await apiClient.get<PaginatedResponse<Order>>('/orders', {
      params: { page, limit },
    });
    return data;
  },
  
  getOrder: async (id: number): Promise<Order> => {
    const { data } = await apiClient.get<Order>(`/orders/${id}`);
    return data;
  },
  
  createOrder: async (orderData: CreateOrderData): Promise<Order> => {
    const { data } = await apiClient.post<Order>('/orders', orderData);
    return data;
  },
  
  updateOrderStatus: async (id: number, status: Order['status']): Promise<Order> => {
    const { data } = await apiClient.patch<Order>(`/orders/${id}/status`, { status });
    return data;
  },
};
```

### Step 6: Zustand Stores

#### `src/stores/authStore.ts`
```typescript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';
import { User } from '@/types';

interface AuthStore {
  user: User | null;
  isAuthenticated: boolean;
  setAuth: (user: User, tokens: { access_token: string; refresh_token: string }) => void;
  logout: () => void;
  updateUser: (updates: Partial<User>) => void;
}

export const useAuthStore = create<AuthStore>()(
  persist(
    (set) => ({
      user: null,
      isAuthenticated: false,
      
      setAuth: (user, tokens) => {
        localStorage.setItem('access_token', tokens.access_token);
        localStorage.setItem('refresh_token', tokens.refresh_token);
        set({ user, isAuthenticated: true });
      },
      
      logout: () => {
        localStorage.removeItem('access_token');
        localStorage.removeItem('refresh_token');
        set({ user: null, isAuthenticated: false });
      },
      
      updateUser: (updates) => set((state) => ({
        user: state.user ? { ...state.user, ...updates } : null,
      })),
    }),
    {
      name: 'auth-storage',
      partialize: (state) => ({ user: state.user, isAuthenticated: state.isAuthenticated }),
    }
  )
);
```

#### `src/stores/cartStore.ts`
```typescript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';
import { CartItem } from '@/types';

interface CartStore {
  items: CartItem[];
  addItem: (item: CartItem) => void;
  removeItem: (mealId: number) => void;
  updateQuantity: (mealId: number, quantity: number) => void;
  clearCart: () => void;
  getTotal: () => number;
  getItemCount: () => number;
}

export const useCartStore = create<CartStore>()(
  persist(
    (set, get) => ({
      items: [],
      
      addItem: (item) => set((state) => {
        const existingItem = state.items.find((i) => i.meal_id === item.meal_id);
        
        if (existingItem) {
          return {
            items: state.items.map((i) =>
              i.meal_id === item.meal_id
                ? { ...i, quantity: i.quantity + item.quantity }
                : i
            ),
          };
        }
        
        return { items: [...state.items, item] };
      }),
      
      removeItem: (mealId) => set((state) => ({
        items: state.items.filter((item) => item.meal_id !== mealId),
      })),
      
      updateQuantity: (mealId, quantity) => set((state) => ({
        items: state.items
          .map((item) => (item.meal_id === mealId ? { ...item, quantity } : item))
          .filter((item) => item.quantity > 0),
      })),
      
      clearCart: () => set({ items: [] }),
      
      getTotal: () => {
        return get().items.reduce((total, item) => total + item.meal.price * item.quantity, 0);
      },
      
      getItemCount: () => {
        return get().items.reduce((count, item) => count + item.quantity, 0);
      },
    }),
    {
      name: 'cart-storage',
    }
  )
);
```

#### `src/stores/uiStore.ts`
```typescript
import { create } from 'zustand';

interface UIStore {
  sidebarOpen: boolean;
  mobileMenuOpen: boolean;
  toggleSidebar: () => void;
  toggleMobileMenu: () => void;
  closeMobileMenu: () => void;
}

export const useUIStore = create<UIStore>((set) => ({
  sidebarOpen: false,
  mobileMenuOpen: false,
  
  toggleSidebar: () => set((state) => ({ sidebarOpen: !state.sidebarOpen })),
  toggleMobileMenu: () => set((state) => ({ mobileMenuOpen: !state.mobileMenuOpen })),
  closeMobileMenu: () => set({ mobileMenuOpen: false }),
}));
```

### Step 7: Routing Setup

#### `src/routes/index.tsx`
```typescript
import { createBrowserRouter } from 'react-router-dom';
import Layout from '@/layouts/MainLayout';
import ProtectedRoute from '@/routes/protectedRoutes';
import Home from '@/pages/Home';
import Login from '@/pages/Login';
import Register from '@/pages/Register';
import Meals from '@/pages/Meals';
import MealDetails from '@/pages/MealDetails';
import Cart from '@/pages/Cart';
import Checkout from '@/pages/Checkout';
import Orders from '@/pages/Orders';
import OrderDetails from '@/pages/OrderDetails';
import Profile from '@/pages/Profile';
import NotFound from '@/pages/NotFound';

export const router = createBrowserRouter([
  {
    path: '/',
    element: <Layout />,
    children: [
      { index: true, element: <Home /> },
      { path: 'login', element: <Login /> },
      { path: 'register', element: <Register /> },
      { path: 'meals', element: <Meals /> },
      { path: 'meals/:id', element: <MealDetails /> },
      { path: 'cart', element: <Cart /> },
      {
        path: 'checkout',
        element: (
          <ProtectedRoute>
            <Checkout />
          </ProtectedRoute>
        ),
      },
      {
        path: 'orders',
        element: (
          <ProtectedRoute>
            <Orders />
          </ProtectedRoute>
        ),
      },
      {
        path: 'orders/:id',
        element: (
          <ProtectedRoute>
            <OrderDetails />
          </ProtectedRoute>
        ),
      },
      {
        path: 'profile',
        element: (
          <ProtectedRoute>
            <Profile />
          </ProtectedRoute>
        ),
      },
      { path: '*', element: <NotFound /> },
    ],
  },
]);
```

#### `src/routes/protectedRoutes.tsx`
```typescript
import { Navigate } from 'react-router-dom';
import { useAuthStore } from '@/stores/authStore';

function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  return <>{children}</>;
}

export default ProtectedRoute;
```

### Step 8: Main App Setup

#### `src/App.tsx`
```typescript
import { QueryClientProvider } from '@tanstack/react-query';
import { RouterProvider } from 'react-router-dom';
import { queryClient } from './services/queryClient';
import { router } from './routes';
import { Toaster } from 'react-hot-toast';

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <RouterProvider router={router} />
      <Toaster position="top-right" />
    </QueryClientProvider>
  );
}

export default App;
```

#### `src/main.tsx`
```typescript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

## 🚀 Part 2: API + Authentication + Products + Cart

### Step 9: Common Components

#### `src/components/common/Button/index.tsx`
```typescript
import { ButtonHTMLAttributes, forwardRef } from 'react';

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'danger' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  loading?: boolean;
  fullWidth?: boolean;
}

const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  (
    {
      children,
      variant = 'primary',
      size = 'md',
      loading = false,
      fullWidth = false,
      disabled,
      className = '',
      ...props
    },
    ref
  ) => {
    const baseStyles = 'inline-flex items-center justify-center rounded-lg font-medium transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed';
    
    const variantStyles = {
      primary: 'bg-orange-500 text-white hover:bg-orange-600 focus:ring-orange-500',
      secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300 focus:ring-gray-500',
      danger: 'bg-red-500 text-white hover:bg-red-600 focus:ring-red-500',
      ghost: 'bg-transparent text-gray-700 hover:bg-gray-100 focus:ring-gray-500',
    };
    
    const sizeStyles = {
      sm: 'px-3 py-1.5 text-sm',
      md: 'px-4 py-2 text-base',
      lg: 'px-6 py-3 text-lg',
    };
    
    return (
      <button
        ref={ref}
        disabled={disabled || loading}
        className={`${baseStyles} ${variantStyles[variant]} ${sizeStyles[size]} ${fullWidth ? 'w-full' : ''} ${className}`}
        {...props}
      >
        {loading ? (
          <svg className="animate-spin h-5 w-5" viewBox="0 0 24 24">
            <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4" fill="none" />
            <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z" />
          </svg>
        ) : (
          children
        )}
      </button>
    );
  }
);

Button.displayName = 'Button';

export default Button;
```

#### `src/components/common/Input/index.tsx`
```typescript
import { InputHTMLAttributes, forwardRef } from 'react';

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  label?: string;
  error?: string;
  helperText?: string;
}

const Input = forwardRef<HTMLInputElement, InputProps>(
  ({ label, error, helperText, className = '', ...props }, ref) => {
    return (
      <div className="w-full">
        {label && (
          <label className="block text-sm font-medium text-gray-700 mb-1">
            {label}
          </label>
        )}
        <input
          ref={ref}
          className={`w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500 ${
            error ? 'border-red-500' : 'border-gray-300'
          } ${className}`}
          {...props}
        />
        {error && <p className="mt-1 text-sm text-red-500">{error}</p>}
        {helperText && !error && <p className="mt-1 text-sm text-gray-500">{helperText}</p>}
      </div>
    );
  }
);

Input.displayName = 'Input';

export default Input;
```

#### `src/components/common/LoadingSpinner/index.tsx`
```typescript
interface LoadingSpinnerProps {
  size?: 'sm' | 'md' | 'lg';
  color?: string;
}

const LoadingSpinner = ({ size = 'md', color = 'currentColor' }: LoadingSpinnerProps) => {
  const sizeClasses = {
    sm: 'h-4 w-4',
    md: 'h-8 w-8',
    lg: 'h-12 w-12',
  };
  
  return (
    <svg
      className={`animate-spin ${sizeClasses[size]}`}
      viewBox="0 0 24 24"
      fill="none"
    >
      <circle
        className="opacity-25"
        cx="12"
        cy="12"
        r="10"
        stroke={color}
        strokeWidth="4"
      />
      <path
        className="opacity-75"
        fill={color}
        d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
      />
    </svg>
  );
};

export default LoadingSpinner;
```

#### `src/components/common/EmptyState/index.tsx`
```typescript
interface EmptyStateProps {
  title: string;
  description?: string;
  action?: {
    label: string;
    onClick: () => void;
  };
}

const EmptyState = ({ title, description, action }: EmptyStateProps) => {
  return (
    <div className="flex flex-col items-center justify-center py-12">
      <svg
        className="w-16 h-16 text-gray-400 mb-4"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          strokeLinecap="round"
          strokeLinejoin="round"
          strokeWidth={2}
          d="M20 13V6a2 2 0 00-2-2H6a2 2 0 00-2 2v7m16 0v5a2 2 0 01-2 2H6a2 2 0 01-2-2v-5m16 0h-2.586a1 1 0 00-.707.293l-2.414 2.414a1 1 0 01-.707.293h-3.172a1 1 0 01-.707-.293l-2.414-2.414A1 1 0 006.586 13H4"
        />
      </svg>
      <h3 className="text-lg font-medium text-gray-900 mb-2">{title}</h3>
      {description && <p className="text-gray-500 mb-4">{description}</p>}
      {action && (
        <button
          onClick={action.onClick}
          className="px-4 py-2 bg-orange-500 text-white rounded-lg hover:bg-orange-600"
        >
          {action.label}
        </button>
      )}
    </div>
  );
};

export default EmptyState;
```

### Step 10: Layout Components

#### `src/layouts/MainLayout/index.tsx`
```typescript
import { Outlet } from 'react-router-dom';
import Header from '@/components/layout/Header';
import Footer from '@/components/layout/Footer';

function MainLayout() {
  return (
    <div className="min-h-screen flex flex-col">
      <Header />
      <main className="flex-1">
        <Outlet />
      </main>
      <Footer />
    </div>
  );
}

export default MainLayout;
```

#### `src/components/layout/Header/index.tsx`
```typescript
import { Link, useNavigate } from 'react-router-dom';
import { useAuthStore } from '@/stores/authStore';
import { useCartStore } from '@/stores/cartStore';
import { useUIStore } from '@/stores/uiStore';

function Header() {
  const navigate = useNavigate();
  const user = useAuthStore((state) => state.user);
  const isAuthenticated = useAuthStore((state) => state.isAuthenticated);
  const logout = useAuthStore((state) => state.logout);
  const itemCount = useCartStore((state) => state.getItemCount());
  const mobileMenuOpen = useUIStore((state) => state.mobileMenuOpen);
  const toggleMobileMenu = useUIStore((state) => state.toggleMobileMenu);
  const closeMobileMenu = useUIStore((state) => state.closeMobileMenu);
  
  const handleLogout = () => {
    logout();
    navigate('/login');
  };
  
  return (
    <header className="bg-white shadow-md sticky top-0 z-50">
      <div className="container mx-auto px-4">
        <div className="flex items-center justify-between h-16">
          {/* Logo */}
          <Link to="/" className="text-2xl font-bold text-orange-500">
            FoodApp
          </Link>
          
          {/* Desktop Navigation */}
          <nav className="hidden md:flex items-center space-x-6">
            <Link to="/" className="text-gray-700 hover:text-orange-500">
              Home
            </Link>
            <Link to="/meals" className="text-gray-700 hover:text-orange-500">
              Meals
            </Link>
            <Link to="/cart" className="text-gray-700 hover:text-orange-500 relative">
              Cart
              {itemCount() > 0 && (
                <span className="absolute -top-2 -right-2 bg-orange-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
                  {itemCount()}
                </span>
              )}
            </Link>
            
            {isAuthenticated ? (
              <>
                <Link to="/orders" className="text-gray-700 hover:text-orange-500">
                  Orders
                </Link>
                <Link to="/profile" className="text-gray-700 hover:text-orange-500">
                  Profile
                </Link>
                <button
                  onClick={handleLogout}
                  className="text-gray-700 hover:text-orange-500"
                >
                  Logout
                </button>
              </>
            ) : (
              <>
                <Link to="/login" className="text-gray-700 hover:text-orange-500">
                  Login
                </Link>
                <Link
                  to="/register"
                  className="bg-orange-500 text-white px-4 py-2 rounded-lg hover:bg-orange-600"
                >
                  Sign Up
                </Link>
              </>
            )}
          </nav>
          
          {/* Mobile Menu Button */}
          <button
            onClick={toggleMobileMenu}
            className="md:hidden p-2 rounded-lg hover:bg-gray-100"
          >
            <svg className="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 6h16M4 12h16M4 18h16" />
            </svg>
          </button>
        </div>
        
        {/* Mobile Menu */}
        {mobileMenuOpen && (
          <nav className="md:hidden py-4 border-t">
            <Link
              to="/"
              onClick={closeMobileMenu}
              className="block py-2 text-gray-700 hover:text-orange-500"
            >
              Home
            </Link>
            <Link
              to="/meals"
              onClick={closeMobileMenu}
              className="block py-2 text-gray-700 hover:text-orange-500"
            >
              Meals
            </Link>
            <Link
              to="/cart"
              onClick={closeMobileMenu}
              className="block py-2 text-gray-700 hover:text-orange-500"
            >
              Cart ({itemCount()})
            </Link>
            
            {isAuthenticated ? (
              <>
                <Link
                  to="/orders"
                  onClick={closeMobileMenu}
                  className="block py-2 text-gray-700 hover:text-orange-500"
                >
                  Orders
                </Link>
                <Link
                  to="/profile"
                  onClick={closeMobileMenu}
                  className="block py-2 text-gray-700 hover:text-orange-500"
                >
                  Profile
                </Link>
                <button
                  onClick={() => {
                    handleLogout();
                    closeMobileMenu();
                  }}
                  className="block py-2 text-gray-700 hover:text-orange-500 w-full text-left"
                >
                  Logout
                </button>
              </>
            ) : (
              <>
                <Link
                  to="/login"
                  onClick={closeMobileMenu}
                  className="block py-2 text-gray-700 hover:text-orange-500"
                >
                  Login
                </Link>
                <Link
                  to="/register"
                  onClick={closeMobileMenu}
                  className="block py-2 text-orange-500 font-medium"
                >
                  Sign Up
                </Link>
              </>
            )}
          </nav>
        )}
      </div>
    </header>
  );
}

export default Header;
```

#### `src/components/layout/Footer/index.tsx`
```typescript
function Footer() {
  return (
    <footer className="bg-gray-800 text-white py-8">
      <div className="container mx-auto px-4">
        <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
          <div>
            <h3 className="text-lg font-bold mb-4">FoodApp</h3>
            <p className="text-gray-400">
              Order your favorite meals from the best restaurants in town.
            </p>
          </div>
          
          <div>
            <h3 className="text-lg font-bold mb-4">Quick Links</h3>
            <ul className="space-y-2">
              <li>
                <a href="/meals" className="text-gray-400 hover:text-white">
                  Meals
                </a>
              </li>
              <li>
                <a href="/orders" className="text-gray-400 hover:text-white">
                  Orders
                </a>
              </li>
              <li>
                <a href="/profile" className="text-gray-400 hover:text-white">
                  Profile
                </a>
              </li>
            </ul>
          </div>
          
          <div>
            <h3 className="text-lg font-bold mb-4">Contact</h3>
            <ul className="space-y-2 text-gray-400">
              <li>support@foodapp.com</li>
              <li>+1 234 567 890</li>
            </ul>
          </div>
        </div>
        
        <div className="border-t border-gray-700 mt-8 pt-8 text-center text-gray-400">
          <p>&copy; 2024 FoodApp. All rights reserved.</p>
        </div>
      </div>
    </footer>
  );
}

export default Footer;
```

### Step 11: Authentication Pages

#### `src/pages/Login/index.tsx`
```typescript
import { useState } from 'react';
import { useNavigate, Link } from 'react-router-dom';
import { useMutation } from '@tanstack/react-query';
import { useAuthStore } from '@/stores/authStore';
import { authService } from '@/services/api/auth';
import { LoginCredentials } from '@/types';
import Input from '@/components/common/Input';
import Button from '@/components/common/Button';
import toast from 'react-hot-toast';

function Login() {
  const navigate = useNavigate();
  const setAuth = useAuthStore((state) => state.setAuth);
  const [credentials, setCredentials] = useState<LoginCredentials>({
    email: '',
    password: '',
  });
  const [errors, setErrors] = useState<Partial<LoginCredentials>>({});
  
  const loginMutation = useMutation({
    mutationFn: authService.login,
    onSuccess: (data) => {
      setAuth(data.user, {
        access_token: data.access_token,
        refresh_token: data.refresh_token,
      });
      toast.success('Login successful!');
      navigate('/');
    },
    onError: (error: any) => {
      toast.error(error.response?.data?.message || 'Login failed');
    },
  });
  
  const validate = (): boolean => {
    const newErrors: Partial<LoginCredentials> = {};
    
    if (!credentials.email) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(credentials.email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!credentials.password) {
      newErrors.password = 'Password is required';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    
    if (validate()) {
      loginMutation.mutate(credentials);
    }
  };
  
  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100 py-12 px-4">
      <div className="max-w-md w-full bg-white rounded-lg shadow-md p-8">
        <h1 className="text-3xl font-bold text-center mb-8">Login</h1>
        
        <form onSubmit={handleSubmit} className="space-y-6">
          <Input
            label="Email"
            type="email"
            value={credentials.email}
            onChange={(e) => setCredentials({ ...credentials, email: e.target.value })}
            error={errors.email}
            placeholder="Enter your email"
          />
          
          <Input
            label="Password"
            type="password"
            value={credentials.password}
            onChange={(e) => setCredentials({ ...credentials, password: e.target.value })}
            error={errors.password}
            placeholder="Enter your password"
          />
          
          <Button
            type="submit"
            loading={loginMutation.isPending}
            fullWidth
          >
            Login
          </Button>
        </form>
        
        <p className="mt-6 text-center text-gray-600">
          Don't have an account?{' '}
          <Link to="/register" className="text-orange-500 hover:text-orange-600">
            Sign up
          </Link>
        </p>
      </div>
    </div>
  );
}

export default Login;
```

#### `src/pages/Register/index.tsx`
```typescript
import { useState } from 'react';
import { useNavigate, Link } from 'react-router-dom';
import { useMutation } from '@tanstack/react-query';
import { useAuthStore } from '@/stores/authStore';
import { authService } from '@/services/api/auth';
import { RegisterData } from '@/types';
import Input from '@/components/common/Input';
import Button from '@/components/common/Button';
import toast from 'react-hot-toast';

function Register() {
  const navigate = useNavigate();
  const setAuth = useAuthStore((state) => state.setAuth);
  const [userData, setUserData] = useState<RegisterData>({
    name: '',
    email: '',
    password: '',
    phone: '',
  });
  const [errors, setErrors] = useState<Partial<RegisterData>>({});
  
  const registerMutation = useMutation({
    mutationFn: authService.register,
    onSuccess: ({ user }) => {
      toast.success('Registration successful!');
      navigate('/login');
    },
    onError: (error: any) => {
      toast.error(error.response?.data?.message || 'Registration failed');
    },
  });
  
  const validate = (): boolean => {
    const newErrors: Partial<RegisterData> = {};
    
    if (!userData.name) {
      newErrors.name = 'Name is required';
    }
    
    if (!userData.email) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(userData.email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!userData.password) {
      newErrors.password = 'Password is required';
    } else if (userData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }
    
    if (!userData.phone) {
      newErrors.phone = 'Phone is required';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    
    if (validate()) {
      registerMutation.mutate(userData);
    }
  };
  
  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100 py-12 px-4">
      <div className="max-w-md w-full bg-white rounded-lg shadow-md p-8">
        <h1 className="text-3xl font-bold text-center mb-8">Sign Up</h1>
        
        <form onSubmit={handleSubmit} className="space-y-6">
          <Input
            label="Name"
            type="text"
            value={userData.name}
            onChange={(e) => setUserData({ ...userData, name: e.target.value })}
            error={errors.name}
            placeholder="Enter your name"
          />
          
          <Input
            label="Email"
            type="email"
            value={userData.email}
            onChange={(e) => setUserData({ ...userData, email: e.target.value })}
            error={errors.email}
            placeholder="Enter your email"
          />
          
          <Input
            label="Password"
            type="password"
            value={userData.password}
            onChange={(e) => setUserData({ ...userData, password: e.target.value })}
            error={errors.password}
            placeholder="Enter your password"
          />
          
          <Input
            label="Phone"
            type="tel"
            value={userData.phone}
            onChange={(e) => setUserData({ ...userData, phone: e.target.value })}
            error={errors.phone}
            placeholder="Enter your phone number"
          />
          
          <Button
            type="submit"
            loading={registerMutation.isPending}
            fullWidth
          >
            Sign Up
          </Button>
        </form>
        
        <p className="mt-6 text-center text-gray-600">
          Already have an account?{' '}
          <Link to="/login" className="text-orange-500 hover:text-orange-600">
            Login
          </Link>
        </p>
      </div>
    </div>
  );
}

export default Register;
```

### Step 12: Home Page

#### `src/pages/Home/Hero.tsx`
```typescript
import { Link } from 'react-router-dom';

function Hero() {
  return (
    <section className="bg-gradient-to-r from-orange-500 to-orange-600 text-white py-20">
      <div className="container mx-auto px-4 text-center">
        <h1 className="text-5xl font-bold mb-4">Delicious Food, Delivered</h1>
        <p className="text-xl mb-8 opacity-90">
          Order your favorite meals from the best restaurants in town
        </p>
        <Link
          to="/meals"
          className="inline-block bg-white text-orange-500 px-8 py-3 rounded-lg font-semibold hover:bg-gray-100 transition"
        >
          Order Now
        </Link>
      </div>
    </section>
  );
}

export default Hero;
```

#### `src/pages/Home/Categories.tsx`
```typescript
import { useQuery } from '@tanstack/react-query';
import { Link } from 'react-router-dom';
import { mealsService } from '@/services/api/meals';
import LoadingSpinner from '@/components/common/LoadingSpinner';

function Categories() {
  const { data: categories, isLoading, error } = useQuery({
    queryKey: ['categories'],
    queryFn: mealsService.getCategories,
  });
  
  if (isLoading) {
    return (
      <div className="flex justify-center py-12">
        <LoadingSpinner size="lg" />
      </div>
    );
  }
  
  if (error) {
    return (
      <div className="text-center py-12 text-red-500">
        Failed to load categories
      </div>
    );
  }
  
  return (
    <section className="py-16 bg-gray-50">
      <div className="container mx-auto px-4">
        <h2 className="text-3xl font-bold text-center mb-8">Browse by Category</h2>
        
        <div className="grid grid-cols-2 md:grid-cols-4 gap-6">
          {categories?.map((category) => (
            <Link
              key={category.id}
              to={`/meals?category=${category.id}`}
              className="group"
            >
              <div className="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition">
                <div className="h-40 bg-gray-200 relative overflow-hidden">
                  <img
                    src={category.image}
                    alt={category.name}
                    className="w-full h-full object-cover group-hover:scale-110 transition duration-300"
                  />
                </div>
                <div className="p-4">
                  <h3 className="font-semibold text-gray-900">{category.name}</h3>
                  <p className="text-sm text-gray-500 mt-1">{category.description}</p>
                </div>
              </div>
            </Link>
          ))}
        </div>
      </div>
    </section>
  );
}

export default Categories;
```

#### `src/pages/Home/PopularMeals.tsx`
```typescript
import { useQuery } from '@tanstack/react-query';
import { Link } from 'react-router-dom';
import { mealsService } from '@/services/api/meals';
import LoadingSpinner from '@/components/common/LoadingSpinner';

function PopularMeals() {
  const { data: meals, isLoading, error } = useQuery({
    queryKey: ['popular-meals'],
    queryFn: mealsService.getPopularMeals,
  });
  
  if (isLoading) {
    return (
      <div className="flex justify-center py-12">
        <LoadingSpinner size="lg" />
      </div>
    );
  }
  
  if (error) {
    return (
      <div className="text-center py-12 text-red-500">
        Failed to load popular meals
      </div>
    );
  }
  
  return (
    <section className="py-16">
      <div className="container mx-auto px-4">
        <h2 className="text-3xl font-bold text-center mb-8">Popular Meals</h2>
        
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          {meals?.map((meal) => (
            <Link key={meal.id} to={`/meals/${meal.id}`} className="group">
              <div className="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition">
                <div className="h-48 bg-gray-200 relative overflow-hidden">
                  <img
                    src={meal.image}
                    alt={meal.name}
                    className="w-full h-full object-cover group-hover:scale-110 transition duration-300"
                  />
                </div>
                <div className="p-4">
                  <h3 className="font-semibold text-gray-900">{meal.name}</h3>
                  <div className="flex items-center justify-between mt-2">
                    <span className="text-orange-500 font-bold">${meal.price.toFixed(2)}</span>
                    <div className="flex items-center">
                      <svg className="w-4 h-4 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
                        <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
                      </svg>
                      <span className="ml-1 text-sm text-gray-600">{meal.rating}</span>
                    </div>
                  </div>
                </div>
              </div>
            </Link>
          ))}
        </div>
        
        <div className="text-center mt-8">
          <Link
            to="/meals"
            className="inline-block bg-orange-500 text-white px-6 py-3 rounded-lg font-semibold hover:bg-orange-600 transition"
          >
            View All Meals
          </Link>
        </div>
      </div>
    </section>
  );
}

export default PopularMeals;
```

#### `src/pages/Home/index.tsx`
```typescript
import Hero from './Hero';
import Categories from './Categories';
import PopularMeals from './PopularMeals';

function Home() {
  return (
    <div>
      <Hero />
      <Categories />
      <PopularMeals />
    </div>
  );
}

export default Home;
```

### Step 13: Meals Page

#### `src/components/meals/MealCard/index.tsx`
```typescript
import { Link } from 'react-router-dom';
import { Meal } from '@/types';
import { useCartStore } from '@/stores/cartStore';
import toast from 'react-hot-toast';

interface MealCardProps {
  meal: Meal;
}

function MealCard({ meal }: MealCardProps) {
  const addItem = useCartStore((state) => state.addItem);
  
  const handleAddToCart = () => {
    addItem({
      id: Date.now(),
      meal_id: meal.id,
      quantity: 1,
      meal,
    });
    toast.success(`${meal.name} added to cart`);
  };
  
  return (
    <div className="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition">
      <Link to={`/meals/${meal.id}`}>
        <div className="h-48 bg-gray-200 relative overflow-hidden">
          <img
            src={meal.image}
            alt={meal.name}
            className="w-full h-full object-cover hover:scale-110 transition duration-300"
          />
        </div>
      </Link>
      
      <div className="p-4">
        <Link to={`/meals/${meal.id}`}>
          <h3 className="font-semibold text-gray-900 hover:text-orange-500">{meal.name}</h3>
        </Link>
        
        <p className="text-sm text-gray-500 mt-1 line-clamp-2">{meal.description}</p>
        
        <div className="flex items-center justify-between mt-3">
          <span className="text-orange-500 font-bold text-lg">${meal.price.toFixed(2)}</span>
          
          <div className="flex items-center gap-2">
            <div className="flex items-center">
              <svg className="w-4 h-4 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
                <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
              </svg>
              <span className="ml-1 text-sm text-gray-600">{meal.rating}</span>
            </div>
            
            <button
              onClick={handleAddToCart}
              className="p-2 bg-orange-500 text-white rounded-lg hover:bg-orange-600 transition"
              title="Add to cart"
            >
              <svg className="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </div>
  );
}

export default MealCard;
```

#### `src/components/meals/MealFilters/index.tsx`
```typescript
import { useState } from 'react';
import { useSearchParams } from 'react-router-dom';
import { useQuery } from '@tanstack/react-query';
import { mealsService } from '@/services/api/meals';
import Input from '@/components/common/Input';

interface MealFiltersProps {
  onFilterChange: (filters: any) => void;
}

function MealFilters({ onFilterChange }: MealFiltersProps) {
  const [searchParams] = useSearchParams();
  const [search, setSearch] = useState(searchParams.get('search') || '');
  const [categoryId, setCategoryId] = useState(searchParams.get('category') || '');
  const [sort, setSort] = useState(searchParams.get('sort') || 'name');
  
  const { data: categories } = useQuery({
    queryKey: ['categories'],
    queryFn: mealsService.getCategories,
  });
  
  const handleSearchChange = (value: string) => {
    setSearch(value);
    onFilterChange({ search: value, category_id: categoryId, sort });
  };
  
  const handleCategoryChange = (value: string) => {
    setCategoryId(value);
    onFilterChange({ search, category_id: value, sort });
  };
  
  const handleSortChange = (value: string) => {
    setSort(value);
    onFilterChange({ search, category_id: categoryId, sort: value });
  };
  
  return (
    <div className="bg-white rounded-lg shadow-md p-6 mb-6">
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        <Input
          placeholder="Search meals..."
          value={search}
          onChange={(e) => handleSearchChange(e.target.value)}
        />
        
        <select
          value={categoryId}
          onChange={(e) => handleCategoryChange(e.target.value)}
          className="px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500"
        >
          <option value="">All Categories</option>
          {categories?.map((category) => (
            <option key={category.id} value={category.id}>
              {category.name}
            </option>
          ))}
        </select>
        
        <select
          value={sort}
          onChange={(e) => handleSortChange(e.target.value)}
          className="px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500"
        >
          <option value="name">Sort by Name</option>
          <option value="price">Sort by Price</option>
          <option value="rating">Sort by Rating</option>
          <option value="popular">Most Popular</option>
        </select>
      </div>
    </div>
  );
}

export default MealFilters;
```

#### `src/pages/Meals/index.tsx`
```typescript
import { useState } from 'react';
import { useQuery } from '@tanstack/react-query';
import { useSearchParams } from 'react-router-dom';
import { mealsService } from '@/services/api/meals';
import { MealsFilters } from '@/types';
import MealCard from '@/components/meals/MealCard';
import MealFilters from '@/components/meals/MealFilters';
import LoadingSpinner from '@/components/common/LoadingSpinner';
import EmptyState from '@/components/common/EmptyState';
import Button from '@/components/common/Button';

function Meals() {
  const [searchParams] = useSearchParams();
  const [page, setPage] = useState(1);
  const [filters, setFilters] = useState<MealsFilters>({
    page: 1,
    limit: 12,
    category_id: searchParams.get('category') ? Number(searchParams.get('category')) : undefined,
    search: searchParams.get('search') || undefined,
    sort: (searchParams.get('sort') as any) || 'name',
  });
  
  const { data, isLoading, error } = useQuery({
    queryKey: ['meals', filters],
    queryFn: () => mealsService.getMeals(filters),
  });
  
  const handleFilterChange = (newFilters: Partial<MealsFilters>) => {
    setFilters({ ...filters, ...newFilters, page: 1 });
    setPage(1);
  };
  
  const handlePageChange = (newPage: number) => {
    setPage(newPage);
    setFilters({ ...filters, page: newPage });
  };
  
  if (isLoading) {
    return (
      <div className="flex justify-center py-12">
        <LoadingSpinner size="lg" />
      </div>
    );
  }
  
  if (error) {
    return (
      <div className="container mx-auto px-4 py-12">
        <EmptyState
          title="Failed to load meals"
          description="Please try again later"
          action={{
            label: 'Retry',
            onClick: () => window.location.reload(),
          }}
        />
      </div>
    );
  }
  
  if (!data || data.data.length === 0) {
    return (
      <div className="container mx-auto px-4 py-12">
        <MealFilters onFilterChange={handleFilterChange} />
        <EmptyState
          title="No meals found"
          description="Try adjusting your filters"
        />
      </div>
    );
  }
  
  return (
    <div className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-6">Our Menu</h1>
      
      <MealFilters onFilterChange={handleFilterChange} />
      
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 mb-8">
        {data.data.map((meal) => (
          <MealCard key={meal.id} meal={meal} />
        ))}
      </div>
      
      {/* Pagination */}
      {data.meta.total_pages > 1 && (
        <div className="flex justify-center items-center gap-2">
          <Button
            variant="secondary"
            onClick={() => handlePageChange(page - 1)}
            disabled={page === 1}
          >
            Previous
          </Button>
          
          {Array.from({ length: data.meta.total_pages }, (_, i) => i + 1).map((pageNum) => (
            <Button
              key={pageNum}
              variant={pageNum === page ? 'primary' : 'ghost'}
              onClick={() => handlePageChange(pageNum)}
            >
              {pageNum}
            </Button>
          ))}
          
          <Button
            variant="secondary"
            onClick={() => handlePageChange(page + 1)}
            disabled={page === data.meta.total_pages}
          >
            Next
          </Button>
        </div>
      )}
    </div>
  );
}

export default Meals;
```

### Step 14: Meal Details Page

#### `src/pages/MealDetails/index.tsx`
```typescript
import { useState } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import { useQuery } from '@tanstack/react-query';
import { mealsService } from '@/services/api/meals';
import { useCartStore } from '@/stores/cartStore';
import LoadingSpinner from '@/components/common/LoadingSpinner';
import Button from '@/components/common/Button';
import toast from 'react-hot-toast';

function MealDetails() {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();
  const [quantity, setQuantity] = useState(1);
  const addItem = useCartStore((state) => state.addItem);
  
  const { data: meal, isLoading, error } = useQuery({
    queryKey: ['meal', id],
    queryFn: () => mealsService.getMeal(Number(id)),
    enabled: !!id,
  });
  
  const handleAddToCart = () => {
    if (meal) {
      addItem({
        id: Date.now(),
        meal_id: meal.id,
        quantity,
        meal,
      });
      toast.success(`${meal.name} added to cart`);
      navigate('/cart');
    }
  };
  
  const handleQuantityChange = (delta: number) => {
    setQuantity((prev) => Math.max(1, prev + delta));
  };
  
  if (isLoading) {
    return (
      <div className="flex justify-center py-12">
        <LoadingSpinner size="lg" />
      </div>
    );
  }
  
  if (error || !meal) {
    return (
      <div className="container mx-auto px-4 py-12 text-center text-red-500">
        Failed to load meal details
      </div>
    );
  }
  
  return (
    <div className="container mx-auto px-4 py-8">
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
        {/* Image */}
        <div className="bg-white rounded-lg shadow-md overflow-hidden">
          <img
            src={meal.image}
            alt={meal.name}
            className="w-full h-96 object-cover"
          />
        </div>
        
        {/* Details */}
        <div className="bg-white rounded-lg shadow-md p-6">
          <h1 className="text-3xl font-bold mb-4">{meal.name}</h1>
          
          <div className="flex items-center gap-4 mb-4">
            <div className="flex items-center">
              <svg className="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
                <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
              </svg>
              <span className="ml-1 text-lg font-semibold">{meal.rating}</span>
            </div>
            
            <span className="text-gray-500">•</span>
            
            <span className="text-gray-500">{meal.preparation_time} min</span>
          </div>
          
          <p className="text-gray-600 mb-6">{meal.description}</p>
          
          <div className="mb-6">
            <h3 className="font-semibold mb-2">Ingredients</h3>
            <div className="flex flex-wrap gap-2">
              {meal.ingredients.map((ingredient, index) => (
                <span
                  key={index}
                  className="px-3 py-1 bg-gray-100 rounded-full text-sm"
                >
                  {ingredient}
                </span>
              ))}
            </div>
          </div>
          
          {meal.nutrition && (
            <div className="mb-6">
              <h3 className="font-semibold mb-2">Nutrition Facts</h3>
              <div className="grid grid-cols-4 gap-4">
                <div className="text-center">
                  <div className="text-2xl font-bold text-orange-500">
                    {meal.nutrition.calories}
                  </div>
                  <div className="text-sm text-gray-500">Calories</div>
                </div>
                <div className="text-center">
                  <div className="text-2xl font-bold text-orange-500">
                    {meal.nutrition.protein}g
                  </div>
                  <div className="text-sm text-gray-500">Protein</div>
                </div>
                <div className="text-center">
                  <div className="text-2xl font-bold text-orange-500">
                    {meal.nutrition.carbs}g
                  </div>
                  <div className="text-sm text-gray-500">Carbs</div>
                </div>
                <div className="text-center">
                  <div className="text-2xl font-bold text-orange-500">
                    {meal.nutrition.fat}g
                  </div>
                  <div className="text-sm text-gray-500">Fat</div>
                </div>
              </div>
            </div>
          )}
          
          <div className="border-t pt-6">
            <div className="flex items-center justify-between mb-4">
              <div>
                <span className="text-3xl font-bold text-orange-500">
                  ${meal.price.toFixed(2)}
                </span>
              </div>
              
              <div className="flex items-center gap-2">
                <button
                  onClick={() => handleQuantityChange(-1)}
                  className="w-10 h-10 rounded-lg border-2 border-gray-300 flex items-center justify-center hover:border-orange-500 transition"
                >
                  -
                </button>
                <span className="w-12 text-center text-xl font-semibold">{quantity}</span>
                <button
                  onClick={() => handleQuantityChange(1)}
                  className="w-10 h-10 rounded-lg border-2 border-gray-300 flex items-center justify-center hover:border-orange-500 transition"
                >
                  +
                </button>
              </div>
            </div>
            
            <Button onClick={handleAddToCart} fullWidth size="lg">
              Add to Cart - ${(meal.price * quantity).toFixed(2)}
            </Button>
          </div>
        </div>
      </div>
    </div>
  );
}

export default MealDetails;
```

### Step 15: Cart Page

#### `src/components/cart/CartItem/index.tsx`
```typescript
import { CartItem as CartItemType } from '@/types';
import { useCartStore } from '@/stores/cartStore';

interface CartItemProps {
  item: CartItemType;
}

function CartItem({ item }: CartItemProps) {
  const updateQuantity = useCartStore((state) => state.updateQuantity);
  const removeItem = useCartStore((state) => state.removeItem);
  
  const handleIncrease = () => {
    updateQuantity(item.meal_id, item.quantity + 1);
  };
  
  const handleDecrease = () => {
    if (item.quantity > 1) {
      updateQuantity(item.meal_id, item.quantity - 1);
    } else {
      removeItem(item.meal_id);
    }
  };
  
  const handleRemove = () => {
    removeItem(item.meal_id);
  };
  
  return (
    <div className="flex items-center gap-4 bg-white rounded-lg shadow-md p-4">
      <img
        src={item.meal.image}
        alt={item.meal.name}
        className="w-24 h-24 object-cover rounded-lg"
      />
      
      <div className="flex-1">
        <h3 className="font-semibold text-gray-900">{item.meal.name}</h3>
        <p className="text-orange-500 font-bold">${item.meal.price.toFixed(2)}</p>
      </div>
      
      <div className="flex items-center gap-2">
        <button
          onClick={handleDecrease}
          className="w-8 h-8 rounded-lg border-2 border-gray-300 flex items-center justify-center hover:border-orange-500 transition"
        >
          -
        </button>
        <span className="w-8 text-center font-semibold">{item.quantity}</span>
        <button
          onClick={handleIncrease}
          className="w-8 h-8 rounded-lg border-2 border-gray-300 flex items-center justify-center hover:border-orange-500 transition"
        >
          +
        </button>
      </div>
      
      <div className="text-right">
        <p className="font-bold text-lg">
          ${(item.meal.price * item.quantity).toFixed(2)}
        </p>
        <button
          onClick={handleRemove}
          className="text-red-500 text-sm hover:underline"
        >
          Remove
        </button>
      </div>
    </div>
  );
}

export default CartItem;
```

#### `src/components/cart/CartSummary/index.tsx`
```typescript
import { useCartStore } from '@/stores/cartStore';
import { Link } from 'react-router-dom';

function CartSummary() {
  const items = useCartStore((state) => state.items);
  const getTotal = useCartStore((state) => state.getTotal);
  const clearCart = useCartStore((state) => state.clearCart);
  
  const subtotal = getTotal();
  const tax = subtotal * 0.1; // 10% tax
  const total = subtotal + tax;
  
  return (
    <div className="bg-white rounded-lg shadow-md p-6">
      <h2 className="text-xl font-bold mb-4">Order Summary</h2>
      
      <div className="space-y-2 mb-4">
        <div className="flex justify-between">
          <span className="text-gray-600">Subtotal</span>
          <span className="font-semibold">${subtotal.toFixed(2)}</span>
        </div>
        <div className="flex justify-between">
          <span className="text-gray-600">Tax (10%)</span>
          <span className="font-semibold">${tax.toFixed(2)}</span>
        </div>
        <div className="flex justify-between border-t pt-2">
          <span className="text-lg font-bold">Total</span>
          <span className="text-lg font-bold text-orange-500">${total.toFixed(2)}</span>
        </div>
      </div>
      
      {items.length > 0 && (
        <>
          <Link
            to="/checkout"
            className="block w-full bg-orange-500 text-white text-center py-3 rounded-lg font-semibold hover:bg-orange-600 transition mb-2"
          >
            Proceed to Checkout
          </Link>
          
          <button
            onClick={clearCart}
            className="w-full text-red-500 py-2 hover:underline"
          >
            Clear Cart
          </button>
        </>
      )}
    </div>
  );
}

export default CartSummary;
```

#### `src/pages/Cart/index.tsx`
```typescript
import { useCartStore } from '@/stores/cartStore';
import CartItem from '@/components/cart/CartItem';
import CartSummary from '@/components/cart/CartSummary';
import EmptyState from '@/components/common/EmptyState';
import { Link } from 'react-router-dom';

function Cart() {
  const items = useCartStore((state) => state.items);
  const getItemCount = useCartStore((state) => state.getItemCount);
  
  if (items.length === 0) {
    return (
      <div className="container mx-auto px-4 py-12">
        <EmptyState
          title="Your cart is empty"
          description="Add some delicious meals to get started"
          action={{
            label: 'Browse Meals',
            onClick: () => (window.location.href = '/meals'),
          }}
        />
      </div>
    );
  }
  
  return (
    <div className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-6">Shopping Cart ({getItemCount()} items)</h1>
      
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <div className="lg:col-span-2 space-y-4">
          {items.map((item) => (
            <CartItem key={item.id} item={item} />
          ))}
        </div>
        
        <div>
          <CartSummary />
        </div>
      </div>
    </div>
  );
}

export default Cart;
```

---

## 🚀 Part 3: Checkout + Orders + Refactoring + Performance

### Step 16: Checkout Page

#### `src/pages/Checkout/index.tsx`
```typescript
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { useMutation } from '@tanstack/react-query';
import { useCartStore } from '@/stores/cartStore';
import { useAuthStore } from '@/stores/authStore';
import { ordersService } from '@/services/api/orders';
import { CreateOrderData } from '@/types';
import Input from '@/components/common/Input';
import Button from '@/components/common/Button';
import LoadingSpinner from '@/components/common/LoadingSpinner';
import toast from 'react-hot-toast';

function Checkout() {
  const navigate = useNavigate();
  const user = useAuthStore((state) => state.user);
  const items = useCartStore((state) => state.items);
  const getTotal = useCartStore((state) => state.getTotal);
  const clearCart = useCartStore((state) => state.clearCart);
  
  const [orderData, setOrderData] = useState<CreateOrderData>({
    customer_name: user?.name || '',
    phone: user?.phone || '',
    address: '',
    payment_method: 'cash',
    notes: '',
  });
  
  const [errors, setErrors] = useState<Partial<CreateOrderData>>({});
  
  const createOrderMutation = useMutation({
    mutationFn: ordersService.createOrder,
    onSuccess: (order) => {
      clearCart();
      toast.success('Order placed successfully!');
      navigate(`/orders/${order.id}`);
    },
    onError: (error: any) => {
      toast.error(error.response?.data?.message || 'Failed to place order');
    },
  });
  
  const validate = (): boolean => {
    const newErrors: Partial<CreateOrderData> = {};
    
    if (!orderData.customer_name) {
      newErrors.customer_name = 'Name is required';
    }
    
    if (!orderData.phone) {
      newErrors.phone = 'Phone is required';
    }
    
    if (!orderData.address) {
      newErrors.address = 'Address is required';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    
    if (validate()) {
      createOrderMutation.mutate(orderData);
    }
  };
  
  const subtotal = getTotal();
  const tax = subtotal * 0.1;
  const total = subtotal + tax;
  
  if (items.length === 0) {
    return (
      <div className="container mx-auto px-4 py-12 text-center">
        <h1 className="text-3xl font-bold mb-4">Your cart is empty</h1>
        <button
          onClick={() => navigate('/meals')}
          className="bg-orange-500 text-white px-6 py-3 rounded-lg hover:bg-orange-600"
        >
          Browse Meals
        </button>
      </div>
    );
  }
  
  return (
    <div className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-6">Checkout</h1>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
        {/* Form */}
        <div className="bg-white rounded-lg shadow-md p-6">
          <h2 className="text-xl font-bold mb-4">Delivery Information</h2>
          
          <form onSubmit={handleSubmit} className="space-y-4">
            <Input
              label="Full Name"
              value={orderData.customer_name}
              onChange={(e) => setOrderData({ ...orderData, customer_name: e.target.value })}
              error={errors.customer_name}
            />
            
            <Input
              label="Phone Number"
              type="tel"
              value={orderData.phone}
              onChange={(e) => setOrderData({ ...orderData, phone: e.target.value })}
              error={errors.phone}
            />
            
            <Input
              label="Delivery Address"
              value={orderData.address}
              onChange={(e) => setOrderData({ ...orderData, address: e.target.value })}
              error={errors.address}
            />
            
            <div>
              <label className="block mb-2 font-medium">Payment Method</label>
              <select
                value={orderData.payment_method}
                onChange={(e) => setOrderData({ ...orderData, payment_method: e.target.value as any })}
                className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500"
              >
                <option value="cash">Cash on Delivery</option>
                <option value="card">Credit/Debit Card</option>
              </select>
            </div>
            
            <div>
              <label className="block mb-2 font-medium">Special Instructions (Optional)</label>
              <textarea
                value={orderData.notes}
                onChange={(e) => setOrderData({ ...orderData, notes: e.target.value })}
                className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500"
                rows={3}
                placeholder="Any special requests..."
              />
            </div>
            
            <Button
              type="submit"
              loading={createOrderMutation.isPending}
              fullWidth
              size="lg"
            >
              Place Order - ${total.toFixed(2)}
            </Button>
          </form>
        </div>
        
        {/* Order Summary */}
        <div className="bg-white rounded-lg shadow-md p-6 h-fit">
          <h2 className="text-xl font-bold mb-4">Order Summary</h2>
          
          <div className="space-y-3 mb-4">
            {items.map((item) => (
              <div key={item.id} className="flex justify-between">
                <span>
                  {item.meal.name} x {item.quantity}
                </span>
                <span className="font-semibold">
                  ${(item.meal.price * item.quantity).toFixed(2)}
                </span>
              </div>
            ))}
          </div>
          
          <div className="border-t pt-4 space-y-2">
            <div className="flex justify-between">
              <span className="text-gray-600">Subtotal</span>
              <span className="font-semibold">${subtotal.toFixed(2)}</span>
            </div>
            <div className="flex justify-between">
              <span className="text-gray-600">Tax (10%)</span>
              <span className="font-semibold">${tax.toFixed(2)}</span>
            </div>
            <div className="flex justify-between text-lg font-bold">
              <span>Total</span>
              <span className="text-orange-500">${total.toFixed(2)}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

export default Checkout;
```

### Step 17: Orders Pages

#### `src/components/orders/OrderStatusBadge/index.tsx`
```typescript
interface OrderStatusBadgeProps {
  status: string;
}

function OrderStatusBadge({ status }: OrderStatusBadgeProps) {
  const statusConfig: Record<string, { color: string; label: string }> = {
    pending: { color: 'bg-yellow-100 text-yellow-800', label: 'Pending' },
    preparing: { color: 'bg-blue-100 text-blue-800', label: 'Preparing' },
    ready: { color: 'bg-purple-100 text-purple-800', label: 'Ready' },
    delivered: { color: 'bg-green-100 text-green-800', label: 'Delivered' },
    cancelled: { color: 'bg-red-100 text-red-800', label: 'Cancelled' },
  };
  
  const config = statusConfig[status] || statusConfig.pending;
  
  return (
    <span className={`px-3 py-1 rounded-full text-sm font-medium ${config.color}`}>
      {config.label}
    </span>
  );
}

export default OrderStatusBadge;
```

#### `src/components/orders/OrderCard/index.tsx`
```typescript
import { Link } from 'react-router-dom';
import { Order } from '@/types';
import OrderStatusBadge from './OrderStatusBadge';

interface OrderCardProps {
  order: Order;
}

function OrderCard({ order }: OrderCardProps) {
  return (
    <div className="bg-white rounded-lg shadow-md p-6">
      <div className="flex items-center justify-between mb-4">
        <div>
          <h3 className="font-semibold text-lg">Order #{order.id}</h3>
          <p className="text-sm text-gray-500">
            {new Date(order.created_at).toLocaleDateString()}
          </p>
        </div>
        <OrderStatusBadge status={order.status} />
      </div>
      
      <div className="flex items-center justify-between">
        <div>
          <p className="text-gray-600">
            {order.items.length} item{order.items.length !== 1 ? 's' : ''}
          </p>
        </div>
        <div className="text-right">
          <p className="font-bold text-lg text-orange-500">
            ${order.total.toFixed(2)}
          </p>
          <Link
            to={`/orders/${order.id}`}
            className="text-sm text-orange-500 hover:underline"
          >
            View Details
          </Link>
        </div>
      </div>
    </div>
  );
}

export default OrderCard;
```

#### `src/pages/Orders/index.tsx`
```typescript
import { useState } from 'react';
import { useQuery } from '@tanstack/react-query';
import { ordersService } from '@/services/api/orders';
import OrderCard from '@/components/orders/OrderCard';
import LoadingSpinner from '@/components/common/LoadingSpinner';
import EmptyState from '@/components/common/EmptyState';
import Button from '@/components/common/Button';

function Orders() {
  const [page, setPage] = useState(1);
  
  const { data, isLoading, error } = useQuery({
    queryKey: ['orders', page],
    queryFn: () => ordersService.getOrders(page, 10),
  });
  
  if (isLoading) {
    return (
      <div className="flex justify-center py-12">
        <LoadingSpinner size="lg" />
      </div>
    );
  }
  
  if (error) {
    return (
      <div className="container mx-auto px-4 py-12">
        <EmptyState
          title="Failed to load orders"
          description="Please try again later"
          action={{
            label: 'Retry',
            onClick: () => window.location.reload(),
          }}
        />
      </div>
    );
  }
  
  if (!data || data.data.length === 0) {
    return (
      <div className="container mx-auto px-4 py-12">
        <EmptyState
          title="No orders yet"
          description="Start ordering to see your orders here"
          action={{
            label: 'Browse Meals',
            onClick: () => (window.location.href = '/meals'),
          }}
        />
      </div>
    );
  }
  
  return (
    <div className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-6">My Orders</h1>
      
      <div className="space-y-4 mb-8">
        {data.data.map((order) => (
          <OrderCard key={order.id} order={order} />
        ))}
      </div>
      
      {/* Pagination */}
      {data.meta.total_pages > 1 && (
        <div className="flex justify-center items-center gap-2">
          <Button
            variant="secondary"
            onClick={() => setPage(page - 1)}
            disabled={page === 1}
          >
            Previous
          </Button>
          
          {Array.from({ length: data.meta.total_pages }, (_, i) => i + 1).map((pageNum) => (
            <Button
              key={pageNum}
              variant={pageNum === page ? 'primary' : 'ghost'}
              onClick={() => setPage(pageNum)}
            >
              {pageNum}
            </Button>
          ))}
          
          <Button
            variant="secondary"
            onClick={() => setPage(page + 1)}
            disabled={page === data.meta.total_pages}
          >
            Next
          </Button>
        </div>
      )}
    </div>
  );
}

export default Orders;
```

#### `src/pages/OrderDetails/index.tsx`
```typescript
import { useParams } from 'react-router-dom';
import { useQuery } from '@tanstack/react-query';
import { ordersService } from '@/services/api/orders';
import OrderStatusBadge from '@/components/orders/OrderStatusBadge';
import LoadingSpinner from '@/components/common/LoadingSpinner';
import Button from '@/components/common/Button';

function OrderDetails() {
  const { id } = useParams<{ id: string }>();
  
  const { data: order, isLoading, error } = useQuery({
    queryKey: ['order', id],
    queryFn: () => ordersService.getOrder(Number(id)),
    enabled: !!id,
  });
  
  if (isLoading) {
    return (
      <div className="flex justify-center py-12">
        <LoadingSpinner size="lg" />
      </div>
    );
  }
  
  if (error || !order) {
    return (
      <div className="container mx-auto px-4 py-12 text-center text-red-500">
        Failed to load order details
      </div>
    );
  }
  
  return (
    <div className="container mx-auto px-4 py-8">
      <div className="mb-6">
        <Button variant="secondary" onClick={() => window.history.back()}>
          ← Back to Orders
        </Button>
      </div>
      
      <div className="bg-white rounded-lg shadow-md p-6 mb-6">
        <div className="flex items-center justify-between mb-4">
          <h1 className="text-2xl font-bold">Order #{order.id}</h1>
          <OrderStatusBadge status={order.status} />
        </div>
        
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm">
          <div>
            <p className="text-gray-500">Customer</p>
            <p className="font-semibold">{order.customer_name}</p>
          </div>
          <div>
            <p className="text-gray-500">Phone</p>
            <p className="font-semibold">{order.phone}</p>
          </div>
          <div>
            <p className="text-gray-500">Address</p>
            <p className="font-semibold">{order.address}</p>
          </div>
          <div>
            <p className="text-gray-500">Payment Method</p>
            <p className="font-semibold capitalize">{order.payment_method}</p>
          </div>
          <div>
            <p className="text-gray-500">Order Date</p>
            <p className="font-semibold">
              {new Date(order.created_at).toLocaleString()}
            </p>
          </div>
          {order.estimated_delivery && (
            <div>
              <p className="text-gray-500">Estimated Delivery</p>
              <p className="font-semibold">
                {new Date(order.estimated_delivery).toLocaleString()}
              </p>
            </div>
          )}
        </div>
      </div>
      
      <div className="bg-white rounded-lg shadow-md p-6 mb-6">
        <h2 className="text-xl font-bold mb-4">Order Items</h2>
        
        <div className="space-y-3">
          {order.items.map((item, index) => (
            <div key={index} className="flex justify-between py-2 border-b">
              <div>
                <p className="font-semibold">{item.meal_name}</p>
                <p className="text-sm text-gray-500">Qty: {item.quantity}</p>
              </div>
              <p className="font-bold">
                ${(item.price * item.quantity).toFixed(2)}
              </p>
            </div>
          ))}
        </div>
        
        <div className="border-t mt-4 pt-4">
          <div className="flex justify-between text-lg font-bold">
            <span>Total</span>
            <span className="text-orange-500">${order.total.toFixed(2)}</span>
          </div>
        </div>
      </div>
      
      {order.notes && (
        <div className="bg-white rounded-lg shadow-md p-6">
          <h2 className="text-xl font-bold mb-4">Special Instructions</h2>
          <p className="text-gray-600">{order.notes}</p>
        </div>
      )}
    </div>
  );
}

export default OrderDetails;
```

### Step 18: Profile Page

#### `src/pages/Profile/index.tsx`
```typescript
import { useState } from 'react';
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { useAuthStore } from '@/stores/authStore';
import { authService } from '@/services/api/auth';
import { User } from '@/types';
import Input from '@/components/common/Input';
import Button from '@/components/common/Button';
import LoadingSpinner from '@/components/common/LoadingSpinner';
import toast from 'react-hot-toast';

function Profile() {
  const queryClient = useQueryClient();
  const user = useAuthStore((state) => state.user);
  const updateUser = useAuthStore((state) => state.updateUser);
  
  const [formData, setFormData] = useState<Partial<User>>({
    name: user?.name || '',
    phone: user?.phone || '',
  });
  
  const [errors, setErrors] = useState<Partial<User>>({});
  
  const { data: profile, isLoading } = useQuery({
    queryKey: ['profile'],
    queryFn: authService.getProfile,
  });
  
  const updateMutation = useMutation({
    mutationFn: authService.updateProfile,
    onSuccess: (updatedUser) => {
      updateUser(updatedUser);
      toast.success('Profile updated successfully');
      queryClient.invalidateQueries({ queryKey: ['profile'] });
    },
    onError: (error: any) => {
      toast.error(error.response?.data?.message || 'Failed to update profile');
    },
  });
  
  const validate = (): boolean => {
    const newErrors: Partial<User> = {};
    
    if (!formData.name) {
      newErrors.name = 'Name is required';
    }
    
    if (!formData.phone) {
      newErrors.phone = 'Phone is required';
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    
    if (validate()) {
      updateMutation.mutate(formData);
    }
  };
  
  if (isLoading) {
    return (
      <div className="flex justify-center py-12">
        <LoadingSpinner size="lg" />
      </div>
    );
  }
  
  return (
    <div className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-6">My Profile</h1>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
        {/* Profile Form */}
        <div className="bg-white rounded-lg shadow-md p-6">
          <h2 className="text-xl font-bold mb-4">Edit Profile</h2>
          
          <form onSubmit={handleSubmit} className="space-y-4">
            <Input
              label="Name"
              value={formData.name}
              onChange={(e) => setFormData({ ...formData, name: e.target.value })}
              error={errors.name}
            />
            
            <Input
              label="Email"
              value={user?.email}
              disabled
              helperText="Email cannot be changed"
            />
            
            <Input
              label="Phone"
              type="tel"
              value={formData.phone}
              onChange={(e) => setFormData({ ...formData, phone: e.target.value })}
              error={errors.phone}
            />
            
            <Button
              type="submit"
              loading={updateMutation.isPending}
              fullWidth
            >
              Update Profile
            </Button>
          </form>
        </div>
        
        {/* Account Info */}
        <div className="bg-white rounded-lg shadow-md p-6">
          <h2 className="text-xl font-bold mb-4">Account Information</h2>
          
          <div className="space-y-4">
            <div>
              <p className="text-gray-500 text-sm">Account ID</p>
              <p className="font-semibold">{user?.id}</p>
            </div>
            
            <div>
              <p className="text-gray-500 text-sm">Email</p>
              <p className="font-semibold">{user?.email}</p>
            </div>
            
            <div>
              <p className="text-gray-500 text-sm">Member Since</p>
              <p className="font-semibold">
                {user?.created_at ? new Date(user.created_at).toLocaleDateString() : 'N/A'}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

export default Profile;
```

### Step 19: NotFound Page

#### `src/pages/NotFound/index.tsx`
```typescript
import { Link } from 'react-router-dom';
import Button from '@/components/common/Button';

function NotFound() {
  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-100">
      <div className="text-center">
        <h1 className="text-9xl font-bold text-orange-500 mb-4">404</h1>
        <h2 className="text-2xl font-bold text-gray-900 mb-4">Page Not Found</h2>
        <p className="text-gray-600 mb-8">
          The page you're looking for doesn't exist or has been moved.
        </p>
        <Link to="/">
          <Button>Go Home</Button>
        </Link>
      </div>
    </div>
  );
}

export default NotFound;
```

### Step 20: Performance Optimization

#### React.memo for Components

```typescript
// إضافة React.memo للمكونات التي تعيد render كثيراً
import { memo } from 'react';

const MealCard = memo(function MealCard({ meal }: MealCardProps) {
  // ... component code
});

const CartItem = memo(function CartItem({ item }: CartItemProps) {
  // ... component code
});
```

#### useMemo for Computed Values

```typescript
import { useMemo } from 'react';

function Cart() {
  const items = useCartStore((state) => state.items);
  
  const subtotal = useMemo(() => {
    return items.reduce((sum, item) => sum + item.meal.price * item.quantity, 0);
  }, [items]);
  
  const tax = useMemo(() => subtotal * 0.1, [subtotal]);
  const total = useMemo(() => subtotal + tax, [subtotal, tax]);
  
  // ...
}
```

#### useCallback for Event Handlers

```typescript
import { useCallback } from 'react';

function MealCard({ meal }: MealCardProps) {
  const addItem = useCartStore((state) => state.addItem);
  
  const handleAddToCart = useCallback(() => {
    addItem({
      id: Date.now(),
      meal_id: meal.id,
      quantity: 1,
      meal,
    });
  }, [meal, addItem]);
  
  // ...
}
```

#### Lazy Loading for Pages

```typescript
import { lazy, Suspense } from 'react';
import LoadingSpinner from '@/components/common/LoadingSpinner';

const Checkout = lazy(() => import('@/pages/Checkout'));
const Orders = lazy(() => import('@/pages/Orders'));
const OrderDetails = lazy(() => import('@/pages/OrderDetails'));
const Profile = lazy(() => import('@/pages/Profile'));

// في router
{
  path: 'checkout',
  element: (
    <Suspense fallback={<LoadingSpinner />}>
      <Checkout />
    </Suspense>
  ),
}
```

---

## ✅ Production Checklist

### Security
- [ ] HTTPS enabled
- [ ] Environment variables secured
- [ ] API keys server-side only
- [ ] CORS configured
- [ ] Rate limiting
- [ ] Input validation
- [ ] XSS prevention
- [ ] CSRF protection
- [ ] Secure headers

### Performance
- [ ] Bundle size analyzed
- [ ] Code splitting implemented
- [ ] Lazy loading for routes
- [ ] Images optimized
- [ ] Caching strategy
- [ ] CDN configured
- [ ] Gzip/Brotli compression
- [ ] Minification enabled
- [ ] Tree shaking working

### SEO
- [ ] Meta tags configured
- [ ] Open Graph tags
- [ ] Sitemap.xml
- [ ] Robots.txt
- [ ] Semantic HTML
- [ ] Alt text for images

### Accessibility
- [ ] ARIA labels
- [ ] Keyboard navigation
- [ ] Screen reader compatible
- [ ] Color contrast (WCAG AA)
- [ ] Focus indicators

### Testing
- [ ] Unit tests written
- [ ] Integration tests written
- [ ] E2E tests written
- [ ] Test coverage > 80%
- [ ] Manual testing completed

### Monitoring
- [ ] Error tracking (Sentry)
- [ ] Analytics (Google Analytics)
- [ ] Performance monitoring
- [ ] Uptime monitoring
- [ ] Logging configured

---

## 🔍 Code Review Checklist

### Code Quality
- [ ] Code follows project conventions
- [ ] No console.logs in production
- [ ] No debug code
- [ ] Proper error handling
- [ ] Loading states implemented
- [ ] Empty states implemented
- [ ] TypeScript strict mode
- [ ] No any types (specific types used)
- [ ] Proper component naming
- [ ] Meaningful variable names

### Architecture
- [ ] Separation of concerns
- [ ] Proper folder structure
- [ ] Reusable components
- [ ] No code duplication
- [ ] Proper state management
- [ ] Clean API layer
- [ ] Proper routing structure

### Performance
- [ ] No unnecessary re-renders
- [ ] Proper memoization
- [ ] Efficient algorithms
- [ ] Optimized images
- [ ] Lazy loading implemented
- [ ] Bundle size optimized

### Security
- [ ] No sensitive data in frontend
- [ ] Proper input validation
- [ ] XSS prevention
- [ ] CSRF protection
- [ ] Secure API calls

---

## 🔒 Security Checklist

### Authentication
- [ ] JWT tokens properly stored
- [ ] Token refresh implemented
- [ ] Secure logout (clear tokens)
- [ ] Protected routes working
- [ ] Session timeout

### API Security
- [ ] HTTPS only in production
- [ ] API keys not exposed
- [ ] Rate limiting
- [ ] Input sanitization
- [ ] SQL injection prevention
- [ ] CORS properly configured

### Data Security
- [ ] No sensitive data in localStorage
- [ ] Proper encryption
- [ ] Secure password handling
- [ ] Data validation
- [ ] Error messages don't leak info

---

## ⚡ Performance Checklist

### Initial Load
- [ ] First paint < 1.5s
- [ ] First contentful paint < 2s
- [ ] Time to interactive < 3.5s
- [ ] Bundle size < 500KB (gzipped)

### Runtime Performance
- [ ] No layout shifts
- [ ] Smooth animations (60fps)
- [ ] Fast API responses
- [ ] Efficient re-renders
- [ ] Memory leaks fixed

### Optimization
- [ ] Code splitting implemented
- [ ] Lazy loading working
- [ ] Images optimized
- [ ] Caching configured
- [ ] CDN working
- [ ] Gzip/Brotli enabled

---

## ❓ 10 Interview Questions

### أسئلة تقنية

1. **كيف اخترت التقنيات لهذا المشروع؟**
   - React: Component-based, large ecosystem
   - TypeScript: Type safety, للـ large codebases
   - Zustand: Simple, small, no boilerplate
   - React Query: Server state management
   - Vite: Fast HMR, modern build tool

2. **كيف تتعامل مع state في هذا المشروع؟**
   - Server State: React Query (API data)
   - Client State: Zustand (UI, preferences)
   - Local State: useState (component-level)
   - Context: Theme, language, notifications

3. **كيف تحسن أداء التطبيق؟**
   - React.memo للمكونات
   - useMemo للحسابات
   - useCallback للـ functions
   - Lazy loading للـ routes
   - Code splitting
   - Bundle optimization

4. **كيف تتعامل مع errors؟**
   - Error boundaries
   - Try-catch في async functions
   - Error states في UI
   - Toast notifications
   - Logging للـ errors

5. **كيف تضمن security؟**
   - JWT tokens
   - Protected routes
   - Input validation
   - HTTPS
   - Environment variables
   - CORS

6. **كيف تنظم الـ code؟**
   - Folder structure واضح
   - Separation of concerns
   - Reusable components
   - Clean architecture
   - Proper naming conventions

7. **كيف تختبر التطبيق؟**
   - Unit tests للـ components
   - Integration tests للـ features
   - E2E tests للـ user flows
   - Testing Library لـ React
   - Jest لـ unit tests

8. **كيف تتعامل مع forms؟**
   - Controlled components
   - Validation
   - Error handling
   - Loading states
   - TypeScript types

9. **كيف تدير الـ API calls؟**
   - Axios للـ HTTP requests
   - Interceptors للـ tokens
   - React Query للـ data fetching
   - Error handling مركزي
   - Retry logic

10. **كيف تحضر التطبيق للـ production؟**
    - Environment variables
    - Build optimization
    - Security checks
    - Performance testing
    - Deployment pipeline

---

## 🎯 10 Project Questions

### أسئلة المشروع

1. **ما هي أكبر تحدي واجهته في هذا المشروع؟**
   - إدارة state بين React Query و Zustand
   - معرفة متى تستخدم كل أداة
   - الحفاظ على consistency في الـ code

2. **كيف تحسّن هذا المشروع مستقبلاً؟**
   - إضافة real-time updates (WebSocket)
   - PWA capabilities
   - Offline support
   - Payment integration (Stripe)
   - Reviews و ratings

3. **ما الذي كنت ستفعله بشكل مختلف؟**
   - استخدام RTK Query بدلاً من React Query
   - إضافة server-side rendering
   - استخدام monorepo structure
   - إضافة testing من البداية

4. **كيف تشرح الـ architecture للمشروع؟**
   - Layered architecture
   - Separation of concerns
   - Clean architecture principles
   - SOLID principles

5. **ما هي أفضل ممارسات اتبعتها؟**
   - TypeScript strict mode
   - Proper error handling
   - Clean code principles
   - Reusable components
   - Proper state management

6. **كيف تضمن code quality؟**
   - ESLint و Prettier
   - Code reviews
   - Testing
   - CI/CD
   - Documentation

7. **ما هي performance metrics التي تتابعها؟**
   - Bundle size
   - Load time
   - Time to interactive
   - First contentful paint
   - API response time

8. **كيف تتعامل مع scalability؟**
   - Code splitting
   - Lazy loading
   - Efficient state management
   - Optimized API calls
   - Caching strategies

9. **ما هي security measures التي طبقتها؟**
   - JWT authentication
   - Protected routes
   - Input validation
   - HTTPS
   - Environment variables

10. **كيف تدير dependencies؟**
    - Package.json و lock files
    - Regular updates
    - Security audits
    - Tree shaking
    - Bundle analysis

---

## 📝 Final Assignment

### المهمة النهائية

أكمل المشروع بإضافة الميزات التالية:

1. **Real-time Updates**
   - إضافة WebSocket support
   - تحديثات live للـ order status
   - Notifications للـ updates

2. **Reviews & Ratings**
   - إضافة نظام reviews
   - ratings للـ meals
   - reviews من المستخدمين

3. **Advanced Search**
   - Full-text search
   - Filters متقدمة
   - Sort options إضافية

4. **Payment Integration**
   - Stripe integration
   - Payment methods متعددة
   -Receipt generation

5. **Admin Panel**
   - إدارة meals
   - إدارة orders
   - Analytics dashboard

6. **Mobile App**
   - React Native version
   - Push notifications
   - Offline support

7. **PWA**
   - Service worker
   - Offline support
   - App-like experience

8. **Testing**
   - Unit tests
   - Integration tests
   - E2E tests
   - 80% coverage

9. **Documentation**
   - API documentation
   - Component documentation
   - Deployment guide
   - Contributing guide

10. **Deployment**
    - CI/CD pipeline
    - Automated testing
    - Staging environment
    - Production deployment

---

## 💡 أفكار لتطوير المشروع

### Ideas for Post-Course Development

1. **Social Features**
   - Share orders on social media
   - Follow favorite restaurants
   - Recommendations based on history

2. **Loyalty Program**
   - Points system
   - Rewards
   - Discounts
   - VIP tiers

3. **Delivery Tracking**
   - GPS tracking
   - Real-time driver location
   - ETA updates
   - Driver communication

4. **Multi-language Support**
   - i18n implementation
   - RTL support
   - Currency conversion
   - Local payment methods

5. **AI Recommendations**
   - Machine learning recommendations
   - Personalized suggestions
   - Trending meals
   - Popular combinations

6. **Advanced Analytics**
   - User behavior tracking
   - Sales analytics
   - Restaurant performance
   - Delivery optimization

7. **Catering Services**
   - Bulk orders
   - Event planning
   - Custom menus
   - Scheduled deliveries

8. **Subscription Service**
   - Weekly meal plans
   - Auto-renewal
   - Custom schedules
   - Family plans

9. **Gift Cards**
   - Digital gift cards
   - Physical cards
   - Balance tracking
   - Gifting options

10. **Restaurant Partnerships**
    - Partner onboarding
    - Restaurant dashboard
    - Menu management
    - Performance analytics

---

## 🎓 الخاتمة

### ملخص الجلسة

في هذه الجلسة العاشرة والأخيرة:

1. ✅ **Design المشروع الكامل**: Food Ordering Application
2. ✅ **Planning شامل**: Requirements, User Stories, Architecture
3. ✅ **Implementation كامل**: من Setup إلى Deployment
4. ✅ **Best Practices**: Clean Code, Performance, Security
5. ✅ **Production Ready**: Checklists للإطلاق

### ما تعلمناه

- **Architecture**: Layered architecture مع separation of concerns
- **State Management**: React Query + Zustand + useState
- **Performance**: Optimization techniques
- **Security**: Authentication, authorization, data protection
- **Testing**: Strategies للـ testing
- **Deployment**: Production preparation

### الخطوات القادمة

1. أكمل الميزات المتبقية
2. أضف testing شامل
3. Deploy إلى production
4. Monitor performance
5. Collect user feedback
6. Iterate و improve

### موارد إضافية

- [React Documentation](https://react.dev)
- [TypeScript Documentation](https://www.typescriptlang.org)
- [React Query Documentation](https://tanstack.com/query/latest)
- [Zustand Documentation](https://zustand-demo.pmnd.rs)
- [Vite Documentation](https://vitejs.dev)

---

**تهانينا! 🎉 لقد أكملت كورس React بنجاح**

**أنت الآن جاهز لبناء تطبيقات React production-ready!**

**Good luck with your future projects! 🚀**