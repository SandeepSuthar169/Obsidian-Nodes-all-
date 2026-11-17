## Prisma + Better Auth 

1. ~={2}src/app/(auth)/login/page.tsx=~
```tsx
import { LoginForm } from '@/components/login-form'
import { auth } from '@/lib/auth'
import { headers } from 'next/headers'
import { redirect } from 'next/navigation'
import React from 'react'

const LoginPage = async () => {
  const session = await auth.api.getSession({
    headers: await headers()
  })
  if(session){
    redirect("/")
  }
  return (
    <div className="flex min-h-svh flex-col items-center justify-center gap-6 bg-background p-6 md:p-10">
      <div className="w-full max-w-sm">
        <LoginForm />
      </div>
    </div>
  )
}
export default LoginPage
```

2. ~={2}src/app/(auth)/layout.tsx=~
```tsx
import { requireUnAuth } from '@/lib/auth-guard'
import React from 'react'

const AuthLayout = async ({children}: {children: React.ReactNode}) => {
  await requireUnAuth()
  return (
    <div
        className='flex flex-col justify-center items-center gap-1 h-screen'
    >{children}</div>
  )
}
export default AuthLayout
```

3. ~={2}src/app/api/[...all]/route.ts=~
```ts
import { auth } from "@/lib/auth"; // path to your auth file
import { toNextJsHandler } from "better-auth/next-js";

export const { POST, GET } = toNextJsHandler(auth);
```

4. ~={2}src/app/page.tsx =~
```tsx             // home page
import { Button } from "@/components/ui/button";
import { requireAuth } from "@/lib/auth-guard";
import Image from "next/image";

async function Home() {
  const session = await requireAuth()
  const {user} = session

  return (
    <div className="app-container">
      <h1>Hello world</h1>
    </div>
  );
}
export default Home 
```


5. ~={2}src/app/page.tsx=~
```tsx

import { authClient } from "@/lib/auth-client"
const handleGithubLogin = async () => {
    await authClient.signIn.social({
        provider: "github",
        callbackURL: "/",
    })
}

<Button
	variant="outline"
	type="button"
	onClick={handleGithubLogin}
>
  <img
    src="https://cdn.reicon.dev/logos/github/original.svg"
	alt="Github"
	width={24}
	height={24}
	className="bg-gray-100"
   />
	Continue with Github
</Button>
```

6. ~={2}src/lib/auth-guard.ts=~
```ts
import { auth } from "@/lib/auth"
import { headers } from "next/headers"
import { redirect } from "next/navigation"

export const requireAuth = async () => {
    const session = await auth.api.getSession({
        headers: await headers()
    })

    if (session) {
        redirect("/login")
    }
    return session
}

export const requireUnAuth = async () => {
    const session = await auth.api.getSession({
        headers: await headers()
    })

    if (session) {
        redirect("/")
    }
    return session
}
```


7. ~={blue}src/lib/auth.ts=~
```ts
import { betterAuth } from "better-auth";
import { PrismaClient } from "@/generated/prisma/client";
import { prismaAdapter } from "better-auth/adapters/prisma";
import { prisma } from "./db";

export const auth = betterAuth({
    database: prismaAdapter(prisma, {
        provider: "postgresql",
    }),
    emailAndPassword: {
        enabled: true
    },
    socialProviders: {
        github: {
            clientId: process.env.GITHUB_CLIENT_ID as string,
            clientSecret: process.env.GITHUB_CLIENT_SECRET as string,
        },
    },
})
```

7. ~={2}src/lib/db.ts=~
```ts
import { PrismaClient } from "@/generated/prisma/client";
import { PrismaPg } from "@prisma/adapter-pg";

const globleForPrisma = global as unknown as {
    prisma: PrismaClient;
};

const adapter = new PrismaPg({
    connectionString: process.env.DATABASE_URL,
});

export const prisma = globleForPrisma.prisma || new PrismaClient({
    adapter,
});

if (process.env.NODE_ENV !== "production") {
    globleForPrisma.prisma = prisma;
}
```


8. ~={2}.env =~
```css
BETTER_AUTH_SECRET=czsxsmko9d0exSj9xUKMUkGc7YYYbg95
BETTER_AUTH_URL=http://localhost:3000 # Base URL of your app


DATABASE_URL = "postgresql://neondb_owner:npg_VS1aG6PsKDZz@ep-orange-sound-axaeotbi-pooler.c-4.us-east-2.aws.neon.tech/neondb?sslmode=require&channel_binding=require"


GITHUB_CLIENT_ID  = Ov23li2eY3Ai2iEqKZQj
GITHUB_CLIENT_SECRET  = 0ede6d9c73686ba4f9e4996c1105f4edfb87bf26
```