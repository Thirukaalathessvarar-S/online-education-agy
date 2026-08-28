# Compiled Design Payload — Full Page

## 1. Global Design Tokens

### Colors
| Token | Value | Usage |
|---|---|---|
| Body BG | `#26335D` | Page body background |
| Header BG | `#2C799A` | Header background |
| Primary CTA | `#8D5CF6` | Buttons, links, accents |
| Text White | `#FFFFFF` | Primary text on dark backgrounds |
| Text Dark | `#252B42` | Card headings |
| Text Gray | `#737373` | Card body text, secondary text |
| Border Light | `#ECECEC` | Outline button borders |
| Card BG | `#FFFFFF` | Feature cards background |
| Decor Red | `#E74040` | Decorative lines |
| Price Old | `#BDBDBD` | Strikethrough price |
| Price Sale | `#FF7B52` | Sale price |
| Star Yellow | `#FFCE31` | Rating star icon (course cards) |
| Star Gold | `#F3CD03` | Rating star icon (review cards) |
| Rating BG | `#26335D` | Rating badge background |
| Border Input | `#E6E6E6` | Newsletter input/button border |

### Typography (Font Family: Montserrat throughout)
| Element | Weight | Size | Line-Height | Letter-Spacing | Color |
|---|---|---|---|---|---|
| Navbar Brand | 700 | 24px | 32px | 0.1px | #FFFFFF |
| Nav Links | 700 | 14px | 24px | 0.2px | #FFFFFF |
| Auth Links | 700 | 14px | 22px | 0.2px | #FFFFFF |
| Hero Subtitle (h5) | 700 | 16px | 24px | 0.1px | #FFFFFF |
| Hero Heading (h1) | 700 | 58px | 80px | 0.2px | #FFFFFF |
| Hero Description (h4) | 400 | 20px | 30px | 0.2px | #FFFFFF |
| CTA Buttons | 700 | 14px | 22px | 0.2px | #FFFFFF |
| Section Heading (h2) | 700 | 40px | 50px | 0.2px | #FFFFFF |
| Section Subtitle (h6) | 700 | 14px | 24px | 0.2px | #8D5CF6 |
| Card Heading (h5) | 700 | 16px | 24px | 0.1px | #252B42 |
| Card Paragraph | 400 | 14px | 20px | 0.2px | #737373 |
| Card Link | 700 | 14px | 24px | 0.2px | #8D5CF6 |
| Rating Text | 400 | 12px | 16px | 0.2px | #FFFFFF |
| Sales Text (h6) | 700 | 14px | 24px | 0.2px | #737373 |
| Price Old (h5) | 700 | 16px | 24px | 0.1px | #BDBDBD |
| Price Sale (h5) | 700 | 16px | 24px | 0.1px | #FF7B52 |
| Review Content | 400 | 14px | 20px | 0.2px | #737373 |
| Review Name (h6) | 700 | 14px | 24px | 0.2px | #8D5CF6 |
| Review Role (small) | 400 | 12px | 16px | 0.2px | #252B42 |
| FAQ Title (h5) | 700 | 16px | 24px | 0.1px | #252B42 |
| FAQ Text (h6) | 700 | 14px | 24px | 0.2px | #737373 |
| Newsletter Input | 400 | 14px | 28px | 0.2px | #737373 |
| Newsletter Button | 400 | 14px | 28px | 0.2px | #FFFFFF |

---

## 2. Section-by-Section Layout Hierarchy

### 2.1 Hero Section (Level 1)

