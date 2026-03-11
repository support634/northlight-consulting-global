# 🤖 NorthLight Consulting Global — Junior Dev Build Agent

**Site:** NorthLight Consulting Global
**Stack:** Next.js 14 (App Router) + Tailwind CSS
**Inspiration:** [premiermarketingus.com](https://premiermarketingus.com/)
**Pages in Scope:** Home · About Us · Work Portfolio

---

## 🖼️ Site Logo

![NorthLight Consulting Global Logo](https://uploads.linear.app/d1bf6014-5724-4855-9106-4d39aa653a53/f2a3af49-7756-41d7-97e0-3aed89f6f44c/de077e55-4ad5-49bc-b6e3-ba1f05030d7e)

> Save this image to `public/images/logo.svg` (or `.png`) and reference it in the Navbar and Footer as the site logo.  

---

## 🎨 Brand Tokens (Use These Everywhere)

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: {
          black:      '#0A0A0A',   // primary background
          darkgray:   '#111111',   // section alt background
          purple:     '#9B30FF',   // primary accent / hover
          purpleDeep: '#6B21A8',   // darker purple for gradients
          white:      '#FFFFFF',   // primary text
          silver:     '#A1A1AA',   // muted/subtext
          gold:       '#F5C518',   // optional badge accent
        },
      },
      fontFamily: {
        display: ['Montserrat', 'sans-serif'],
        body:    ['Inter', 'sans-serif'],
      },
    },
  },
};
```

> **Google Fonts to import in `app/layout.tsx`:**
> `Montserrat` (weights: 700, 800, 900) — headlines  
> `Inter` (weights: 400, 500, 600) — body copy

---

## 📁 Folder Structure

```
northlight/
├── app/
│   ├── layout.tsx            ← Root layout (Navbar + Footer)
│   ├── page.tsx              ← Home page
│   ├── about/
│   │   └── page.tsx          ← About Us page
│   ├── portfolio/
│   │   └── page.tsx          ← Work Portfolio page
│   └── services/
│       └── [slug]/
│           └── page.tsx      ← Individual service page (optional)
├── components/
│   ├── layout/
│   │   ├── Navbar.tsx        ← Sticky nav + Services dropdown
│   │   └── Footer.tsx        ← Footer with CTA
│   ├── home/
│   │   ├── HeroSection.tsx
│   │   ├── ValueProps.tsx
│   │   ├── ServicesOverview.tsx
│   │   └── CtaBanner.tsx
│   ├── about/
│   │   ├── CompanyStory.tsx
│   │   ├── MissionValues.tsx
│   │   ├── TeamSection.tsx
│   │   └── Differentiators.tsx
│   ├── portfolio/
│   │   ├── CaseStudyCard.tsx
│   │   ├── IndustriesServed.tsx
│   │   └── ResultsMetrics.tsx
│   └── ui/
│       ├── Button.tsx        ← Reusable CTA button
│       ├── SectionLabel.tsx  ← Small uppercase section tag
│       └── AnimatedCounter.tsx ← Number roll-up for stats
├── lib/
│   ├── services.ts           ← Services data array
│   ├── caseStudies.ts        ← Portfolio data
│   └── team.ts               ← Team member data
├── public/
│   └── images/
│       ├── logo.svg
│       ├── hero-bg.jpg       ← or .mp4 for video background
│       └── ...
├── tailwind.config.js
└── next.config.js
```

---

## 🚀 Bootstrap Commands

```bash
# 1. Create the project
npx create-next-app@latest northlight --typescript --tailwind --eslint --app --src-dir=false

# 2. Install dependencies
cd northlight
npm install framer-motion        # animations
npm install @headlessui/react    # accessible dropdown + mobile menu
npm install lucide-react         # icons
npm install clsx                 # conditional classNames

