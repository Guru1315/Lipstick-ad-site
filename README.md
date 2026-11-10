<div align="center">

<a href="https://roselle-lip-lab-delta.vercel.app"><img src="assets/banner.svg" alt="Roselle Lip Lab: a turning 3D lipstick twists its bullet up through eight shades while rose petals fall" width="100%"></a>

# Roselle · Lip Lab

A Black Friday lipstick store built in 3D. Eight lipsticks turn on a carousel, one lipstick opens up chapter by chapter as you scroll, and a ribbon and 150 petals whip into a tornado. Below that: a muse arc, a silk film, a tilting swatch bar, story-style reels, an SVG shade try-on and a winter edit on pedestals. **Every lipstick, product shot and editorial image is generated in the browser.**

[![Live demo](https://img.shields.io/badge/LIVE_DEMO-roselle--lip--lab--delta.vercel.app-f43a62?style=for-the-badge&logo=vercel&logoColor=fde8de&labelColor=4b0613)](https://roselle-lip-lab-delta.vercel.app)

![Next.js](https://img.shields.io/badge/Next.js-16-000?logo=nextdotjs) ![Three.js](https://img.shields.io/badge/Three.js-r170-049ef4?logo=threedotjs&logoColor=white) ![GSAP](https://img.shields.io/badge/GSAP-ScrollTrigger-88ce02?logo=greensock&logoColor=white) ![Lenis](https://img.shields.io/badge/Lenis-smooth_scroll-ff98a2) ![Tailwind CSS](https://img.shields.io/badge/Tailwind-v4-38bdf8?logo=tailwindcss&logoColor=white) ![Built with Claude Code](https://img.shields.io/badge/built_with-Claude_Code-d97757)

</div>

---

## 🎬 The 3D, live

<div align="center">

<a href="https://roselle-lip-lab-delta.vercel.app"><img src="assets/scroll-story.gif" alt="The liquid intro drains, the hero carousel turns to Ruby Rouge and Berry Noir, then scrolling opens the lipstick chapter by chapter until the petals swirl into a tornado" width="100%"></a>

<sub>Recorded from the live site: the liquid intro, two turns of the hero carousel, then a scroll through the four Inside chapters and the swirl. Click it to open the demo.</sub>

</div>

**Try these on the live site:**

| | |
|---|---|
| 💄 **Turn the hero** with ‹ › or the arrow keys. The front lipstick opens and its cap floats | 🖱️ **Scroll slowly through Inside.** The case opens, the bullet twists up, the ingredients orbit, then the petal tornado |
| 🎯 **Click a chapter ring** to jump straight to that chapter | 🃏 **Drag or flick** the muse arc, and press **Add** on a portrait |
| 🔊 **Turn the sound on** in *Feel the velvet* for a generated ambient pad | ✨ **Hover a swatch card**: it tilts toward the pointer |
| 📱 **Open a reel**: ← → skip, hold to pause, Esc to close | 👄 **Find your match**: pick a skin tone and a shade, then **See it in 3D** |
| 🎁 **Hover a lipstick** in the winter edit for its card | 💋 **Seal a kiss** in the nav for a code, and open the **bag** |

---

## 📸 Screenshots

All taken from the live deployment at 1912 × 871.

<table>
  <tr>
    <td width="50%"><img src="screenshots/desktop/01-hero.jpg" alt="Hero carousel with 30% off"><br><sub><b>Hero</b>: eight shades on a fan, light boxes, pedestal</sub></td>
    <td width="50%"><img src="screenshots/desktop/02-the-case.jpg" alt="Chapter 01, Dressed in gold"><br><sub><b>01 · Dressed in gold</b>: labels pinned to the case</sub></td>
  </tr>
  <tr>
    <td><img src="screenshots/desktop/03-the-twist.jpg" alt="Chapter 02, Twist it up"><br><sub><b>02 · Twist it up</b>: the cap lifts, the bullet rises</sub></td>
    <td><img src="screenshots/desktop/04-the-formula.jpg" alt="Chapter 03, Ruby rouge, with ingredients"><br><sub><b>03 · Ruby rouge</b>: pigment, shea, vitamin E, rose hip</sub></td>
  </tr>
  <tr>
    <td><img src="screenshots/desktop/05-the-finish.jpg" alt="Chapter 04, Matte to gloss"><br><sub><b>04 · Matte to gloss</b></sub></td>
    <td><img src="screenshots/desktop/06-swirl.jpg" alt="Ribbon and petal tornado"><br><sub><b>The swirl</b>: a ribbon pours, petals become a tornado</sub></td>
  </tr>
  <tr>
    <td><img src="screenshots/desktop/07-muses.jpg" alt="Worn by muses arc"><br><sub><b>Worn by muses</b>: a draggable arc of ten portraits</sub></td>
    <td><img src="screenshots/desktop/08-film.jpg" alt="Feel the velvet silk film"><br><sub><b>Feel the velvet</b>: a live 3D silk loop, with sound</sub></td>
  </tr>
  <tr>
    <td><img src="screenshots/desktop/09-swatch-bar.jpg" alt="Pick your pout swatch bar"><br><sub><b>Pick your pout</b>: tilt cards for eight shades</sub></td>
    <td><img src="screenshots/desktop/10-reel-viewer.jpg" alt="Reel viewer open"><br><sub><b>On the lips</b>: the story-style reel viewer</sub></td>
  </tr>
  <tr>
    <td><img src="screenshots/desktop/11-shade-finder.jpg" alt="Find your match try-on"><br><sub><b>Find your match</b>: an SVG lip try-on, six skin tones</sub></td>
    <td><img src="screenshots/desktop/12-the-edit.jpg" alt="Top 7 shades on pedestals"><br><sub><b>The winter edit</b>: hover a lipstick for its card</sub></td>
  </tr>
</table>

### 📱 On phones (390 px)

<table>
  <tr>
    <td><img src="screenshots/mobile/01-hero.jpg" alt="Mobile hero"></td>
    <td><img src="screenshots/mobile/02-the-case.jpg" alt="Mobile chapter 01"></td>
    <td><img src="screenshots/mobile/03-the-formula.jpg" alt="Mobile chapter 03"></td>
    <td><img src="screenshots/mobile/04-swirl.jpg" alt="Mobile swirl"></td>
  </tr>
  <tr>
    <td><img src="screenshots/mobile/05-film.jpg" alt="Mobile film"></td>
    <td><img src="screenshots/mobile/06-reel-viewer.jpg" alt="Mobile reel viewer"></td>
    <td><img src="screenshots/mobile/07-shade-finder.jpg" alt="Mobile shade finder"></td>
    <td><img src="screenshots/mobile/08-bag.jpg" alt="Mobile bag"></td>
  </tr>
</table>

---

## 🛠️ How it works

- **One WebGL renderer for the whole page.** A single canvas sits behind the HTML. The hero, the Inside stage and the silk film each register as a "view" tied to a DOM element, and each draws into its own scissor rectangle.
- **The lipstick is built in code.** One class makes the lathed case, rose-gold rings, slanted bullet and cap, and two numbers drive it: `open` and `twist`. Anchor points are projected to the screen every frame, so the labels stay pinned while it turns.
- **One scroll value tells the story.** The Inside section blends between five poses as you scroll, then a second value pours the ribbon and spins 150 instanced petals into a tornado.
- **Fast on a laptop GPU.** The first build took 33.6 s to appear. It now takes about 7 s, thanks to:
  - shared shader programs
  - background `compileAsync` warm-ups
  - all 28 product shots rendered into one atlas
  - revealing the page before the background art is done
- **Photos without photos.** The muse cards, reels and film stills are composed on canvases from the rendered products and a lip shape drawn in code.

Full details are in **[HOW-IT-WORKS.md](HOW-IT-WORKS.md)**.

> **Concept site.** Roselle is a fictional brand. The shades, prices and Black Friday sale are made up. The bag, checkout and "Seal a kiss" code are demos, and nothing is sold.
