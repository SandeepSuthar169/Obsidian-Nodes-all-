### ~={1}Server Components (Default in Next.js App Router)=~
1. By default, every components is a ~={4}Server Component=~ unless you mark it as a ~={4}client component =~
2. ~={4}React Server Components (RSCs)=~ are components that run only on the server their code never goes to the browser.  They produce UI on the server and send the result down to the client. 
3. Server Components don't run  in the browser.
4. Do not send their ~={4}JavaScript=~ to the client.
5. They cannot use React Hooks like~={3} `useState`, `useEffect`=~, to ~={3} `Browser APIs ( window, LocalStorage )`=~.
6. They can run async/await directly in the component body. 

```tsx
export default const ServerComponent = async () => {
	const data = await fectchData();
	return( 
		<div>
			{data}
		</div>
		)
}
```

---
#### ~={1}Advantages of Server Components=~
1. Fast Initial load -------> Static part of the UI
2. Better Performance & SEO;
3. More Secure
4. Built for Data Fetching

#### ~={1}Limitation of Server Components =~
1. Can't handle UI Interactivity or browser APIs
2. Can't use React Hooks 

---

### Client Components. 
#####  A  Client components is a component that runs in the ~={3}`browser ( client side )`=~  and support interactivity, state, and browser APIs you OTP into it by putting "use client" at the top of the file. 

#### ~={red}Use Client Components when you need:=~
- ~={3}`Static`=~ and ~={3}`Event handlers`=~ Ex: onClick, onChange.
- ~={3}`Lifecycle logic`=~ Ex useEffect.
- Browser-only APIs Ex ~={3}`localStorage`,  `Window`, `Navigator`, `geolacation`=~, etc.
-  ~={3}`Custome hooks` =~
#### ~={red}When you must use Client Components=~
1. Interactive UI ( Button, Modals, Forms )
2. Components relying on Client State
3. Ant Logic that needs to run in the browser. 

---

| Criteria         | Server Components | Client Components |
| ---------------- | ----------------- | ----------------- |
| Interactivity    | No                | yes               |
| User Hooks       | No                | yes               |
| Browser APIs     | No                | yes               |
| Data Fetching    | Best              | Sometimes         |
| JS Bundle Size   | Smaller           | Larger            |
| SEO & Performace | Better            | Hybration         |
| Security         | Safe              | Need Caution      |

---

### Next.js Hooks 

~={orange}`useParams()`=~ is a client-side hook in the Next.js App Router ( ~={orange}`app/`=~ directory ) that lets you access dynamic route parameters from the current URL.  

![[Screenshot 2026-07-24 225712.png]]

```css
app/
 ├── products/
 │    └── [id]/
 │         └── page.tsx
```

```tsx
"use client";

import { useParams } from "next/navigation";

type Params = {
  id: string;
};

export default function Product() {
  const { id } = useParams<Params>();

  return <h1>{id}</h1>;
}
```

Output
```css
Product ID: 25
```

---
###  Multiple Dynamic Routes

```css
app/
 └── blog/
      └── [category]/
           └── [slug]/
                └── page.tsx
```