# 3. Add Google Fonts — in app/layout.tsx
# Use next/font/google for performance
```

---

## 🧭 COMPONENT 1 — Navbar (`components/layout/Navbar.tsx`)

### What It Does
- Sticky top nav (`position: sticky; top: 0; z-index: 50`)
- Logo left · Nav links center/right · CTA button far right
- **Services dropdown** on hover (desktop) / tap (mobile)
- Mobile hamburger with animated drawer + accordion services list

### Services Dropdown Data (`lib/services.ts`)

```ts
export const services = [
  { label: 'SEO',                     href: '/services/seo' },
  { label: 'PPC',                     href: '/services/ppc' },
  { label: 'Website Design',          href: '/services/website-design' },
  { label: 'Branding',                href: '/services/branding' },
  { label: 'Graphic Design',          href: '/services/graphic-design' },
  { label: 'Email / SMS',             href: '/services/email-sms' },
  { label: 'AI Automation',           href: '/services/ai-automation' },
  { label: 'Credit Card Processing',  href: '/services/credit-card-processing' },
  { label: 'CRM / Zoho Configuration',href: '/services/crm-zoho' },
  { label: 'Social Media Management', href: '/services/social-media' },
];
```

### Navbar Tailwind Classes (Key Patterns)

```tsx
// Outer nav bar
<nav className="sticky top-0 z-50 bg-brand-black border-b border-white/10 backdrop-blur-sm">

// Desktop dropdown trigger
<button
  onMouseEnter={() => setOpen(true)}
  onMouseLeave={() => setOpen(false)}
  className="text-white hover:text-brand-purple transition-colors duration-200 font-medium"
>
  Services ▾
</button>

// Desktop dropdown panel (animate with framer-motion or CSS transition)
<div className="absolute top-full left-0 w-56 bg-[#111111] border border-white/10 rounded-lg shadow-xl py-2">
  {services.map(s => (
    <a
      key={s.href}
      href={s.href}
      className="block px-4 py-2 text-sm text-white hover:bg-brand-purple hover:text-white transition-colors duration-150"
    >
      {s.label}
    </a>
  ))}
</div>

// CTA button
<a
  href="#contact"
  className="bg-brand-purple hover:bg-brand-purpleDeep text-white font-semibold px-5 py-2.5 rounded-full transition-colors duration-200 text-sm"
>
  Get a Free Consultation
</a>
```

### Mobile Hamburger Logic
- Use `useState` for `mobileOpen` and `servicesAccordionOpen`
- Toggle `servicesAccordionOpen` when "Services" is tapped inside the drawer
- Drawer slides in from right or top using `framer-motion` `AnimatePresence`

---

## 🏠 PAGE 1 — Home (`app/page.tsx`)

### Section Order
1. `HeroSection` — full-viewport, dark bg, headline + CTA
2. `ValueProps` — 3–4 columns with icons
3. `ServicesOverview` — card grid, links to service pages
4. `CtaBanner` — mid-page CTA strip (purple background)
5. `TestimonialsSlider` *(optional)* — social proof
6. Footer CTA

---

### HeroSection (`components/home/HeroSection.tsx`)

**What to build:**
- Full-width, 100vh dark section
- Background: dark gradient OR subtle looping video (muted, autoplay, loop)
- Large H1, subheading, primary + secondary CTA buttons
- Subtle animated floating gradient blob in the background (purple glow)

```tsx
// Key Tailwind classes
<section className="relative min-h-screen flex items-center bg-brand-black overflow-hidden">

  {/* Background glow blob */}
  <div className="absolute top-1/4 left-1/2 -translate-x-1/2 w-[600px] h-[600px] 
                  bg-brand-purple/20 rounded-full blur-[120px] pointer-events-none" />

  {/* Content */}
  <div className="relative z-10 max-w-7xl mx-auto px-6 lg:px-12">
    <p className="text-brand-purple uppercase tracking-widest text-sm font-semibold mb-4">
      Digital Growth Agency
    </p>
    <h1 className="font-display text-5xl md:text-7xl font-black text-white leading-tight mb-6">
      We Grow Businesses<br />
      <span className="text-brand-purple">That Mean Something</span>
    </h1>
    <p className="text-brand-silver text-lg md:text-xl max-w-2xl mb-10">
      NorthLight Consulting Global delivers measurable digital results —
      from SEO and PPC to AI automation and full brand transformation.
    </p>
    <div className="flex flex-wrap gap-4">
      <a href="#contact" className="bg-brand-purple hover:bg-brand-purpleDeep text-white 
                                    font-bold px-8 py-4 rounded-full transition-all duration-200">
        Get a Free Consultation
      </a>
      <a href="/portfolio" className="border border-white/30 text-white hover:border-brand-purple 
                                      px-8 py-4 rounded-full transition-all duration-200">
        See Our Work
      </a>
    </div>
  </div>
