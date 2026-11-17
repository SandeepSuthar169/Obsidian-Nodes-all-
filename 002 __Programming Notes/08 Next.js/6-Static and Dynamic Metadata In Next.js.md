### ~={red}**Metadata** =~
- is used to define information about your pages, such as the page title, description, Open Graph tags, Twitter cards, icons, and more. Next.js provides two ways to define metadata:
1.  ~={orange}**Static Metadata**=~ – Fixed at build time.
2.  ~={orange}**Dynamic Metadata**=~ – Generated at request time or based on fetched data.

---
### ~={3}1. Static Metadata=~

Static metadata is used when your page metadata **doesn't change**.
You export a `metadata` object from `page.tsx` or `layout.tsx`.

```tsx
// app/about/page.tsx

import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "About Us",
  description: "Learn more about our company.",
};

export default function AboutPage() {
  return <h1>About Page</h1>;
}
```

#### Generated HTML
```tsx
<title>About Us</title>

<meta
  name="description"
  content="Learn more about our company."
/>
```

##### When to use
- Home page
- About page
- Contact page
- Privacy Policy
- Terms & Conditions
---
### ~={3}2. Dynamic Metadata=~

Sometimes metadata depends on:
- Database
- API
- Product ID
- Blog slug
- User profile
In that case, use **`generateMetadata()`**.

```tsx
import type { Metadata } from "next";

type Props = {
  params: Promise<{
    slug: string;
  }>;
};

export async function generateMetadata(
  { params }: Props
): Promise<Metadata> {
  const { slug } = await params;

  return {
    title: slug,
    description: `Article about ${slug}`,
  };
}

export default async function BlogPage({ params }: Props) {
  const { slug } = await params;

  return <h1>{slug}</h1>;
}
```

#### Dynamic Metadata with Database/API
```tsx
import type { Metadata } from "next";

async function getProduct(id: string) {
  const res = await fetch(`https://api.example.com/products/${id}`);
  return res.json();
}

type Props = {
  params: Promise<{
    id: string;
  }>;
};

export async function generateMetadata(
  { params }: Props
): Promise<Metadata> {
  const { id } = await params;

  const product = await getProduct(id);

  return {
    title: product.name,
    description: product.description,
  };
}

export default async function ProductPage({ params }: Props) {
  const { id } = await params;

  const product = await getProduct(id);

  return <h1>{product.name}</h1>;
}
```

---

Static vs Dynamic Metadata

|  ~={3}Feature=~   |  ~={3}Static Metadata=~  | ~={3}Dynamic Metadata=~ |
| :---------------: | :----------------------: | :---------------------: |
|   Defined using   | ~={1}`metadata` =~object |   generateMetadata()    |
|       Runs        |        Build time        |   Request/render time   |
|       Async       |           ❌ No           |          ✅ Yes          |
|  Can fetch data   |           ❌ No           |          ✅ Yes          |
| Uses route params |           ❌ No           |          ✅ Yes          |
|        SEO        |       ✅ Excellent        |       ✅ Excellent       |
|     Best for      |       Fixed pages        |      Dynamic pages      |

---
~={green}app/layout.tsx=~

```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: {
    default: "My Store",
    template: "%s | My Store",
  },
  description: "Welcome to My Store",
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

~={green}app/about/page.tsx=~

```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "About",
};

export default function AboutPage() {
  return <h1>About</h1>;
}

// About | My Store
```

 ~={green}`app/products/[id]/page.tsx`=~

```tsx
import type { Metadata } from "next";

type Props = {
  params: Promise<{ id: string }>;
};

export async function generateMetadata(
  { params }: Props
): Promise<Metadata> {
  const { id } = await params;

  return {
    title: `Product ${id}`,
  };
}

export default async function ProductPage({ params }: Props) {
  const { id } = await params;

  return <h1>Product {id}</h1>;
}

//       If the URL is `/products/10`, the page title becomes:
```