```
Hero Section (Level 1) — full-width section
├── Header (Level 2) — container-fluid, height: 10vh, bg: #2C799A
│   └── Header-container (Level 3) — container, d-flex, flex-column, justify-content-between
│       ├── Navbar-brand (Level 4) — "BrandName"
│       ├── Navbar-collapse (Level 4)
│       │   ├── ul (Level 5) — d-flex, gap: 21px — [Home, Product, Pricing, Contact]
│       │   └── ul (Level 5) — d-flex, gap: 45px — [Login, Sign up → (button)]
│       └── Navbar-toggler (Level 4) — default Bootstrap toggler
├── Hero (Level 2) — container-fluid, height: 90vh
│   └── container (Level 3) — d-flex, justify-content-center, align-items-start
│       └── Hero box (Level 4) — d-flex, flex-column, justify-content-center, gap: 35px
│           ├── h5 — "Online training"
│           ├── h1 — "25K+ STUDENTS TRUST US"
│           ├── h4 — "Our goal is to make online education work for everyone"
│           └── button group (Level 5) — d-flex, gap: 10px
│               ├── button1 — "Get Quote Now" (bg: #8D5CF6, radius: 5px, px: 40px, py: 15px)
│               └── button2 — "Learn More" (border: 1px solid #ECECEC, radius: 5px, px: 40px, py: 15px)
└── Hero::before (Level 2) — pseudo-element
    ├── background-image: assets/images/landing-background.svg
    ├── background: no-repeat, cover, 100% width, 100% height
    └── z-index: behind hero box
```

---

### 2.2 Feature Section (Level 1)

```
Feature-section (Level 1)
└── Container (Level 2) — container, row, row-cols-3, gap: 30px
    ├── Card 1 (Level 3) — d-flex, justify-content-between, flex-column
    │   ├── Image container → Img: assets/images/school.svg
    │   ├── h5 — "2,769 online courses"
    │   ├── Decor — height: 2px, width: 50px, bg: #E74040
    │   └── Paragraph — "The gradual accumulation of information about atomic and small-scale behaviour..."
    ├── Card 2 (Level 3) — Image: assets/images/books.svg, h5 — "Expert instruction"
    └── Card 3 (Level 3) — Image: assets/images/books.svg, h5 — "Expert instruction"
```

Card Specs: Padding: 35px 40px | Gap: 20px | Box-shadow: 0px 13px 19px 0px #00000012 | BG: #FFFFFF

---

### 2.3 Expert Section (Level 1)

```
Expert-section (Level 1)
└── container (Level 2) — container, d-flex, justify-content-between
    ├── Image Container (Level 3) → Img: assets/images/expert-teacher.svg
    └── Content container (Level 3) — justify-content-center, margin-left: auto
        └── Content (Level 4) — d-flex, flex-column, gap: 35px
            ├── Decor — width: 94px, height: 7px, bg: #E74040
            ├── H2 — "Our Experts Teacher"
            ├── Paragraph — "Problems trying to resolve the conflict..."
            └── Link group — d-flex, align-items-center, gap: 10px
                ├── Anchor — "Learn More" (color: #8D5CF6)
                └── Arrow — icon(angle-right)
```

---

### 2.4 Package Section (Level 1)

```
Package-section (Level 1)
└── Container (Level 2) — container, row, row-cols-2, justify-content-between
    ├── Content Container (Level 3) — col
    │   ├── Decor — width: 94px, height: 7px, bg: #E74040
    │   ├── H2 — "Approdable Packages"
    │   └── Link group — d-flex, align-items-center, gap: 10px
    └── Image Container (Level 3) — col
        └── Img: assets/images/books.svg (position: relative)
            └── Button (Play Video) — position: absolute, centered, bg: #8D5CF6, data-bs-toggle="modal", data-bs-target="#courseVideoModal"
```

---

### 2.5 Course Section (Level 1)

```
Course-section (Level 1)
└── Container (Level 2) — d-flex, flex-column, align-items-start, gap: 80px
    ├── Heading Container (Level 3)
    │   ├── H6 — "Practice Advice" (color: #8D5CF6)
    │   ├── H2 — "Our Popular Courses"
    │   └── Paragraph
    └── Course Container (Level 3) — row, row-cols-3, gap/gutter: 10px
        ├── Card 1 — Img: pencils.svg, Link: "For Better Future", H5: "2,769 online courses"
        ├── Card 2 — Img: library.svg, Link: "Welcome", H5: "Training Courses"
        └── Card 3 — Img: apple.svg, Link: "Welcome", H5: "Books Liberary"
```

---

### 2.6 Review Section (Level 1)