</section>
```

---

### ValueProps (`components/home/ValueProps.tsx`)

**What to build:** 3–4 cards with icon + headline + 1–2 sentence description

```tsx
const values = [
  { icon: '⚡', title: 'Results-First Strategy',   body: 'Every campaign starts with your outcomes, not our services.' },
  { icon: '🎯', title: 'Full-Service Expertise',   body: 'From search to automation, we execute under one roof.' },
  { icon: '🔒', title: 'Transparent Reporting',    body: 'You always know exactly what your investment is doing.' },
  { icon: '🤖', title: 'AI-Powered Efficiency',    body: 'We bake automation into everything we build for you.' },
];

// Card classes
<div className="bg-brand-darkgray border border-white/10 rounded-2xl p-8 
                hover:border-brand-purple/50 transition-colors duration-300">
```

---

### ServicesOverview (`components/home/ServicesOverview.tsx`)

**What to build:** 2×5 or 3×4 card grid linking to each service page

```tsx
// Import from lib/services.ts — reuse the same data
// Card classes
<a href={service.href}
   className="group flex items-center gap-3 bg-brand-darkgray border border-white/10 
              rounded-xl px-5 py-4 hover:border-brand-purple hover:bg-brand-purple/10 
              transition-all duration-200">
  <span className="text-brand-purple font-bold text-xl">→</span>
  <span className="text-white font-medium group-hover:text-brand-purple transition-colors">
    {service.label}
  </span>
</a>
```

---

## 👥 PAGE 2 — About Us (`app/about/page.tsx`)

### Section Order
1. `PageHero` — "About NorthLight" headline + subtext
2. `CompanyStory` — 2-col layout: text left, image right
3. `MissionValues` — 3-card grid
4. `TeamSection` — headshot cards grid
5. `Differentiators` — "What Sets Us Apart" — icon list
6. Mid-page CTA

---

### CompanyStory (`components/about/CompanyStory.tsx`)

```tsx
// 2-column grid
<section className="max-w-7xl mx-auto px-6 lg:px-12 py-24 grid md:grid-cols-2 gap-16 items-center">
  <div>
    <p className="text-brand-purple uppercase tracking-widest text-sm mb-3">Our Story</p>
    <h2 className="font-display text-4xl font-black text-white mb-6">
      Built to Make Your Brand Shine
    </h2>
    <p className="text-brand-silver leading-relaxed">
      NorthLight Consulting Global was founded on a simple belief: every business 
      deserves a marketing partner that treats their growth like its own. We combine 
      data-driven strategy with creative execution to deliver results that compound.
    </p>
  </div>
  <div className="rounded-2xl overflow-hidden border border-white/10">
    <img src="/images/about-story.jpg" alt="NorthLight team" className="w-full h-full object-cover" />
  </div>
</section>
```

---

### MissionValues (`components/about/MissionValues.tsx`)

```ts
const values = [
  { title: 'Our Mission',  body: 'To illuminate the path forward for every client through honest strategy and measurable outcomes.' },
  { title: 'Our Vision',   body: 'A world where businesses of all sizes compete with confidence in the digital space.' },
  { title: 'Our Values',   body: 'Integrity · Transparency · Innovation · Client Obsession' },
];
```

---

### TeamSection (`components/about/TeamSection.tsx`)

```ts
// lib/team.ts
export const team = [
  { name: 'Alex Rivera',   title: 'Founder & CEO',        photo: '/images/team/alex.jpg' },
  { name: 'Jordan Lee',    title: 'Head of SEO Strategy', photo: '/images/team/jordan.jpg' },
  { name: 'Morgan Smith',  title: 'Creative Director',    photo: '/images/team/morgan.jpg' },
  { name: 'Taylor Brooks', title: 'Paid Media Lead',      photo: '/images/team/taylor.jpg' },
];

// Card classes
<div className="bg-brand-darkgray border border-white/10 rounded-2xl overflow-hidden 
                hover:border-brand-purple/50 transition-colors duration-300">
  <img src={member.photo} className="w-full h-56 object-cover" />
  <div className="p-5">
    <p className="text-white font-bold">{member.name}</p>
    <p className="text-brand-purple text-sm">{member.title}</p>
  </div>
