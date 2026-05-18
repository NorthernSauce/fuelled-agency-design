# Fuelled Agency - Homepage v2 Design Brief

Written: 2026-04-29
Source files: `design/v2/index.html`, `design/v2/styles.css`
Strategy source: `strategy/strategy.md`

---

## 1. Project context

Fuelled Agency is a UK-based full-stack social media production company. Owner-led. Crew on site. One flat monthly fee from £475.

**ICP:** Owner-led businesses (yachting, hospitality, food & drink, lifestyle) where the founder is the brand but doesn't have time, taste, or team to make watchable content. They are great in real life and invisible online.

**Job to be done by the homepage:** convert a perfect-fit visitor into a booked discovery call.

**Competitors and what they get wrong:**
- Big agencies talk to brand teams that don't exist in these businesses.
- Cheap freelancers schedule a few captions on a phone and ghost.
- Nobody else turns up with a crew, directs the founder on camera, and runs the whole thing for one number.

That gap is the entire positioning.

---

## 2. Design system

### 2.1 Colour (OKLCH, brand lime as the only accent)

| Token | Value | Use |
|---|---|---|
| `--c-paper` | oklch(98% 0.005 195) | Light page surface |
| `--c-cream` | oklch(97% 0.012 90) | Warm bento card surface |
| `--c-base` | oklch(99% 0.003 195) | Cool bento card surface |
| `--c-text` | oklch(20% 0.018 215) | Body text on light |
| `--c-text-soft` | oklch(42% 0.018 215) | Eyebrows, secondary copy |
| `--c-dark` | oklch(28% 0.030 198) | Primary dark surface |
| `--c-dark-deeper` | oklch(20% 0.025 198) | Page background behind floating bento sections |
| `--c-on-dark` | oklch(98% 0.008 195) | Text on dark |
| `--c-accent` | `#C1FF72` (brand lime) | Single accent. Buttons, selection, eyebrows on dark |

**Rules:**
- One accent. Lime only. No secondary brand colour.
- Neutrals are tinted toward 195–215 hue (slightly cool) for cohesion.
- Pure black and pure white never used.
- `::selection` swaps to lime/dark for personality.

### 2.2 Typography

- **Display:** Playfair Display (500 + italic 500). Used for all headings and `<em>` accents inside body copy.
- **Body:** Manrope (300/400/500/600/700).
- Scale uses fluid `clamp()` values from `--fs-100` (0.78rem) up to `--fs-monster` (12vw).
- `<em>` inside headings = italic Playfair. Used as the rhythm device in every section: a stated fact + an italic emphasis.

### 2.3 Spacing & radii

- 4-point spacing scale: `--sp-2` (4px) through `--sp-10` (96px).
- Section padding: `--sp-section` clamp(80px, 10vw, 160px).
- Bento radii: clamp(16px, 1.6vw, 28px) on the section, internal cards stepped down.
- Pill radius: `--r-pill` 9999px for buttons, eyebrows, ticker logos.

### 2.4 Layout pattern: floating bento sections

The page background is `--c-dark-deeper`. Every major section sits on top of it as a rounded panel with `margin: clamp(10px, 1vw, 16px)`. Sections alternate between cream/paper/dark fills. This is the visual signature of the page - nothing is edge-to-edge except the dark backdrop.

---

## 3. Page architecture

Order top to bottom:

1. **Nav** - transparent over hero, sticky, lime CTA pill
2. **Hero** - full-bleed blurred reel background, centered Playfair headline, italic emphasis, two CTAs
3. **Trusted by** - flush horizontal logo ticker, auto-scrolling
4. **Position** - Playfair H2 with two inline thumbnails baked into the headline ("Built [img] for owners who'd rather be on camera [img] than chase the algorithm"), sidebar paragraph + dual CTA
5. **Featured Work** - image-led bento grid (lead + portrait + wide + standard), captioned on hover, all link to case studies
6. **Our Service / What Changes** - 3 outcome cards (inbox, Sundays, grid). Each card is illustration + headline + sub. NOT a feature list.
7. **Manifesto / How we work** - full-width press-play section. Sticky pin, ambient looping video frames behind, three numbered pills (Owner-led / Full production / One price). Active pill swaps the background video.
8. **Testimonials** - bento wall mixing video tiles + quote tiles + stat tiles. Floating PiP widget allows continued playback while scrolling. Modal opens for full video stories.
9. **Insights / Learning Hub** - H2 "The questions other agencies dodge", 3 article cards
10. **End CTA** - monumental "Tell us what your business actually does." Two CTAs.
11. **Footer** - minimal, 4 columns, brand line top

