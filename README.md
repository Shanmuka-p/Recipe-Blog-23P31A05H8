# 🍳 Culinary - Multilingual Recipe Blog

![Next.js](https://img.shields.io/badge/Next.js-14-black?style=for-the-badge&logo=next.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue?style=for-the-badge&logo=typescript&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Contentful](https://img.shields.io/badge/Contentful-CMS-warmred?style=for-the-badge&logo=contentful&logoColor=white)

**Culinary** is a modern, high-performance recipe blog designed to demonstrate advanced web development concepts. Built with **Next.js** and **Contentful CMS**, it features **Static Site Generation (SSG)** for optimal performance, full **Internationalization (i18n)** support, and a robust **Dockerized** deployment pipeline.

---

## 📑 Table of Contents
- [🚀 Features](#-features)
- [🛠️ Tech Stack](#-tech-stack)
- [🏗️ Architecture & Design Decisions](#-architecture--design-decisions)
- [📦 Installation & Setup](#-installation--setup)
- [📂 Project Structure](#-project-structure)
- [✅ Core Requirements Verification](#-core-requirements-verification)
- [📄 License](#-license)

---

## 🚀 Features

* **🌍 Multilingual Support:** Full content localization in **English (en)**, **Spanish (es)**, and **French (fr)**. The UI and content switch dynamically based on the user's preference.
* **⚡ Static Site Generation (SSG):** Homepage and individual recipe pages are pre-rendered at build time for lightning-fast Time-to-First-Byte (TTFB) and superior SEO.
* **🔍 Client-Side Search & Filter:** Instant search capabilities and category filtering on the `/recipes` page without server round-trips.
* **📱 Responsive & Professional UI:** A mobile-first design built with **Tailwind CSS**, featuring a clean Slate & Emerald color palette.
* **🖼️ Optimized Media:** Automatic image optimization using `next/image` to prevent layout shifts and reduce bandwidth.
* **🐳 Fully Containerized:** A production-ready Docker setup that builds the app and serves it on port 3000.
* **📡 SEO Best Practices:** Automatic `sitemap.xml` generation and dynamic metadata for social sharing (OpenGraph/Twitter Cards).
* **🖨️ Print Optimization:** Specific print styles to hide navigation and footers when printing recipes.

---

## 🛠️ Tech Stack

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Framework** | [Next.js 14](https://nextjs.org/) | Core React framework for SSG/SSR |
| **Language** | [TypeScript](https://www.typescriptlang.org/) | Type safety and code robustness |
| **Styling** | [Tailwind CSS](https://tailwindcss.com/) | Utility-first styling with Typography plugin |
| **CMS** | [Contentful](https://www.contentful.com/) | Headless CMS for managing recipes & assets |
| **Data Fetching** | GraphQL | Efficient data querying from Contentful API |
| **State Management** | React Hooks | `useState` and `useContext` for local state |
| **i18n** | `next-i18next` | Internationalization routing and translation |
| **DevOps** | Docker | Containerization and orchestration |

---

## 🏗️ Architecture & Design Decisions

### 1. Rendering Strategy: Static Site Generation (SSG)
We chose **SSG** (`getStaticProps` / `getStaticPaths`) for the **Homepage** and **Recipe Details** pages.
* **Reasoning:** Recipe content is relatively static and does not change frequently. Pre-rendering these pages ensures the best possible performance and SEO ranking.
* **Implementation:** Pages are generated at build time. ISR (Incremental Static Regeneration) can be easily enabled via the `revalidate` prop if content updates become more frequent.

### 2. Client-Side Search vs. Server-Side Search
The **All Recipes** page (`/recipes`) fetches all recipe data at build time but handles searching and filtering in the browser.
* **Reasoning:** For a typical blog with hundreds of recipes, client-side filtering provides an **instant** user experience (0ms latency). It avoids the overhead of making API calls to the server for every keystroke.

### 3. Internationalization (i18n) Strategy
We utilize a hybrid approach for translations:
* **Static UI Text:** Managed via `public/locales/*.json` files using `next-i18next`. This keeps the codebase clean and separates content from code.
* **Dynamic Content:** Fetched directly from Contentful's **Localization API**. This allows content editors to manage translations for recipes (Ingredients, Instructions) without developer intervention.

### 4. Headless CMS Integration
We decoupled the frontend from the backend using **Contentful**.
* **Reasoning:** This allows for a flexible content model where recipes, authors, and categories are managed independently of the presentation layer. It supports the "Jamstack" architecture.

---

## 📦 Installation & Setup

Follow these steps to get the application running locally using Docker.

### Prerequisites
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running.
* Git installed.

### Step 1: Clone the Repository
```bash
git clone https://github.com/Shanmuka-p/Recipe-Blog-23P31A05H8.git
cd Recipe-Blog-23P31A05H8

```

### Step 2: Configure Environment Variables

The application requires connection details for Contentful. Create a `.env.local` file based on the example.

```bash
cp .env.example .env.local

```

Open `.env.local` and populate it with your credentials:

```ini
# CMS Provider
CMS_PROVIDER='contentful'

# Contentful API Credentials
CONTENTFUL_SPACE_ID='your_space_id'
CONTENTFUL_ACCESS_TOKEN='your_delivery_token' # Content Delivery API Token
CONTENTFUL_PREVIEW_ACCESS_TOKEN='your_preview_token'

```

*> **Note:** `.env.local` is ignored by git to protect your secrets.*

### Step 3: Run with Docker

Start the application using Docker Compose. This command builds the image and starts the container.

```bash
docker-compose up --build

```

### Step 4: Access the Application

Once the terminal shows "Ready", open your browser:

* **App:** [http://localhost:3000](https://www.google.com/search?q=http://localhost:3000)
* **Sitemap:** [http://localhost:3000/sitemap.xml](https://www.google.com/search?q=http://localhost:3000/sitemap.xml)

---

## 📂 Project Structure

```bash
├── components/          # Reusable UI components
│   ├── RecipeCard.tsx   # Displays recipe preview
│   ├── Newsletter.tsx   # Subscription form component
│   └── LanguageSwitcher.tsx # Toggle for en/es/fr
├── lib/                 # Utility libraries
│   └── api.ts           # Contentful GraphQL API client
├── pages/               # Next.js Pages & Routing
│   ├── index.tsx        # Homepage (SSG)
│   ├── recipes/
│   │   ├── [slug].tsx   # Recipe Details (Dynamic SSG)
│   │   └── index.tsx    # Search & Filter Page
│   └── _app.tsx         # Global App layout & providers
├── public/              # Static Assets
│   └── locales/         # i18n JSON files
├── styles/              # Global CSS & Tailwind setup
├── types/               # TypeScript interfaces
├── Dockerfile           # Docker image configuration
└── docker-compose.yml   # Docker orchestration

```

---

## ✅ Core Requirements Verification

This project has been verified against the submission rubric:

* [x] **Dockerized:** Application runs via `docker-compose up`.
* [x] **Env Config:** `.env.example` provided; `.env.local` ignored.
* [x] **i18n:** Supports `en`, `es`, `fr` with persistent language switcher.
* [x] **Homepage:** Statically generated with featured recipes.
* [x] **Recipe Detail:** Dynamic routing with localized content.
* [x] **Search:** Client-side filtering by text and category.
* [x] **Newsletter:** Form with validation and feedback states.
* [x] **Images:** Optimized using `next/image`.
* [x] **Sitemap:** Auto-generated at `/sitemap.xml`.
* [x] **Social Share:** Twitter sharing integration.
* [x] **Print Styles:** optimized print layout.

---

## 📄 License

This project is open-source and available under the **MIT License**.

