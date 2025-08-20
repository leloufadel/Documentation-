# Next.js Internationalization (i18n) Setup with next-intl

## Overview
This document provides a complete guide for the next-intl setup in the SchoolTIC project, supporting French, Arabic, and English translations.

## 🚀 Quick Start

Your application now supports three locales:
- **French (fr)** - Default locale
- **Arabic (ar)** - RTL support included
- **English (en)**

### URLs
- French: `http://localhost:3000/fr` or `http://localhost:3000/` (default)
- Arabic: `http://localhost:3000/ar`
- English: `http://localhost:3000/en`

## 📁 Project Structure

```
schooltic/
├── messages/
│   ├── fr.json          # French translations
│   ├── ar.json          # Arabic translations
│   └── en.json          # English translations
├── src/
│   ├── i18n/
│   │   ├── routing.ts   # Locale configuration
│   │   ├── navigation.ts # Internationalized navigation
│   │   └── request.ts   # Server-side configuration
│   ├── middleware.ts    # Locale detection middleware
│   └── app/
│       └── [locale]/    # Dynamic locale segment
│           ├── layout.tsx
│           ├── page.tsx
│           └── ...      # All your pages
├── next.config.ts       # Next.js config with next-intl plugin
└── package.json
```

## 🛠 Setup Steps Completed

### 1. Installation
```bash
npm install next-intl
```

### 2. Configuration Files

#### `next.config.ts`
```typescript
import type { NextConfig } from "next";
import createNextIntlPlugin from 'next-intl/plugin';

const nextConfig: NextConfig = {
  // your existing config
};

const withNextIntl = createNextIntlPlugin();
export default withNextIntl(nextConfig);
```

#### `src/i18n/routing.ts`
```typescript
import {defineRouting} from 'next-intl/routing';

export const routing = defineRouting({
  locales: ['en', 'fr', 'ar'],
  defaultLocale: 'fr'
});
```

#### `src/i18n/navigation.ts`
```typescript
import {createNavigation} from 'next-intl/navigation';
import {routing} from './routing';

export const {Link, redirect, usePathname, useRouter, getPathname} =
  createNavigation(routing);
```

#### `src/middleware.ts`
```typescript
import createMiddleware from 'next-intl/middleware';
import {routing} from './i18n/routing';

export default createMiddleware(routing);

export const config = {
  matcher: '/((?!api|trpc|_next|_vercel|.*\\..*).*)'
};
```

### 3. Layout Configuration

#### `src/app/[locale]/layout.tsx`
- ✅ Supports dynamic locale parameter
- ✅ Includes `NextIntlClientProvider`
- ✅ RTL support for Arabic (`dir={locale === 'ar' ? 'rtl' : 'ltr'}`)
- ✅ Static rendering support with `generateStaticParams`
- ✅ Locale validation with `hasLocale`

## 📝 Translation Usage

### In Server Components
```typescript
import { useTranslations } from 'next-intl';

export default function HomePage() {
  const t = useTranslations('HomePage');
  
  return (
    <div>
      <h1>{t('title')}</h1>
      <h2>{t('title2')}</h2>
      <p>{t('description')}</p>
    </div>
  );
}
```

### In Client Components
```typescript
'use client';
import { useTranslations } from 'next-intl';

export default function MyComponent() {
  const t = useTranslations('Navigation');
  
  return <nav>{t('home')}</nav>;
}
```

### Navigation
```typescript
import { Link } from '@/i18n/navigation';

export default function MyComponent() {
  return (
    <Link href="/contact">
      {t('contact')}
    </Link>
  );
}
```

## 📋 Message Structure

Each locale file (`messages/{locale}.json`) contains:

```json
{
  "HomePage": {
    "title": "EETFP-TIC",
    "title2": "Pourquoi choisir l'EETFP-TIC ?",
    "description": "École d'Enseignement Technique...",
    "formations": "Découvrir nos formations",
    "contact": "Nous contacter"
  },
  "Navigation": {
    "home": "Accueil",
    "formations": "Formations",
    "contact": "Contact"
  },
  "Common": {
    "loading": "Chargement...",
    "error": "Erreur",
    "save": "Enregistrer"
  }
}
```

## 🌍 Language-Specific Features

### Arabic (RTL) Support
- Automatic RTL direction: `dir={locale === 'ar' ? 'rtl' : 'ltr'}`
- Arabic translations with proper right-to-left text flow

### French (Default)
- Set as default locale in routing configuration
- Accessible at root URL (`/`) and `/fr`

### English
- Available at `/en`
- Complete English translations provided

## 🎯 Key Features Implemented

1. **Automatic Locale Detection**: Middleware handles locale negotiation
2. **Static Rendering**: Optimized for performance with `generateStaticParams`
3. **Type Safety**: Full TypeScript support
4. **SEO Friendly**: Proper locale-based URLs
5. **Client & Server Support**: Works in both component types
6. **Navigation**: Internationalized Link component
7. **RTL Support**: Arabic text direction support

## 🔧 Adding New Translations

### 1. Add to Message Files
Update `messages/fr.json`, `messages/ar.json`, and `messages/en.json`:

```json
{
  "NewSection": {
    "newKey": "New translation text"
  }
}
```

### 2. Use in Components
```typescript
const t = useTranslations('NewSection');
return <p>{t('newKey')}</p>;
```

## 🧪 Testing

### Manual Testing
1. Visit `http://localhost:3000/fr` - Should show French content
2. Visit `http://localhost:3000/ar` - Should show Arabic content (RTL)
3. Visit `http://localhost:3000/en` - Should show English content
4. Test navigation between pages
5. Verify middleware redirects work properly

### Browser Language Detection
The middleware automatically detects browser language preferences and redirects accordingly.

## 📚 Examples

### Homepage Translation Example
```typescript
// Before (hardcoded French)
<h1>École d'Enseignement Technique</h1>

// After (internationalized)
<h1>{t('title')}</h1>
```

### Navigation Link Example
```typescript
// Before
import Link from 'next/link';
<Link href="/contact">Contact</Link>

// After
import { Link } from '@/i18n/navigation';
<Link href="/contact">{t('contact')}</Link>
```

## 🚨 Common Issues & Solutions

### 1. Build Errors
Make sure all pages in `[locale]` directory include `setRequestLocale(locale)` for static rendering.

### 2. Missing Translations
Check that translation keys exist in all three message files (fr.json, ar.json, en.json).

### 3. Navigation Issues
Always use `Link` from `@/i18n/navigation`, not `next/link`.

### 4. Middleware Conflicts
Ensure middleware matcher excludes API routes and static assets.

## 📞 Support

For questions about this setup, refer to:
- [next-intl Documentation](https://next-intl.dev/)
- [Next.js i18n Documentation](https://nextjs.org/docs/app/building-your-application/routing/internationalization)

---

**Setup completed**: ✅ All features working
**Languages supported**: 🇫🇷 French | 🇸🇦 Arabic | 🇬🇧 English
**Last updated**: January 2025