---

## 4. Interaction & motion

### 4.1 Cursor
- Custom cursor via CSS vars `--mx`/`--my` driven by JS rAF loop.
- Two layers: instant dot (snaps), luminous teal orb (lerps behind).
- Hidden on touch devices via media query.

### 4.2 Buttons
- Three variants: `btn` (lime primary), `btn--ghost` (outlined dark), `btn--dark` (dark fill on light bg).
- `:has(.arrow)` applies asymmetric padding (capsule with arrow puck on the right). Symmetric padding when no arrow.
- Magnetic hover via separate vars (`--mx`, `--my`, `--lift`) so the hover lift composes with the cursor pull.

### 4.3 Reveals
- `.reveal` class + `IntersectionObserver` adds `is-visible`.
- Stagger via `--reveal-delay` inline style on adjacent siblings.
- Scroll-driven CSS animations use `animation-timeline: view()` where supported.

### 4.4 Manifesto press-play
- Section is taller than viewport. `.manifesto__pin` is `position: sticky; top: 0; height: 100vh`.
- Section uses `overflow: clip` (NOT `hidden` - `hidden` breaks sticky).
- JS scroll-progress switches active pill and crossfades the background video at thresholds.
- Heading and pills unpin together when the last pill lands (no bleed under heading).

### 4.5 Testimonials
- Bento mixes 3 video tiles, 2 quote tiles, 1 stats tile.
- Click any video tile = opens shared modal with `<video>`, transcript, stats panel, prev/next navigation.
- Modal: `max-height: calc(100dvh - clamp(40px, 8vw, 112px))`, content scroll, `min-height: 0` on grid children to prevent overflow.
- Floating PiP widget: persists current video at bottom-right after modal close, click to re-open.
- Avatars sourced from live site, fallback to brand logo for non-person testimonials.

### 4.6 Mobile menu
- Full-screen overlay, lime stagger reveal on links, embedded video testimonial preview, CTA pill.
- Burger icon visibility: must use `display: inline-grid` directly (not unset) - earlier `display: none` in a later @layer was clobbering it. Desktop hide via `@media (min-width: 881px) { display: none !important }`.

---

## 5. Copy & voice

### 5.1 Voice
Owner-to-owner. Plain English. Short sentences. Specific numbers. No agency jargon. No hedging. No three-part lists. No em dashes.

### 5.2 Humaniser rules in force (per `ns-humaniser`)
- **Hard ban: em dashes** anywhere in rendered copy. Use full stops or restructure.
- **No rule-of-three padding** ("for the business, for your week, for the customer").
- **No corporate PR words** (commitment, excellence, deliver value, multifaceted, comprehensive).
- **No fake analytical statements** ("This demonstrates...").
- **No copula avoidance** ("serves as" → just "is").
- **Specificity wins.** "From £475 a month, all in" beats "affordable monthly pricing."

### 5.3 Section-by-section copy (current state)

**Hero**
> Real in life. Invisible *online.* We fix that.
>
> Your business is great in real life. Your social doesn't show it. *We're the film crew that fixes that.* No new headcount. No long contracts.

**Position**
> Built [img] for owners who'd rather *be on camera* [img] than chase the algorithm.
>
> Big agencies talk to brand teams that don't exist in your business. Cheap freelancers schedule a few captions and ghost. Nobody else turns up with a crew, directs you on camera, and runs the whole thing for one flat number every month. *That's our lane.*

**Services / What Changes**
> Three things change. *The day we plug in.*
>
> Forget the feature list. *This is what actually changes* the day we plug in.

