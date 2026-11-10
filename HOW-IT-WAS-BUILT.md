# How it was built

These are the prompts used to build and ship this project with [Claude Code](https://claude.com/claude-code), copied word for word and in order. The notes under each one say what it led to.

---

### 1. Start the rebuild

```text
ok its done now /clone-website https://github.com/alagappan567/lipstick-3d-site-notes
```

This ran the `clone-website` workflow in an AI website-cloner template (Next.js 16, Tailwind v4).

- **Read the reference.** The repo isn't a website. It holds a README, the author's build notes, the prompts they used for each section, and 11 screenshots of **Roselle**, a Black Friday lipstick store. There's no code, no photos, no video and no live site. So the screenshots were the target and the notes were the spec.
- **Built it with nothing to copy.**
  - Every lipstick, cap, pedestal, cherry, petal and ingredient is Three.js geometry, and every product shot is rendered from those models.
  - The editorial "photos" (muse cards, reels, film stills) are composed on canvases.
  - The preloader is a liquid shader, which is the fallback the notes themselves describe.
  - All three 3D areas (hero carousel, the Inside story, the silk film) share **one WebGL renderer**, drawn with scissor rects.
- **Fixed what broke:**
  - **Preloader.** The liquid never appeared because its inside test was upside down.
  - **Stacking.** The story text sat under the 3D canvas, so the sticky stage was split into back and front layers. The film frame was covering its own 3D, and the preloader logo was trapped under the raised canvas; both were fixed.
  - **Clipped letters.** The serif's tall letters were cut off by the word-rise masks.
  - **Font names.** `next/font` renames font families, so canvas lettering fell back to a default font. Canvas text now reads the real family name.
- **Made it load fast.** The first version took **33.6 s** to show the page on an Intel laptop GPU. A profile showed shader compiling was nearly all of it. Changes:
  - one light rig and one material recipe everywhere
  - no transmission
  - background `compileAsync` warm-ups during the intro
  - all 28 product shots rendered into one atlas
  - the page opens before the background art is done

  It now opens in about **7 s** warm and **10 s** cold.
- **Checked it.**
  - Lint, typecheck and the production build pass.
  - Headless-Chrome passes at 1912 × 871 (the width of the author's screenshots) and 390 × 844 drive every control with real mouse, touch and keyboard input.

### 2. Ship it

```text
Push and deploy
```

- **Deployed.** A new Vercel project, `roselle-lip-lab`, went to production at **https://roselle-lip-lab-delta.vercel.app**.
- **Re-tested live.** Both widths were re-run against the live URL: every chapter, every interaction, no console errors, no failed requests and no sideways scroll.
- **Built this showcase.** It holds the showcase only, never the source code, and contains:
  - screenshots from the live site at desktop and phone width
  - `assets/scroll-story.gif`, recorded from the live site in headless Chrome
  - `assets/banner.svg`, a hand-written animated SVG: a turning 3D lipstick whose bullet twists up through all eight shades, a floating cap, falling petals and a chapter ticker
  - this file and [HOW-IT-WORKS.md](HOW-IT-WORKS.md)

### 3. Publish the showcase

```text
push and commit only that assessts and screenshots and how it works and how it was build with 3d animated readme section with live url in this https://github.com/Guru1315/Lipstick-ad-site
```

This repo was published with only the showcase: `assets/`, `screenshots/`, [HOW-IT-WORKS.md](HOW-IT-WORKS.md), this file, and the animated README with the live link.
