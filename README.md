# networkgears.com

Marketing site for Network Gears, an IT infrastructure consultancy based in San Diego, CA. One static page, no build step, hosted on GitHub Pages from this repo. This README is the handoff for anyone (or any assistant) picking the project up.

## What is here

| File | Purpose |
|---|---|
| `index.html` | The entire site: HTML, CSS, and JS in one file (about 150 KB). No framework, no bundler. |
| `og.png` | Share image for links (1200x630), referenced from the `<head>`. |
| `404.html` | Not-found page in the same style. |
| `robots.txt`, `sitemap.xml` | Standard SEO files. |
| `CNAME` | `networkgears.com`, which makes GitHub Pages serve the custom domain. |

Working copy on Adib's Mac: `/Users/subspace/Desktop/Website Network Gears/index.html` (plus the same extra files). The previous site is kept there as `index-previous-2026-05.html`.

## How to deploy

GitHub Pages serves the `main` branch. Any push to `main` is live within about a minute.

From the Mac, one line in Terminal (assumes the deploy clone at `~/.networkgears-deploy/networkgears-site`; if it does not exist, run the first command once):

```
mkdir -p ~/.networkgears-deploy && cd ~/.networkgears-deploy && git clone https://github.com/adib-NG/networkgears-site && echo "✅ cloned"
```

```
cd ~/.networkgears-deploy/networkgears-site && git pull -q && cp "$HOME/Desktop/Website Network Gears/index.html" index.html && git add index.html && git commit -q -m "Update site" && git push -q && echo "✅ done"
```

Paste as a single line. Multi-line pastes in macOS Terminal tend to stop partway through.

Verify: `curl -sL https://networkgears.com/ | grep -c "<title>"` should print 1, and a hard refresh (Cmd+Shift+R) shows the change.

## Site structure (index.html)

Sections, in order, with their element IDs:

1. `#hero`: WebGL particle cloud (`#gl`, raw WebGL, no library) that morphs between six shapes: the gear logo, a globe with real continents, a network constellation, a server rack, the words NETWORK GEARS, a wave. Cursor repels particles, drag rotates, click scatters, scrolling blows it apart. `#net` is a 2D canvas fallback used only when WebGL is unavailable.
   - `#term-card` is an interactive terminal. It plays three demo scenes, then becomes a shell when clicked (or press `/`). Commands: `help`, `stack`, `where`, `how`, `book`, `go <section>`, `shape 1-6`, `size s|m|l`, `max`, `snake`, `incident`, `matrix`, `exit`. The title-bar dots work: red resets, yellow collapses, green maximizes.
2. `.marquee`: platform names, speed reacts to scroll velocity.
3. `#stack`: the seven service layers as tabs (`.layer` buttons) with a detail panel (`.detail`) per layer and an isometric stack diagram (`.slabs`). Accordion on mobile.
4. `#sketch`: "Sketch your environment" configurator. Sliders and toggles redraw an SVG architecture diagram (`#sk-svg`), update the recommendation list, and prefill a `mailto:` to hello@networkgears.com with the whole configuration. Four presets.
5. `#global`: 2D canvas globe (`#globe`) with continent dots from an embedded land mask (`LAND`, a base64 bitmask at 1.5 degree resolution), great-circle routes from San Diego to nine cities, drag to rotate, city chips, distance readout.
6. `#why`: light section. Flip rows (`#flip`) that turn over on hover/tap or all at once with the switch, plus four commitment cards.
7. `#how`: segmented control for the four engagement models with an animated phase timeline.
8. `#contact`: CTA. Email button plus a copy-address button.
9. `footer`.

Global effects: `#field` is a fixed full-page WebGL particle field behind the dark sections (desktop only); `#glow` is a soft cursor glow (desktop only, native cursor stays visible); `.rail` is the section dot rail on the right; `.wipe`, `.rise`, and `.kicker` animate in on scroll; `.spot`, `.spot-b`, `.tilt`, `.mag` are the cursor-tracking glow, border glow, 3D tilt, and magnetic button effects.

All motion respects `prefers-reduced-motion`. Hover effects are off on touch devices. The page is tested at 1440x900 and 390x844 and has no horizontal overflow at either.

Design tokens are CSS variables at the top of the `<style>` block (`--ink`, `--panel`, `--cyan`, `--fog`, fonts Sora and JetBrains Mono from Google Fonts). The logo is an inline SVG `<symbol id="logo">` (8-tooth rounded gear with a three-node network triangle in the hub) and is also the favicon as a data URI.