</div>
```

---

### Differentiators (`components/about/Differentiators.tsx`)

```ts
const diffs = [
  'End-to-end services — no handoffs, no excuses',
  'AI-powered workflows that cut turnaround time in half',
  'Dedicated account manager for every client',
  'Month-to-month flexibility — no lock-in contracts',
  'Proven results across 12+ industries',
];
// Render each as a checkmark list row with brand-purple ✓ icon
```

---

## 💼 PAGE 3 — Work Portfolio (`app/portfolio/page.tsx`)

### Section Order
1. `PageHero` — "Our Work" headline
2. `ResultsMetrics` — 4 animated stat counters
3. `CaseStudyGrid` — filterable cards by industry
4. `IndustriesServed` — logo/icon industry tag cloud
5. CTA Banner

---

### ResultsMetrics (`components/portfolio/ResultsMetrics.tsx`)

```ts
const stats = [
  { value: 200,  suffix: '+', label: 'Clients Served' },
  { value: 340,  suffix: '%', label: 'Average ROI Increase' },
  { value: 12,   suffix: '+', label: 'Industries Served' },
  { value: 98,   suffix: '%', label: 'Client Retention Rate' },
];
// Use AnimatedCounter component — count up on scroll via IntersectionObserver
```

---

### CaseStudyCard (`components/portfolio/CaseStudyCard.tsx`)

```ts
// lib/caseStudies.ts
export const caseStudies = [
  {
    client:     'Apex Legal Group',
    industry:   'Legal',
    service:    'SEO + PPC',
    result:     '287% increase in organic leads in 6 months',
    image:      '/images/cases/apex-legal.jpg',
    tags:       ['SEO', 'PPC', 'Legal'],
  },
  {
    client:     'GreenRoot HVAC',
    industry:   'Home Services',
    service:    'Website Design + Local SEO',
    result:     '4× increase in service calls within 90 days',
    image:      '/images/cases/greenroot.jpg',
    tags:       ['Website Design', 'SEO', 'Home Services'],
  },
  {
    client:     'Radiance MedSpa',
    industry:   'Medical / Healthcare',
    service:    'Social Media + Email/SMS',
    result:     '63% boost in repeat bookings via automation',
    image:      '/images/cases/radiance.jpg',
    tags:       ['Social Media', 'Email/SMS', 'Healthcare'],
  },
  {
    client:     'Summit B2B Solutions',
    industry:   'B2B',
    service:    'CRM / Zoho + AI Automation',
    result:     'Reduced sales cycle from 45 days to 18 days',
    image:      '/images/cases/summit.jpg',
    tags:       ['CRM', 'AI Automation', 'B2B'],
  },
];

// Card Tailwind classes
<div className="group relative bg-brand-darkgray border border-white/10 rounded-2xl 
                overflow-hidden hover:border-brand-purple transition-all duration-300">
  <img src={cs.image} className="w-full h-48 object-cover" />
  <div className="p-6">
    <span className="text-brand-purple text-xs font-semibold uppercase tracking-wider">
      {cs.industry}
    </span>
    <h3 className="text-white font-bold text-xl mt-2 mb-1">{cs.client}</h3>
    <p className="text-brand-silver text-sm mb-4">{cs.service}</p>
    <div className="bg-brand-purple/10 border border-brand-purple/30 rounded-lg px-4 py-3">
      <p className="text-brand-purple font-semibold text-sm">📈 {cs.result}</p>
    </div>
  </div>
</div>
```

---

### IndustriesServed (`components/portfolio/IndustriesServed.tsx`)

```ts
const industries = [
  'Home Services', 'Medical / Healthcare', 'Legal',
  'B2B', 'B2C', 'E-Commerce', 'Education',
  'Manufacturing', 'Finance', 'Real Estate',
  'Hospitality', 'Technology',
];

// Render as pill tags
<span className="border border-white/20 text-brand-silver text-sm px-4 py-2 rounded-full 
                 hover:border-brand-purple hover:text-brand-purple transition-colors cursor-default">
  {industry}
</span>
```

---

## 🔧 Reusable UI Components

### Button (`components/ui/Button.tsx`)

```tsx
type ButtonProps = {
  variant?: 'primary' | 'outline';
  href?: string;
  children: React.ReactNode;
};

export default function Button({ variant = 'primary', href, children }: ButtonProps) {
  const base = 'font-bold px-8 py-4 rounded-full transition-all duration-200 inline-block text-center';
  const styles = {
    primary: 'bg-brand-purple hover:bg-brand-purpleDeep text-white',
    outline: 'border border-white/30 text-white hover:border-brand-purple hover:text-brand-purple',
  };
  if (href) return <a href={href} className={`${base} ${styles[variant]}`}>{children}</a>;
  return <button className={`${base} ${styles[variant]}`}>{children}</button>;
}
```

### SectionLabel (`components/ui/SectionLabel.tsx`)

```tsx
export default function SectionLabel({ children }: { children: string }) {
  return (
    <p className="text-brand-purple uppercase tracking-widest text-xs font-semibold mb-3">
      {children}
    </p>
  );
}
```

### AnimatedCounter (`components/ui/AnimatedCounter.tsx`)

```tsx
'use client';
import { useEffect, useRef, useState } from 'react';

