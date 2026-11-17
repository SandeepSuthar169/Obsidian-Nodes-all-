### ~={red}React=~ = UI Library, ~={orange}Next.js =~ =  Full Framework that uses React and handles routing, data, and deployment. 

#### ~={orange}Next.js =~is a ~={red}React=~ Framework for building full-stack web application 

 ###### - It runs on top of React and adds:
1. Routing
2. Rendering
3. API Routing
4. Build and Dev tooling 

---
![[Screenshot 2026-07-20 112505.png]]


---

### 1. App Router Routing 
- The App Router uses a folder-based file `system` where folders define routes and special files  Ex ~={red}`page.js`=~ and ~={red}`layout.ts`=~

#### ~={blue}`public/`=~ 
- Static assets (images, icons, etc) served at the root. Example: public.logo.png
#### ~={blue}`pacage.json`=~
-  Dependencies and scripts (Ex . bun run dev, bun run build ).
#### ~={blue}`next.config.mjs`=~
- Next configuration (redirects, env, etc.).
#### ~={blue}`app/`=~
- your app's UI and routes. This is where pages, layouts, and routes logic here.
----
## ~={3}Types of Routes in Next.js =~

#### ~={4}1. Static Routes=~
- A static route has fixed URL.  The folder name directly becomes the URL path.

```tsx
// app/about/page.tsx                 [yoursite.com/about]
export default function AboutPage() {
  return (
    <main>
      <h1>About Us</h1>
      <p>This is a static route.</p>
    </main>
  );
}
```


#### ~={4}2.  Dynamic Routes=~
- A dynamic route uses square brackets  **~={3}`[ parms ]`=~**  to create a route form dynamic data.

```tsx
// app/users/[id]/page.tsx           [yoursite.com/users/123]
interface UserPageProps {
  params: Promise<{
	  id: string | numbers
  }> 
}

export default const  UserProfile = async ({ params }: UserPageProps) =>  {
	const { id } = await params
  return (
    <main>
      <h1>User Profile: { id }</h1>
    </main>
  );
}
```


#### ~={4}3.  Catch-all Routes =~
- Advance version of dynamic routes
- For nested documentation, file browsers, and deep category filters.  
~={3} `app/docs/[...slug]/page.tsx`=~ maps to ~={3}`/docs/getting-started`=~ OR ~={3}`/docs/api/v1/auth`=~.

```tsx
// File path: app/shop/[...slug]/page.tsx

import React from 'react';

interface ShopCatchAllProps {
  params: Promise<{
        slug: string[];  
    }>;
}

const ShopCatchAllPage = async ({ params }: ShopCatchAllProps) =>  {

  const { slug } = await params;
  
  console.log(slug);
  
    <main style={{ padding: '2rem' }}>
        <h1>post:- {slug}</h1>
    </main>
  );
}

export default ShopCatchAllPage
```


#### ~={4}4. Route Groups=~
- Organize routes without affecting the URL. 
-~={3} `app/(marketing)/about/page.tsx`=~ still maps to~={3} `[yoursite.com/about ` =~       ~={3}`(https://yoursite.com/about)`=~.

```tsx
// app/(marketing)/layout.tsx
// This layout only applies to pages inside the (marketing) folder
export default function MarketingLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <div>
      <nav>Marketing Navbar (No Dashboard Links)</nav>
      {children}
    </div>
  );
}
```


#### ~={4}5. Parallel Routes =~
- Render multiple pages at the same time inside 
	- ~={3}`app/dashboard/layout.tsx`=~
	- ~={3}`app/dashboard/@team/page.tsx`=~
	- ~={3}`app/dashboard/@analytics/page.tsx`=~

```tsx
// app/dashboard/layout.tsx
interface DashboardLayoutProps {
  children: React.ReactNode; // The default page.tsx
  team: React.ReactNode;     // the @team parallel route
  analytics: React.ReactNode; // the @analytics parallel route
}

export default function DashboardLayout({ children, team, analytics }: DashboardLayoutProps) {
  return (
    <div className="layout">
      {/* Main dashboard content */}
      <main>{children}</main> 
      
      {/* Parallel sections rendered alongside children */}
      <aside>{team}</aside>
      <aside>{analytics}</aside>
    </div>
  );
}
```


#### ~={4}6. Nested Dynamic Routes=~
- Nested dynamic routes allows to create take further nested dynamic routs  means folder inside the folder or files 
```css
app/
└── shop/
    └── [category]/
        └── [productId]/
            └── page.tsx

```

```tsx
// File path: app/shop/[category]/[productId]/page.tsx

interface ShopPageProps {
  params: Promise<{
    category: string;
    productId: string;
  }>;
}

const async  ShopPage = ({ params }: ShopPageProps) => {

  const { category, productId } = await params;

  return (
    <main>      
      <ul>
        <li><strong>Category:</strong> {category}</li>
        <li><strong>Product ID:</strong> {productId}</li>
      </ul>
    </main>
  );
}
```

### 2. Pages Router Routing ( Traditional Approach )

--- 
