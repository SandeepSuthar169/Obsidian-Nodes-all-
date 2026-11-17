If you're building backend projects, it's useful to know the APIs developers commonly integrate in production. Here's a categorized list with examples.

## 1. Authentication & Authorization

- [Clerk](https://clerk.com?utm_source=chatgpt.com) — User authentication, organizations, MFA
- [Auth0](https://auth0.com?utm_source=chatgpt.com) — Enterprise authentication
- [Firebase Authentication](https://firebase.google.com/products/auth?utm_source=chatgpt.com)
- [Supabase Auth](https://supabase.com/auth?utm_source=chatgpt.com)
- [Keycloak](https://www.keycloak.org?utm_source=chatgpt.com) — Open-source identity management

---

## 2. Fake Users & Mock Data

- [Random User API](https://randomuser.me?utm_source=chatgpt.com)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com?utm_source=chatgpt.com)
- [DummyJSON](https://dummyjson.com?utm_source=chatgpt.com)
- [Mockaroo](https://mockaroo.com?utm_source=chatgpt.com)

Example:

```
GET https://randomuser.me/api/
```

---

## 3. User Avatar APIs

- [DiceBear](https://www.dicebear.com?utm_source=chatgpt.com)
- [UI Avatars](https://ui-avatars.com?utm_source=chatgpt.com)
- [Gravatar](https://gravatar.com?utm_source=chatgpt.com)

Example:

```
https://api.dicebear.com/9.x/adventurer/svg?seed=John
```

---

## 4. File Storage

- [Cloudinary](https://cloudinary.com?utm_source=chatgpt.com)
- [AWS S3](https://aws.amazon.com/s3?utm_source=chatgpt.com)
- [UploadThing](https://uploadthing.com?utm_source=chatgpt.com)
- [ImageKit](https://imagekit.io?utm_source=chatgpt.com)

---

## 5. Email APIs

- [Resend](https://resend.com?utm_source=chatgpt.com)
- [SendGrid](https://sendgrid.com?utm_source=chatgpt.com)
- [Mailgun](https://www.mailgun.com?utm_source=chatgpt.com)
- [Amazon SES](https://aws.amazon.com/ses?utm_source=chatgpt.com)

---

## 6. SMS & OTP

- [Twilio](https://www.twilio.com?utm_source=chatgpt.com)
- [Vonage](https://www.vonage.com?utm_source=chatgpt.com)
- [MessageBird](https://www.messagebird.com?utm_source=chatgpt.com)

---

## 7. Payment APIs

- [Stripe](https://stripe.com?utm_source=chatgpt.com)
- [Razorpay](https://razorpay.com?utm_source=chatgpt.com)
- [PayPal Developer](https://developer.paypal.com?utm_source=chatgpt.com)
- [Square](https://developer.squareup.com?utm_source=chatgpt.com)

---

## 8. Maps & Location

- [Google Maps Platform](https://developers.google.com/maps?utm_source=chatgpt.com)
- [Mapbox](https://www.mapbox.com?utm_source=chatgpt.com)
- [OpenStreetMap Nominatim](https://nominatim.openstreetmap.org?utm_source=chatgpt.com)
- [Geoapify](https://www.geoapify.com?utm_source=chatgpt.com)

---

## 9. Weather APIs

- [OpenWeather](https://openweathermap.org/api?utm_source=chatgpt.com)
- [WeatherAPI](https://www.weatherapi.com?utm_source=chatgpt.com)
- [Tomorrow.io](https://www.tomorrow.io?utm_source=chatgpt.com)

---

## 10. AI APIs

- [OpenAI API](https://platform.openai.com?utm_source=chatgpt.com)
- [Anthropic API](https://www.anthropic.com/api?utm_source=chatgpt.com)
- [Google AI Studio](https://aistudio.google.com?utm_source=chatgpt.com)
- [Cohere](https://cohere.com?utm_source=chatgpt.com)

---

## 11. Image APIs

- [Unsplash API](https://unsplash.com/developers?utm_source=chatgpt.com)
- [Pexels API](https://www.pexels.com/api?utm_source=chatgpt.com)
- [Pixabay API](https://pixabay.com/api/docs?utm_source=chatgpt.com)

---

## 12. News APIs

- [NewsAPI](https://newsapi.org?utm_source=chatgpt.com)
- [GNews](https://gnews.io?utm_source=chatgpt.com)
- [The Guardian Open Platform](https://open-platform.theguardian.com?utm_source=chatgpt.com)

---

## 13. Currency & Exchange Rates

- [ExchangeRate.host](https://exchangerate.host?utm_source=chatgpt.com)
- [Frankfurter API](https://www.frankfurter.app?utm_source=chatgpt.com)
- [Fixer](https://fixer.io?utm_source=chatgpt.com)

---

## 14. Translation APIs

- [Google Cloud Translation](https://cloud.google.com/translate?utm_source=chatgpt.com)
- [DeepL API](https://www.deepl.com/pro-api?utm_source=chatgpt.com)
- [LibreTranslate](https://libretranslate.com?utm_source=chatgpt.com)

---

## 15. IP & Geolocation

- [ipinfo.io](https://ipinfo.io?utm_source=chatgpt.com)
- [ipapi](https://ipapi.co?utm_source=chatgpt.com)
- [ip-api](https://ip-api.com?utm_source=chatgpt.com)

---

## 16. URL Shortener

- [Bitly API](https://bitly.com?utm_source=chatgpt.com)
- [TinyURL API](https://tinyurl.com?utm_source=chatgpt.com)

---

## 17. QR Code APIs

- [goQR.me API](https://goqr.me/api?utm_source=chatgpt.com)
- [QR Server API](https://goqr.me/api?utm_source=chatgpt.com)

---

## 18. PDF Generation

- PDFMonkey
- [PDFShift](https://pdfshift.io?utm_source=chatgpt.com)
- HTML/CSS to PDF API

---

## 19. Logging & Monitoring

- [Sentry](https://sentry.io?utm_source=chatgpt.com)
- [Datadog](https://www.datadoghq.com?utm_source=chatgpt.com)
- [New Relic](https://newrelic.com?utm_source=chatgpt.com)

---

## 20. Common Backend APIs You'll Build Yourself

In many applications, you'll expose endpoints such as:

```
POST   /auth/login
POST   /auth/register
POST   /auth/logout
POST   /auth/refresh

GET    /users
GET    /users/:id
PATCH  /users/:id
DELETE /users/:id

POST   /upload
GET    /notifications
POST   /orders
GET    /products
POST   /payments/webhook

GET    /dashboard/stats
GET    /search
POST   /feedback
```

### Recommended stack for modern backend projects

- **Authentication:** [Clerk](https://clerk.com?utm_source=chatgpt.com) or [Auth0](https://auth0.com?utm_source=chatgpt.com)
- **Database:** [Supabase](https://supabase.com?utm_source=chatgpt.com) or [Neon](https://neon.tech?utm_source=chatgpt.com) (PostgreSQL)
- **File uploads:** [Cloudinary](https://cloudinary.com?utm_source=chatgpt.com)
- **Email:** [Resend](https://resend.com?utm_source=chatgpt.com)
- **Payments:** [Stripe](https://stripe.com?utm_source=chatgpt.com) (global) or [Razorpay](https://razorpay.com?utm_source=chatgpt.com) (India)
- **Monitoring:** [Sentry](https://sentry.io?utm_source=chatgpt.com)
- **AI features:** [OpenAI API](https://platform.openai.com?utm_source=chatgpt.com)

These services cover the majority of integrations used in production web applications, from user management and payments to media storage, notifications, and AI.

------
---


Here are some of the most useful **free/public APIs** that backend developers commonly use in real-world applications.

|Category|API|Example Endpoint|
|---|---|---|
|Authentication|Firebase Auth REST|`https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword`|
|Fake Users|[Random User API](https://randomuser.me?utm_source=chatgpt.com)|`https://randomuser.me/api/`|
|User Avatars|[DiceBear](https://www.dicebear.com?utm_source=chatgpt.com)|`https://api.dicebear.com/9.x/avataaars/svg?seed=john`|
|Initial Avatar|[UI Avatars](https://ui-avatars.com?utm_source=chatgpt.com)|`https://ui-avatars.com/api/?name=John+Doe`|
|Country|[REST Countries](https://restcountries.com?utm_source=chatgpt.com)|`https://restcountries.com/v3.1/all`|
|Currency|[ExchangeRate.host](https://exchangerate.host?utm_source=chatgpt.com)|`https://api.exchangerate.host/latest`|
|Weather|[OpenWeather](https://openweathermap.org/api?utm_source=chatgpt.com)|`https://api.openweathermap.org/data/2.5/weather`|
|Geocoding|[OpenStreetMap Nominatim](https://nominatim.openstreetmap.org?utm_source=chatgpt.com)|`/search?q=London&format=json`|
|News|[NewsAPI](https://newsapi.org?utm_source=chatgpt.com)|`/v2/top-headlines`|
|Images|[Unsplash API](https://unsplash.com/developers?utm_source=chatgpt.com)|`/photos/random`|
|Placeholder Images|[Picsum Photos](https://picsum.photos?utm_source=chatgpt.com)|`https://picsum.photos/300/300`|
|QR Code|[goQR API](https://goqr.me/api?utm_source=chatgpt.com)|`/api/?data=Hello`|
|UUID|UUIDTools Generator|`/generate/v4`|
|IP Address|[ipify](https://www.ipify.org?utm_source=chatgpt.com)|`https://api.ipify.org?format=json`|
|IP Geolocation|[ipinfo.io](https://ipinfo.io?utm_source=chatgpt.com)|`https://ipinfo.io/json`|
|Timezone|[World Time API](https://worldtimeapi.org?utm_source=chatgpt.com)|`/api/timezone`|
|Holidays|[Nager.Date](https://date.nager.at?utm_source=chatgpt.com)|`/api/v3/PublicHolidays/2026/US`|
|Quotes|[Quotable Archive](https://github.com/lukePeavey/quotable?utm_source=chatgpt.com)|`/random` (service availability may vary)|
|Jokes|[JokeAPI](https://jokeapi.dev?utm_source=chatgpt.com)|`/joke/Any`|
|Facts|[Useless Facts API](https://uselessfacts.jsph.pl?utm_source=chatgpt.com)|`/api/v2/facts/random`|
|Cat Images|[The Cat API](https://thecatapi.com?utm_source=chatgpt.com)|`/v1/images/search`|
|Dog Images|[Dog CEO API](https://dog.ceo/dog-api?utm_source=chatgpt.com)|`/api/breeds/image/random`|

## Common APIs used in backend projects

### User Profile

```
GET https://randomuser.me/api/
```

### User Avatar

```
GET https://api.dicebear.com/9.x/avataaars/svg?seed=john
```

### Initial Avatar

```
GET https://ui-avatars.com/api/?name=John+Doe
```

### Country List

```
GET https://restcountries.com/v3.1/all
```

### Exchange Rate

```
GET https://api.exchangerate.host/latest
```

### Random Image

```
GET https://picsum.photos/400/400
```

### Random Joke

```
GET https://v2.jokeapi.dev/joke/Any
```

### Random Quote

```
GET https://api.quotable.io/random
```

### Current IP

```
GET https://api.ipify.org?format=json
```

### Current Time

```
GET https://worldtimeapi.org/api/timezone/Asia/Kolkata
```

## Great APIs for practice projects

- Authentication: [Firebase Authentication](https://firebase.google.com/products/auth?utm_source=chatgpt.com)
- Users: [Random User API](https://randomuser.me?utm_source=chatgpt.com)
- Avatars: [DiceBear](https://www.dicebear.com?utm_source=chatgpt.com)
- Countries: [REST Countries](https://restcountries.com?utm_source=chatgpt.com)
- Weather: [OpenWeather](https://openweathermap.org/api?utm_source=chatgpt.com)
- Maps: [OpenStreetMap Nominatim](https://nominatim.openstreetmap.org?utm_source=chatgpt.com)
- Payments (sandbox): [Stripe Developers](https://docs.stripe.com?utm_source=chatgpt.com)
- Email: [Resend](https://resend.com?utm_source=chatgpt.com)
- File uploads: [Cloudinary](https://cloudinary.com?utm_source=chatgpt.com)
- AI: [OpenAI API](https://platform.openai.com?utm_source=chatgpt.com)

These APIs are widely used for learning and production integrations, and many offer free tiers or public endpoints suitable for backend development.