## Content that is editable text

- Headline and hero copy: `.hero-copy`.
- Stats: `.proof` (20+ years, 80+ businesses, 45+ countries).
- The seven layers: each `.layer` button and its matching `.detail` (title, paragraph, three bullets, tool chips).
- Globe cities: the `SITES` array in the globe script (name, lat, lon). Home is `HOME` (San Diego).
- Engagement models: the `models` array in the engagement script (name, for, body, starts, includes, billing, timeline, phases).
- Sketch recommendations and the email body: the `build()` function in the sketch script (rules are plain `if` statements).
- Terminal demo scenes and commands: `scenes` and `CMDS` in the terminal script. Incident game scenarios: `SCENARIOS`.
- Knowledge the chatbot uses is separate; see Quincer below.

Facts on the page that are Adib's claims and should stay accurate: 20+ years in infrastructure, 80+ businesses supported, delivered in over 45 countries, based in San Diego, CA. Tool chips (Okta, Jamf, CrowdStrike, Vanta, Terraform, etc.), the globe cities, and the engagement billing lines (hourly no minimum, 30-day notice, hours roll within the quarter) were drafted by the assistant and have not been confirmed line by line.

## Chat widget (Quincer)

The chat bubble is Quincer AI, loaded by the snippet just before `</body>`:

```html
<script>window.ChatWidgetConfig = { apiKey: "cw_live_...", apiUrl: "https://chat.quincer.com/api" };</script>
<script src="https://chat.quincer.com/api/embed/widget.js" defer></script>
```

The key is the public widget key (meant to be in the page). Everything else lives in the Quincer dashboard at https://chat.quincer.com/dashboard (account adib@networkgears.com, workspace "Network Gears"):

- Customize & Deploy: brand name, tagline "Ask an engineer", welcome message, colors (primary `#0891b2`, chat background `#0c1222`, text `#f4f7fc`), position bottom-left, compact launcher, proactive agent mode, required visitor info Name + Email + Company, file uploads on.
- Personas: Network Gears Assistant (default, front door and booking), Client services (billing, scheduling, hardware orders), Technical Support (triage only; anything down goes to hello@networkgears.com with URGENT in the subject).
- Knowledge: a crawl of networkgears.com plus 16 Q&A items (what we do, where, pricing stance, the free review, engagement models, MSP comparison, remote/multi-site, compliance, hardware sourcing, Mac/Windows, platforms, outage instructions, contact, the sketch tool, residential, timelines). After changing the site, click Rescan on the imported page in Knowledge.
- Not connected yet: a Gmail mailbox for follow-up emails (Integrations), a CRM, a voice plan (voice is enabled but has no minutes, so calls are rejected; either buy minutes or turn voice off).

The widget is positioned bottom-left on purpose: the terminal owns bottom-right in the hero.

## Decisions and history

- 2026-10-28: full revamp from the previous blue/white site to the current dark interactive one. Shop section (Shopify Buy Button for Mac Studios) was already gone before the revamp and was not carried over.
- Contact is email only (hello@networkgears.com). No contact form, no phone number on the site, by request.
- Nairobi was removed from the globe cities by request.
- The custom cursor was replaced by the native cursor plus a glow; the terminal's drag-to-resize corner was replaced by S/M/L buttons; the compare slider in Why was replaced by flip rows. All three after feedback that the drag interactions felt clunky.
- Terminal games (snake, incident) were added by request.

## Open items

1. Replace the `mailto:` booking with a real booking link (Calendly or similar). Biggest conversion win on the page.
2. Add proof: two or three client names or one-line results. The site has no social proof.
3. Analytics (Plausible or GA4).
4. Confirm the tool chips, globe cities, and billing lines listed under "Content" above.
5. Quincer: connect Gmail for follow-ups, decide on voice, review the 16 Q&A answers (especially the residential-networks one).

## Working with Adib

- Direct, concise, copy-paste-ready. No em dashes or en dashes anywhere, including in copy on the site and in the chatbot's replies.
- When revising a draft, give back the whole thing, not a snippet.
- Shell commands for his Mac as one `&&`-chained line ending in `&& echo "✅ done"`.
- He will say "push it" or "ship it"; that means commit to `main` and confirm it is live.
