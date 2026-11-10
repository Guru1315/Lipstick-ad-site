# How it works

The live site is at **https://roselle-lip-lab-delta.vercel.app**. It's one page, built with Next.js 16, React 19, TypeScript, Tailwind CSS v4, Three.js, GSAP ScrollTrigger and Lenis.

The reference repo held only notes, prompts and screenshots: no code, photos or video. So everything you see is drawn by code at load time:

- the lipsticks, props and ingredients are Three.js geometry
- the product shots are rendered from those models
- the editorial "photos" are composed on canvases
- the preloader liquid is a shader

Apart from the favicon, the page loads **no photos or image assets**.

## Page order

| # | Section | What moves |
|---|---|---|
| 0 | Preloader | A cherry liquid floods the screen. "ROSELLE" surfaces, arches drain the liquid, and the logo flies into the nav |
| 1 | **Hero**: "30% off everything" | Eight 3D lipsticks on a fanned carousel around a turning pedestal. The front one opens, its cap floats, and two "BLACK FRIDAY" light boxes glow behind |
| 2 | **Inside**: four chapters plus the swirl | One lipstick blends between five poses as you scroll: *Dressed in gold*, *Twist it up*, *Ruby rouge* (the ingredients orbit it), *Matte to gloss*. Then a ribbon pours and 150 petals whip into a tornado |
| 3 | Worn by muses | Ten portrait cards on a curved arc. Drag or flick it, and it drifts on its own |
| 4 | Feel the velvet | A rippling silk 3D film loop that cycles four shades. The sound chip plays a generated ambient pad |
| 5 | Pick your pout | The swatch bar: eight product cards that tilt toward the pointer. Each has Add and Next controls |
| 6 | On the lips | Six "reels". Tap one to open a story viewer: arrows skip, hold pauses, Esc closes |
| 7 | Find your match | An SVG lip try-on. Pick a skin tone and a shade, the colours blend, and "See it in 3D" spins the hero to that shade |
| 8 | Top 7 Roselle Shades for Winter 2026 | Lipsticks on pedestals. Hover (or tap on phones) for a card with Add |
| – | Nav | Section pill with an active marker, a bag with a count and dropdown, and "Seal a kiss" |

## 1. One WebGL renderer for the whole page

```text
one <canvas> fixed behind the HTML, one THREE.WebGLRenderer

views = [hero, inside-stage, film-frame]       ← each one is tied to a DOM element
every frame:
  for each view whose element is on screen:
    rect = element.getBoundingClientRect()
    renderer.setScissor(rect); renderer.setViewport(rect)
    view.render(renderer, rect, time)           ← its own scene and camera
```

Browsers only allow a few WebGL contexts per page, and each one costs memory and shader compiles. A single renderer with scissored "views" lets three separate 3D scenes sit inside normal page layout, and the HTML text sits on top of them.

## 2. The lipstick is built in code

`Lipstick` is one class used everywhere:

- **Parts:**
  - a lathed case with rose-gold rings
  - a collar
  - a slanted bullet (cut at 45°)
  - a cap
- **Lettering:** "ROSELLE" is painted onto a canvas texture.
- **Controls:** two numbers drive it. `open` lifts the cap away, and `twist` raises the bullet out of the collar.
- **Anchors:** points on the ring, lacquer, base, tip, collar and cap are projected to the screen every frame. That's how the Inside labels ("Magnetic click", "Slanted bullet"…) stay pinned to the right spot while the lipstick turns.

The hero turns its ring of eight lipsticks by easing a single `rot` value toward the chosen index, the shortest way round. The lipstick in the front slot opens, and the slot at the back shrinks to nothing, so you never see the wrap.

## 3. The Inside story is one scroll value

```text
#inside is tall; its stage is sticky
progress 0 → 0.60   step  = 0 → 4      (four chapters, then the swirl pose)
progress 0.64 → 1   storm = 0 → 1      (ribbon pours, petals become a tornado, lipstick lifts away)

pose = blend(POSES[floor(step)], POSES[ceil(step)], smoothstep(fract(step)))
```

Each pose is just position, rotation, scale, `open` and `twist`, and the in-between frames come from blending two poses. The ingredients (pigment, shea, vitamin E, rose hip) fade in around chapter 3. The 150 petals are one `InstancedMesh`: 26 drift during the chapters, and all of them join the tornado.

The stage is split into a back layer (the 3D) and a front layer (the text and labels), both sticky. That way the text always draws above the canvas.

## 4. Making it load fast on a laptop GPU

The first build took **33.6 s** to reveal on Intel UHD graphics. The profiler showed nearly all of that was WebGL shader compiling, at about half a second per program. The fixes:

- **One light rig and one material recipe everywhere.** Every material is a `MeshPhysicalMaterial` with the same features turned on, so all the scenes share a handful of shader programs instead of dozens.
- **No transmission and no extra instancing** on props. Each of those creates a new program.
- **`compileAsync` warm-ups.** Each scene compiles in the background while the liquid intro plays, with the 3D views paused.
- **One atlas for all product shots.** The 28 product images (8 open, 8 closed, 8 bare, 4 props) are rendered into one big texture and read back once, instead of 28 separate renders and readbacks.
- **Reveal early.** The page opens as soon as the fonts and the live scenes are ready. The product shots and editorial art finish in the background.

Result: about **7 s** on a warm visit and **10 s** cold, and the designed intro animation itself is 4.1 s.

## 5. "Photos" without photos

`art.ts` composes every editorial image on a 2D canvas from six templates:

- macro
- lineup
- spotlight
- swatch
- kiss
- petals

It uses the product shots from the atlas and a lip shape drawn in code (`lips.ts`, six skin tones), then saves each result as a JPEG blob URL. The muse cards, reels and film stills all come from this.

## 6. Smaller pieces

- **Lenis + GSAP.** Lenis smooth scroll runs on GSAP's ticker, so the ScrollTrigger timelines never drift from it.
- **One strip engine (`useStrip`)** drives the muse arc, swatch bar and reels. It handles drift, pause on hover, drag and flick with momentum, the wheel, buttons and keys, and it stops when off screen.
- **Shade Finder.** An SVG with gradient lips, a pores noise filter and gloss highlights. Its colours tween over 650 ms. Dark shade names are lifted so they stay readable on the wine panel.
- **Fonts on canvas.** `next/font` renames font families, so canvas text reads the real family name from the CSS variable and redraws once the font has loaded.

## Tested

The tests ran in headless Chrome against the live URL, at 1912 × 871 and 390 × 844, with real mouse, touch and keyboard input. They covered:

- hero next/prev and arrow keys, and nav anchors
- the chapter buttons
- muse drag and Add, and the film sound toggle
- swatch tilt, Add and Next
- the reel viewer: open, skip, hold to pause, Add, Esc
- Shade Finder, then "See it in 3D"
- The Edit hover card and Add
- the bag dropdown and "Seal a kiss"

No console errors, no broken images, and no sideways scroll.