```
Review-section (Level 1)
└── Container (Level 2) — container, d-flex, flex-column, gap: 80px
    ├── Heading Container (Level 3) — d-flex, flex-column, gap: 10px
    │   ├── H6 — "Practice Advice" (color: #FFFFFF)
    │   ├── H2 — "Approdable Packages"
    │   └── Paragraph
    └── Box Container (Level 3) — row, row-cols-3, justify-content-between, gap/gutter: 20px
        ├── Box 1 (Level 4) — padding: 25px, bg: #FFFFFF
        │   ├── Review (Level 5) — padding: 30px, d-flex flex-column gap: 15px
        │   │   ├── Stars (4 solid + 1 empty, color: #F3CD03)
        │   │   └── Review Content — "Slate helps you see how many more days..."
        │   └── Profile (Level 5) — d-flex gap: 15px
        │       ├── Img: feedback-regina-miles.jpg (border-radius: 50%)
        │       └── User box — H6: "Regina Miles" (color: #8D5CF6), Small: "Designer" (color: #252B42)
        ├── Box 2 — same structure, Img: feedbacks-miles.jpg
        └── Box 3 — same structure, Img: feedbacks-avatar.jpg
```

---

### 2.7 FAQ Section (Level 1)

```
FAQ (Level 1)
└── Container (Level 2) — d-flex, flex-column, gap: 50px
    ├── Heading Container (Level 3) — d-flex, flex-column, gap: 10px, padding: 45px 0
    │   ├── H2 — "FAQ"
    │   └── Paragraph — center-aligned
    └── Box Container (Level 2) — row, row-cols-3, gap: 30px, gutter-x: 30px, row-gap: 30px
        └── 9× Box (Level 3) — d-flex, padding: 25px, radius: 9px, bg: #FFFFFF
            ├── Icon: fa-angle-right (color: #8D5CF6)
            └── Content box — d-flex, flex-column, gap: 5px
                ├── H5 — "the quick fox jumps over the lazy dog" (color: #252B42)
                └── H6 — "Things on a very small scale behave like nothing" (color: #737373)
```

---

### 2.8 Newsletter Section (Level 1)

```
Newsletter (Level 1)
└── Container (Level 2) — d-flex, flex-column, gap: 80px
    ├── Heading Container (Level 3) — d-flex, flex-column, gap: 10px
    │   ├── H6 — "Newsletter" (color: #8D5CF6, text-align: center)
    │   ├── H2 — "Every Client Matters" (text-align: center)
    │   └── Paragraph — center-aligned
    └── Form (Level 3) — row, gap: 0
        ├── Input (Level 4) — col-8, border: 1px solid #E6E6E6, top-left-radius: 5px, bottom-left-radius: 5px
        └── Button (Level 4) — col-4, bg: #8D5CF6, top-right-radius: 5px, bottom-right-radius: 5px
```

---

## 3. Asset Manifest

| Asset | Path | Used In |
|---|---|---|
| Landing BG | `assets/images/landing-background.svg` | Hero ::before |
| School Icon | `assets/images/school.svg` | Feature Card 1 |
| Books Icon | `assets/images/books.svg` | Feature Card 2 & 3, Package Section |
| Expert Teacher | `assets/images/expert-teacher.svg` | Expert Section |
| Pencils | `assets/images/pencils.svg` | Course Card 1 |
| Library | `assets/images/library.svg` | Course Card 2 |
| Apple | `assets/images/apple.svg` | Course Card 3 |
| Like | `assets/images/like.svg` | Course Card overlay |
| Basket | `assets/images/basket.svg` | Course Card overlay |
| Views | `assets/images/views.svg` | Course Card overlay |
| Download | `assets/images/download.svg` | Course Card sales |
| Brain | `assets/images/brain.svg` | (Available, not used in extraction) |
| Healthy Check | `assets/images/healthy-check.svg` | (Available, not used in extraction) |
| Mail | `assets/images/mail.svg` | (Available, not used in extraction) |
| Map | `assets/images/map.svg` | (Available, not used in extraction) |
| Phone | `assets/images/phone.svg` | (Available, not used in extraction) |
| Regina Miles Photo | `assets/images/feedback-regina-miles.jpg` | Review Box 1 |
| Feedbacks Miles | `assets/images/feedbacks-miles.jpg` | Review Box 2 |
| Feedbacks Avatar | `assets/images/feedbacks-avatar.jpg` | Review Box 3 |
