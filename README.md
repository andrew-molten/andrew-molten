# AndrewMolten.com

A personal website for Andrew Molten’s independent web design and development services in Christchurch, New Zealand.

Built with **Next.js 16 App Router, TypeScript, React 19, and Tailwind CSS 4**. The homepage is statically rendered; the enquiry receiver is a server route. Compatible with Vercel’s Next.js runtime. The implementation has not been deployed or merged.

## Repository inspection

The supplied checkout contained only a profile `README.md`, with no Vite application, dependencies, environment files, deployment configuration, or image assets. Its Git history and origin are preserved. The original profile is retained in `docs/original-profile.md`. Andrew’s name, Christchurch location, and `andrewmolten23@gmail.com` were verified against that README. All website and SEO references use **andrewmolten.com**.

## Run locally

Use Node.js **22.18+** (Node 24 LTS recommended).

```sh
npm ci
npm run dev
```

Open http://localhost:3000.

```sh
npm run typecheck
npm test
npm run build
npm start
```

With the production server running, run the additional HTTP integration checks:

```sh
TEST_BASE_URL=http://localhost:3000 npm test
```

The HTTP tests skip when `TEST_BASE_URL` is unset. No test sends email or contacts a real webhook.

## Design direction

Warm ivory, dark olive ink, muted terracotta, and a pale sage about section. Instrument Serif gives the headlines a personal, editorial feel; DM Sans keeps the practical details clear. Fonts are served locally from installed packages, with no browser requests to a font service. A restrained layout puts Andrew’s business experience and direct service ahead of technical credentials. Desktop and mobile compositions adapt without carousels, animation libraries, or stock photography. Keyboard focus styles, labelled fields, a skip link, and reduced-motion support are included.

## Content to supply

- A natural personal portrait. Put it in `public/images/` and set `site.portraitImage` to its local path in `src/lib/site.ts`. The responsive `next/image` slot will replace the labelled monogram placeholder automatically.
- Actual desktop and mobile screenshots of both projects. Set `desktopImage` and `mobileImage` for each project in the same file. The existing device frames show clearly labelled placeholders, not invented project screenshots.
- Verified live URLs for Andrew Bolton Sports Massage and Journalingo. Set each project’s `url`; this enables its live website link. No URL has been guessed.
- Confirm that **Andrew Bolton Sports Massage** is the desired public project name; it is retained exactly from the brief even though the personal brand is Andrew Molten.
- Confirm that the email from the original repository is the address to use publicly, or replace `site.email`.

## Enquiry form integration

The direct `mailto:` link is usable now. Until a valid receiver is configured, the form displays an honest explanation, disables its submit button, and `/api/enquiry` returns an unavailable response. It never simulates successful delivery.

1. Copy `.env.example` to `.env.local`.
2. Set `CONTACT_FORM_ENDPOINT` to a **trusted HTTPS webhook** that you control and have configured to deliver enquiries. It must accept this JSON shape:

   ```json
   {
     "name": "Alex Smith",
     "email": "alex@example.com",
     "website": "https://example.com",
     "message": "I would like to discuss my website."
   }
   ```

3. Optionally set `CONTACT_FORM_TOKEN`. The server adds it as an `Authorization: Bearer …` header. Neither setting is exposed to the browser.
4. The receiver must return a successful HTTP status **only after accepting the message for delivery**. Non-success responses and timeouts show an error with the email fallback.
5. Restart the development server. Because the homepage is static, rebuild for production after changing configuration. Set these server environment variables in the chosen hosting environment before its build.

The route validates and trims fields, enforces a 32 KB request limit, checks request origin, includes a honeypot, refuses webhook redirects, and applies a ten-second delivery timeout. The optional website accepts only HTTP(S) URLs. It forwards data without logging or retaining enquiry contents. Use the receiving service’s spam/rate controls before enabling public submissions; no distributed rate-limit store or third-party form service has been added. End-to-end delivery remains unverified until a real receiver is supplied.

## Future client previews

The `src/app/previews/` subtree is reserved for link-accessible concepts. It is separate from the homepage and has no public navigation links. Its layout sets `noindex`, `nofollow`, and `noarchive`; `next.config.ts` additionally sets an `X-Robots-Tag` response header on `/previews` and every descendant. The sitemap is an explicit allowlist containing only the homepage.

To add a future client, create:

```text
src/app/previews/business-slug/page.tsx
src/app/previews/business-slug/components/...
```

The static route takes precedence over the catch-all placeholder. It inherits preview metadata and headers. Keep its components and styles within its folder; use CSS modules to isolate client designs. Do not override the inherited robots metadata or add the route to the public sitemap. Unknown slugs currently return 404. No H2OFF redesign or other client concept has been built.

`robots.txt` intentionally lets crawlers access preview pages so they can read `noindex`; blocking crawling could prevent that directive from being seen. **Noindex does not provide privacy or access control.** Anyone with a direct link can access a future preview. Use authentication for confidential work.

Future outreach screenshots can be captured from these actual rendered routes at desktop and mobile viewport sizes. No screenshot-generation feature is included in this first version.

## Structure

```text
src/app/page.tsx                 Single-page public website
src/app/globals.css              Tailwind entry and visual system
src/app/layout.tsx               Fonts and shared SEO metadata
src/app/opengraph-image.tsx      Generated social sharing image
src/app/sitemap.ts               Homepage-only sitemap
src/app/robots.ts                Crawler configuration
src/app/api/enquiry/route.ts     Server-side webhook integration
src/app/previews/                Future isolated client concepts
src/components/                 Small shared UI components
src/lib/site.ts                  Verified identity and project asset slots
src/lib/enquiry.ts               Validation and integration configuration
tests/                          Input validation and HTTP integration checks
docs/original-profile.md        Preserved original repository content
```

No Vite dependencies or configuration are present. No public pricing, testimonials, results, or unverified claims have been added.
