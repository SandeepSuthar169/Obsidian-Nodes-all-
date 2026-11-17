### ~={red}What is ~={3}`< Script />`=~ in Next.js

- ~={3}`<Script />`=~ is a component from next/script used to load third-party JavaScript efficiently in a Next.js application. 
- It is an optimized version of the normal HTML ~={3}`<script>`=~ tag that you that lets you control when and how script are loaded to improve performance.

#### ~={red}Why Next.js Provides=~ ~={3}`<Script />`=~.
- Optimize page performance
- Prevent scripts from blocking rendering
- Control loading order
- Run code after a script has loaded

This is especially useful for:
- Google Analytics
- Google Maps
- Stripe
- Chat widgets
- Facebook Pixel
- Improving performance and Core Web Vitals

```tsx
import Script from "next/script";

export default function Home() {
  return (
    <>
      <Script src="https://example.com/script.js" />
      <h1>Hello, Next.js!</h1>
    </>
  );
}	 
```


----