export default function AnimatedCounter({ value, suffix = '' }: { value: number; suffix?: string }) {
  const [count, setCount] = useState(0);
  const ref = useRef<HTMLSpanElement>(null);

  useEffect(() => {
    const observer = new IntersectionObserver(([entry]) => {
      if (!entry.isIntersecting) return;
      let start = 0;
      const step = Math.ceil(value / 60);
      const timer = setInterval(() => {
        start += step;
        if (start >= value) { setCount(value); clearInterval(timer); }
        else setCount(start);
      }, 16);
    }, { threshold: 0.5 });
    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, [value]);

  return <span ref={ref}>{count}{suffix}</span>;
}
```

---

## 🦶 Footer (`components/layout/Footer.tsx`)



---

## 📐 Layout Pattern (Every Page)

```tsx
// app/layout.tsx
import Navbar from '@/components/layout/Navbar';
import Footer from '@/components/layout/Footer';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body className="bg-brand-black font-body text-white antialiased">
        <Navbar />
        <main>{children}</main>
        <Footer />
      </body>
    </html>
  );
}
```

---

## ✅ Build Checklist (Junior Dev Order)

```
Phase 1 — Foundation
[ ] npx create-next-app with flags above
[ ] Configure tailwind.config.js with brand tokens
[ ] Add Google Fonts (Montserrat + Inter) via next/font/google
[ ] Create /lib/services.ts, /lib/caseStudies.ts, /lib/team.ts

Phase 2 — Layout Shell
[ ] Build Navbar (desktop + mobile + Services dropdown)
[ ] Build Footer (CTA strip + links + copyright)
[ ] Wire layout.tsx

Phase 3 — Home Page
[ ] HeroSection (dark bg + glow blob + headline + CTAs)
[ ] ValueProps (4-card grid)
[ ] ServicesOverview (service card grid from lib/services.ts)
[ ] CtaBanner (purple strip)

Phase 4 — About Us Page
[ ] CompanyStory (2-col text + image)
[ ] MissionValues (3-card grid)
[ ] TeamSection (headshot cards)
[ ] Differentiators (checkmark list)

Phase 5 — Portfolio Page
[ ] ResultsMetrics (4 animated counters)
[ ] CaseStudyGrid (cards from lib/caseStudies.ts)
[ ] IndustriesServed (pill tags)

Phase 6 — Polish
[ ] Framer Motion: fade-in-up on section entry (stagger children)
[ ] Hover states verified across all interactive elements
[ ] Mobile responsive audit (375px, 768px, 1280px)
[ ] Page meta titles + descriptions in each page.tsx
[ ] next/image for all <img> tags (add domains to next.config.js)
[ ] Lighthouse audit — aim 90+ performance
```

---

## ⚠️ Common Gotchas for Junior Devs

| Issue | Fix |
|---|---|
| Tailwind classes not working | Run `npx tailwindcss init -p`, check `content` paths in config |
| `'use client'` errors | Add `'use client'` at top of any file using hooks or browser APIs |
| Dropdown flickers on hover | Use CSS `transition + group-hover` OR add `onMouseEnter/Leave` with a short delay |
| Mobile menu not scrollable | Add `overflow-y-auto max-h-screen` to the drawer container |
| Images not optimized | Replace `<img>` with `next/image` — always set `width` + `height` or `fill` |
| Font not loading | Import using `next/font/google`, apply className to `<body>` in layout.tsx |
| `className` not applying | Check for Tailwind purge — class must exist as full string, not interpolated |

---

## 📦 Key Package Versions (as of 2025)

```json
{
  "next": "14.x",
  "react": "18.x",
  "tailwindcss": "3.x",
  "framer-motion": "^11.x",
  "@headlessui/react": "^2.x",
  "lucide-react": "^0.383.x",
  "clsx": "^2.x"
}
```

---

*Agent created for NorthLight Consulting Global — Junior Dev Build Reference v1.0*