**Manifesto pills**
1. **Owner-led / No middlemen.** You work directly with the founder. The same person who runs your shoot edits the recap, schedules the posts, and replies at 9pm on a Tuesday. No account managers translating between you and the doer.
2. **Full production / Crew on site.** Big agencies want to be your strategist. Cheap freelancers schedule captions on a phone. Nobody else turns up with a crew. We bring the cameras, we direct on set, we make watchable content with you in front of the lens. That's the deliverable.
3. **One price / No surprises.** From £475 a month, all in. No surprise media fees. No 60-day notice clauses. No "it depends" answers on the first call. You see the number, you see the work, you keep the assets.

**Testimonials**
> From the owners we plug in for
>
> Don't take our word for it. Take theirs.

**Insights**
> The questions *other agencies dodge.*

**End CTA**
> Ready when you are
>
> Tell us what your business *actually does.*
>
> Thirty minutes on the phone. *We'll tell you straight whether content will move the needle for you*, before you've spent a penny. No proposal-by-stealth. No hard sell. If it isn't the right fit, we'll say so.

---

## 6. Asset inventory & status

### 6.1 In place
- Logos: Sunseeker SSYS, Bikini Island, Food Tours Balearics, DealerHive, Concepcio, Hadleys, Little Kickers, Zen, White Hot Wedding, Visa
- Hero/section imagery: yacht shoot Mediterranean, Bikini Island Mallorca, Food Tours TikTok, drone yachting
- Testimonial avatars sourced from live site (6 known mappings, 2 guessed: Jerry → 18.42.16, Thomas → 18.38.50)
- Placeholder ambient videos for manifesto background (`design/assets/*.mp4`)
- Placeholder testimonial videos

### 6.2 Outstanding
- **Real client video testimonials** - currently using stock/placeholder
- **Verbatim testimonial quotes** - current copy is paraphrased, flagged in `strategy.md §10`
- **Confirm avatar mappings** for Jerry and Thomas
- **Real Learning Hub article thumbnails and titles** (currently pulling from live site)

---

## 7. Decisions made & rationale

| Decision | Why |
|---|---|
| Bento sections on dark bg, not edge-to-edge | Differentiates from generic agency sites. Echoes Apple Control Center pattern. Nothing else in this market does it. |
| Single lime accent on cool neutrals | Brand lime is owned. Cool neutrals avoid AI purple/blue trap. |
| Playfair italic as rhythm device | Owner-led brand needs warmth. Italic accent inside every heading creates a recognisable cadence. |
| Manifesto = press-play, not scroll-stack | Scroll-stack archived (`manifesto-scrollstack-archive.html`). Press-play with ambient video is more cinematic and matches the "we make watchable content" promise. |
| Service section = What Changes (3 outcomes) not feature list | Strategy: features go on `/what-we-do`. Homepage sells the change, not the spec. |
| Custom cursor + magnetic buttons + PiP testimonial | Three small "how was this made?" details. Differentiation by craft, not by gimmick. |
| Mobile: keep all sections, drop scroll-driven JS | Manifesto stack collapses to vertical flow. PiP hides. Custom cursor hides. Performance over cleverness on touch. |

---

## 8. Known constraints / gotchas

- **`overflow: clip` not `hidden`** on any section that needs sticky inside it.
- **Layered CSS source order matters** - later `display: none` in `@layer components` will clobber earlier mobile-media `display: inline-grid` unless the mobile rule has `!important` on a desktop-only `min-width` query.
- **Tailwind v4 silently drops pure-CSS files with no `@apply`** - keep form/component CSS inside the same compiled stylesheet, not standalone.
- **Buttons use `:where()` in nav** to drop nav-link colour specificity to 0, otherwise `.nav a { color: white }` stomps `.btn` lime fill.
- **Modal grid children need `min-height: 0`** to allow `overflow-y: auto` to actually scroll.

---

## 9. Next steps (from where we are right now)

1. Source NS-branded Word template for client strategy doc (parked).
2. Replace placeholder testimonial videos with real client footage when supplied.
3. Get verbatim testimonial quotes from Fuelled (3 minimum, 6 ideal).
4. Build out remaining pages: `/what-we-do`, `/our-work`, `/about`, `/learning-hub`.
5. Wire homepage into `_ns/theme/blocks/` once design is locked.
