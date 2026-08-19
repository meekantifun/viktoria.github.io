# Chapter Pages Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build `chapter-1-the-child.html` as the first story chapter page and update `index.html` so the nav and "Enter the Record" button navigate to it with a smooth fade transition.

**Architecture:** Pure static HTML with embedded CSS and JS — no build tools. The chapter page follows the same single-file pattern as `index.html`. Page transitions use a CSS fade-in on load plus a JS `navigateTo()` helper that fades the page out before redirecting. The audio system is copied identically into the chapter page.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, animations), vanilla JS, Google Fonts (Cormorant Garamond + IM Fell English)

---

## File Map

| File | Action | Responsibility |
|---|---|---|
| `index.html` | Modify | Update nav to 8 chapters; wire enterBtn and chapter links to fade-navigate |
| `chapter-1-the-child.html` | Create | Full chapter page: nav, hero, content sections, audio |

---

### Task 1: Update index.html nav to all 8 chapters with correct links

**Files:**
- Modify: `index.html` (nav `.chapters` ul)

- [ ] **Step 1: Replace the chapters list**

Find in `index.html`:
```html
      <li><a href="#">I. THE CHILD</a></li>
      <li><a href="#">II. THE CRUCIBLE</a></li>
      <li><a href="#">III. THE KNAVE</a></li>
```

Replace with:
```html
      <li><a href="chapter-1-the-child.html">I. THE CHILD</a></li>
      <li><a href="chapter-2-the-crucible.html">II. THE CRUCIBLE</a></li>
      <li><a href="chapter-3-the-knave.html">III. THE KNAVE</a></li>
      <li><a href="chapter-4-the-new-house.html">IV. THE NEW HOUSE</a></li>
      <li><a href="chapter-5-the-children.html">V. THE CHILDREN</a></li>
      <li><a href="chapter-6-the-fatui.html">VI. THE FATUI</a></li>
      <li><a href="chapter-7-the-knave.html">VII. THE KNAVE</a></li>
      <li><a href="chapter-8-the-flame.html">VIII. THE FLAME</a></li>
```

- [ ] **Step 2: Verify**

Open `index.html` in a browser. The nav should show all 8 chapter names. Hovering any of them turns it red.

---

### Task 2: Add fade transition and wire enterBtn on index.html

**Files:**
- Modify: `index.html` (enterBtn JS block, `<style>` block)

- [ ] **Step 1: Add fade-in CSS**

Inside `index.html`'s `<style>` block, just before `</style>`, add:
```css
  @keyframes pageFadeIn{ from{ opacity:0; } to{ opacity:1; } }
  body{ animation: pageFadeIn 0.4s ease forwards; }
```

- [ ] **Step 2: Replace the enterBtn click handler**

Find:
```javascript
/* ================= entrance polish for CTA ================= */
document.getElementById('enterBtn').addEventListener('click', ()=>{
  document.body.style.transition = 'filter 0.4s ease';
  document.body.style.filter = 'brightness(1.6)';
  setTimeout(()=>{ document.body.style.filter = 'brightness(1)'; }, 180);
  // Wire this to the first chapter page once it exists, e.g.:
  // window.location.href = 'chapter-1-the-child.html';
});
```

Replace with:
```javascript
/* ================= entrance polish for CTA ================= */
function navigateTo(url){
  document.body.style.transition = 'opacity 0.3s ease';
  document.body.style.opacity = '0';
  setTimeout(()=>{ window.location.href = url; }, 300);
}

document.getElementById('enterBtn').addEventListener('click', ()=>{
  navigateTo('chapter-1-the-child.html');
});

document.querySelectorAll('.chapters a[href$=".html"]').forEach(a=>{
  a.addEventListener('click', e=>{
    e.preventDefault();
    navigateTo(a.getAttribute('href'));
  });
});
```

- [ ] **Step 3: Verify**

Open `index.html` in a browser. The page fades in on load. Clicking "Enter the Record" fades the page out before navigating.

---

### Task 3: Create chapter-1-the-child.html — skeleton, CSS, nav, transitions

**Files:**
- Create: `chapter-1-the-child.html`

- [ ] **Step 1: Create the file**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>I. The Child — The House of the Ember</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=IM+Fell+English:ital@0;1&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after{ box-sizing:border-box; margin:0; padding:0; }

  :root{
    --bg:#0a0808;
    --red:#b3202a;
    --red-bright:#d93030;
    --bone:#e8dcc8;
    --bone-dim:#a09070;
    --line:rgba(90,32,32,0.4);
    --nav-h:48px;
    --edge-pad:64px;
  }

  @keyframes pageFadeIn{ from{ opacity:0; } to{ opacity:1; } }

  html{ scroll-behavior:smooth; }
  body{
    background:var(--bg); color:var(--bone);
    font-family:'Cormorant Garamond', serif;
    line-height:1.7;
    animation: pageFadeIn 0.4s ease forwards;
  }
  a{ color:inherit; text-decoration:none; }

  /* ── NAV ── */
  .nav{
    position:sticky; top:0; z-index:100;
    height:var(--nav-h);
    background:rgba(10,8,8,0.92);
    border-bottom:1px solid var(--line);
    backdrop-filter:blur(8px);
    display:grid;
    grid-template-columns:auto 1fr auto;
    align-items:center;
    padding:0 24px;
  }
  .brand{
    display:flex; align-items:center; gap:8px;
    padding-right:16px;
    border-right:1px solid var(--line);
  }
  .brand span{ font-size:11px; letter-spacing:0.16em; color:var(--bone); white-space:nowrap; }
  .chapters-banner{ justify-self:center; display:flex; align-items:center; overflow:hidden; }
  .chapters{ display:flex; align-items:center; list-style:none; }
  .chapters li{
    font-size:10.5px; letter-spacing:0.1em; color:var(--bone-dim);
    white-space:nowrap; padding:0 12px;
    border-right:1px solid var(--line);
  }
  .chapters li:first-child{ border-left:1px solid var(--line); }
  .chapters li a{ transition:color 0.25s ease; }
  .chapters li a:hover{ color:var(--red-bright); }
  .chapters li.active a{ color:var(--red-bright); }
  .nav-right{ justify-self:end; display:flex; align-items:center; gap:14px; }
  .icon-btn{
    background:none; border:none; cursor:pointer; color:var(--bone-dim);
    width:20px; height:20px; display:flex; align-items:center; justify-content:center;
    transition:color 0.25s ease, filter 0.25s ease;
    filter:drop-shadow(0 1px 3px rgba(0,0,0,0.9));
  }
  .icon-btn:hover{ color:var(--bone); }
  .icon-btn.active{ color:var(--red-bright); filter:drop-shadow(0 0 6px rgba(179,32,42,0.7)); }
  .icon-btn svg{ width:16px; height:16px; }
</style>
</head>
<body>

<audio id="bg-audio" preload="auto" style="display:none"></audio>

<header class="nav">
  <div class="brand">
    <a href="index.html"><span>THE HOUSE OF THE EMBER</span></a>
  </div>
  <div class="chapters-banner">
    <ul class="chapters">
      <li class="active"><a href="chapter-1-the-child.html">I. THE CHILD</a></li>
      <li><a href="chapter-2-the-crucible.html">II. THE CRUCIBLE</a></li>
      <li><a href="chapter-3-the-knave.html">III. THE KNAVE</a></li>
      <li><a href="chapter-4-the-new-house.html">IV. THE NEW HOUSE</a></li>
      <li><a href="chapter-5-the-children.html">V. THE CHILDREN</a></li>
      <li><a href="chapter-6-the-fatui.html">VI. THE FATUI</a></li>
      <li><a href="chapter-7-the-knave.html">VII. THE KNAVE</a></li>
      <li><a href="chapter-8-the-flame.html">VIII. THE FLAME</a></li>
    </ul>
  </div>
  <div class="nav-right">
    <button class="icon-btn" id="musicToggle" title="Toggle music" aria-pressed="false">
      <svg class="icon-play" viewBox="0 0 24 24" fill="currentColor"><polygon points="5,3 19,12 5,21"/></svg>
      <svg class="icon-pause" viewBox="0 0 24 24" fill="currentColor" style="display:none"><rect x="5" y="3" width="4" height="18"/><rect x="15" y="3" width="4" height="18"/></svg>
    </button>
    <button class="icon-btn active" id="muteToggle" title="Mute" aria-pressed="false">
      <svg viewBox="0 0 24 24" fill="currentColor"><path d="M11 5 6 9H2v6h4l5 4V5z"/><path d="M15.5 8.5a5 5 0 0 1 0 7" stroke="currentColor" stroke-width="1.4" fill="none" stroke-linecap="round"/></svg>
    </button>
  </div>
</header>

<main>
  <!-- Hero added in Task 4 -->
</main>

<script>
function navigateTo(url){
  document.body.style.transition = 'opacity 0.3s ease';
  document.body.style.opacity = '0';
  setTimeout(()=>{ window.location.href = url; }, 300);
}
document.querySelectorAll('.chapters a[href$=".html"]').forEach(a=>{
  a.addEventListener('click', e=>{
    e.preventDefault();
    navigateTo(a.getAttribute('href'));
  });
});
document.querySelector('.brand a').addEventListener('click', e=>{
  e.preventDefault();
  navigateTo('index.html');
});
</script>

</body>
</html>
```

- [ ] **Step 2: Verify**

Open `chapter-1-the-child.html` in a browser. You should see:
- Nav with all 8 chapters; "I. THE CHILD" is red
- "THE HOUSE OF THE EMBER" brand link present
- Page fades in on load
- Clicking a nav link fades the page out before navigating

---

### Task 4: Add chapter hero section

**Files:**
- Modify: `chapter-1-the-child.html`

- [ ] **Step 1: Add hero CSS inside `<style>` before `</style>`**

```css
  /* ── CHAPTER HERO ── */
  .chapter-hero{
    position:relative;
    display:grid;
    grid-template-columns:1fr 1fr;
    min-height:420px;
    overflow:hidden;
    border-bottom:1px solid var(--line);
  }
  .hero-left{
    display:flex; flex-direction:column; justify-content:center;
    padding:60px 48px 60px var(--edge-pad);
    position:relative; z-index:2;
  }
  .chapter-number{
    font-weight:300; font-size:28px;
    color:var(--bone-dim); letter-spacing:0.1em;
    margin-bottom:4px;
  }
  .chapter-title{
    font-family:'IM Fell English', serif;
    font-size:72px; color:var(--bone); line-height:1;
    margin-bottom:20px;
  }
  .chapter-divider{ display:flex; align-items:center; gap:10px; margin-bottom:20px; }
  .chapter-divider .cdline{ flex:1; max-width:60px; height:1px; background:var(--red); }
  .chapter-divider .cddiamond{ width:6px; height:6px; background:var(--red); transform:rotate(45deg); }
  .chapter-quote{
    font-style:italic; font-weight:300; font-size:17px;
    color:var(--bone-dim); line-height:1.6; max-width:300px;
  }
  .hero-right{ position:relative; border-left:1px solid var(--line); overflow:hidden; }
  .hero-right img{ width:100%; height:100%; object-fit:cover; object-position:top center; display:block; }
  .hero-right-empty{
    width:100%; height:100%; min-height:420px;
    background:linear-gradient(160deg,#1a0a0a,#0d0505 40%,#0a0d12);
  }
  .chapter-hero::before,.chapter-hero::after{
    content:''; position:absolute; top:0; bottom:0; width:40px; z-index:1; pointer-events:none;
  }
  .chapter-hero::before{ left:0; background:linear-gradient(to right,rgba(140,20,20,0.15),transparent); }
  .chapter-hero::after{ right:0; background:linear-gradient(to left,rgba(140,20,20,0.15),transparent); }
```

- [ ] **Step 2: Replace `<main>` content**

Find:
```html
<main>
  <!-- Hero added in Task 4 -->
</main>
```

Replace with:
```html
<main>

  <section class="chapter-hero">
    <div class="hero-left">
      <div class="chapter-number">I.</div>
      <h1 class="chapter-title">The Child</h1>
      <div class="chapter-divider">
        <span class="cdline"></span>
        <span class="cddiamond"></span>
        <span class="cdline"></span>
      </div>
      <p class="chapter-quote">"All were born as blank pages.<br>Fate simply decides whose story<br>is worth writing."</p>
    </div>
    <div class="hero-right">
      <!--
        When you have a hero image, replace hero-right-empty with:
        <img src="Pics n Stuff/your-hero-image.jpg" alt="Chapter I — The Child">
      -->
      <div class="hero-right-empty"></div>
    </div>
  </section>

  <div class="chapter-sections">
    <!-- Entry sections added in Task 5 -->
  </div>

</main>
```

- [ ] **Step 3: Verify**

Refresh `chapter-1-the-child.html`. You should see:
- "I." in muted color, "The Child" in large serif below it
- Red line–diamond–line divider
- Italic quote
- Right half: dark gradient (ready for hero image)
- Subtle red glow on left and right edges

---

### Task 5: Add entry section styles and two placeholder content sections

**Files:**
- Modify: `chapter-1-the-child.html`

- [ ] **Step 1: Add entry section CSS inside `<style>` before `</style>`**

```css
  /* ── ENTRY SECTIONS ── */
  .chapter-sections{ padding:80px 0; }
  .entry-section{ display:grid; margin-bottom:80px; align-items:start; }

  .entry-section.img-left{
    grid-template-columns:280px 1fr;
    padding-left:var(--edge-pad);
  }
  .entry-section.img-left .entry-text{ padding:6px 80px 0 52px; }

  .entry-section.img-right{
    grid-template-columns:1fr 280px;
    padding-right:var(--edge-pad);
  }
  .entry-section.img-right .framed-image{ order:2; }
  .entry-section.img-right .entry-text{ order:1; padding:6px 52px 0 80px; }

  /* landscape modifier widens the image column */
  .entry-section.img-left.landscape{ grid-template-columns:380px 1fr; }
  .entry-section.img-right.landscape{ grid-template-columns:1fr 380px; }

  /* ── FRAMED IMAGE ── */
  .framed-image{
    position:relative;
    border:1px solid rgba(120,70,40,0.5);
    padding:8px; background:#080606;
  }
  .framed-image::before{
    content:''; position:absolute;
    border:1px solid rgba(140,80,40,0.25);
    inset:4px; pointer-events:none;
  }
  .frame-ornament{
    position:absolute; top:-12px; left:50%; transform:translateX(-50%);
    width:24px; height:24px; background:var(--bg);
    display:flex; align-items:center; justify-content:center;
    font-size:14px; color:var(--red); z-index:2;
  }
  .img-wrap{ width:100%; overflow:hidden; background:#0a0606; display:block; }
  .framed-image.portrait .img-wrap{ aspect-ratio:3/4; }
  .framed-image.landscape .img-wrap{ aspect-ratio:4/3; }
  .img-wrap img{ width:100%; height:100%; object-fit:cover; display:block; }

  /* ── ENTRY TEXT ── */
  .entry-heading-row{ display:flex; align-items:center; gap:10px; margin-bottom:20px; }
  .entry-heading-diamond{ width:8px; height:8px; background:var(--red); transform:rotate(45deg); flex-shrink:0; }
  .entry-heading{ font-size:12px; letter-spacing:0.22em; color:var(--bone); font-weight:600; text-transform:uppercase; }
  .entry-body p{ font-size:15px; color:var(--bone-dim); line-height:1.9; margin-bottom:16px; font-weight:300; }
  .entry-body p:last-child{ margin-bottom:0; }
```

- [ ] **Step 2: Replace the chapter-sections placeholder**

Find:
```html
  <div class="chapter-sections">
    <!-- Entry sections added in Task 5 -->
  </div>
```

Replace with:
```html
  <div class="chapter-sections">

    <!-- SECTION 1: portrait image left
         To use landscape: add "landscape" to both entry-section and framed-image classes -->
    <div class="entry-section img-left">
      <div class="framed-image portrait">
        <div class="frame-ornament">✦</div>
        <div class="img-wrap">
          <!-- Add image: <img src="Pics n Stuff/your-image.jpg" alt="description"> -->
        </div>
      </div>
      <div class="entry-text">
        <div class="entry-heading-row">
          <div class="entry-heading-diamond"></div>
          <div class="entry-heading">Section Title</div>
        </div>
        <div class="entry-body">
          <p>Story text goes here.</p>
        </div>
      </div>
    </div>

    <!-- SECTION 2: portrait image right -->
    <div class="entry-section img-right">
      <div class="framed-image portrait">
        <div class="frame-ornament">✦</div>
        <div class="img-wrap">
          <!-- Add image: <img src="Pics n Stuff/your-image.jpg" alt="description"> -->
        </div>
      </div>
      <div class="entry-text">
        <div class="entry-heading-row">
          <div class="entry-heading-diamond"></div>
          <div class="entry-heading">Section Title</div>
        </div>
        <div class="entry-body">
          <p>Story text goes here.</p>
        </div>
      </div>
    </div>

  </div>
```

- [ ] **Step 3: Verify layout**

Refresh `chapter-1-the-child.html`. You should see:
- Section 1: empty framed portrait image flush-left at `64px` from page edge, text column to the right
- Section 2: empty framed portrait image flush-right at `64px` from page edge, text column to the left
- Image left edges align with the "I." chapter number above them

- [ ] **Step 4: Verify landscape class**

Temporarily change Section 1 to `class="entry-section img-left landscape"` and its framed-image to `class="framed-image landscape"`. The image column should widen from 280px to 380px and the image box should be wider than tall. Revert both class changes after confirming.

---

### Task 6: Add audio system

**Files:**
- Modify: `chapter-1-the-child.html`

- [ ] **Step 1: Add the audio script block before `</body>`**

Add this immediately before `</body>`:

```html
<script>
/* ── background music (shuffled playlist) ── */
(function(){
  const audio    = document.getElementById('bg-audio');
  audio.volume   = 0.5;
  let playing    = false;
  const musicBtn = document.getElementById('musicToggle');
  const muteBtn  = document.getElementById('muteToggle');
  const iconPlay  = musicBtn.querySelector('.icon-play');
  const iconPause = musicBtn.querySelector('.icon-pause');

  const TRACKS = [
    'Pics%20n%20Stuff/Arlecchino%20House%20of%20Hearth%20Theme%20(Caliginous%20Hearthfire)%20%20Genshin%20Impact.mp3',
    'Pics%20n%20Stuff/Elogia%20Cinerosa.mp3',
    'Pics%20n%20Stuff/Revised%20Requiem.mp3',
    'Pics%20n%20Stuff/Sinister%20Mist.mp3'
  ];

  function shuffle(arr){
    for(let i = arr.length - 1; i > 0; i--){
      const j = Math.floor(Math.random() * (i + 1));
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
    return arr;
  }

  let queue = [];
  function loadNext(){
    if(queue.length === 0) queue = shuffle([...TRACKS]);
    audio.src = queue.pop();
    audio.load();
  }

  audio.addEventListener('ended', ()=>{ loadNext(); audio.play().catch(()=>{}); });

  function startMusic(){
    if(playing) return;
    playing = true;
    musicBtn.classList.add('active');
    musicBtn.setAttribute('aria-pressed', true);
    iconPlay.style.display  = 'none';
    iconPause.style.display = '';
    if(!audio.src) loadNext();
    audio.play().catch(()=>{});
  }

  loadNext();
  audio.play().then(()=>{
    playing = true;
    musicBtn.classList.add('active');
    musicBtn.setAttribute('aria-pressed', true);
    iconPlay.style.display  = 'none';
    iconPause.style.display = '';
  }).catch(()=>{});

  document.addEventListener('click', ()=>{ startMusic(); }, { once: true });

  musicBtn.addEventListener('click', ()=>{
    playing = !playing;
    musicBtn.classList.toggle('active', playing);
    musicBtn.setAttribute('aria-pressed', playing);
    iconPlay.style.display  = playing ? 'none' : '';
    iconPause.style.display = playing ? ''     : 'none';
    if(playing){ audio.play(); } else { audio.pause(); }
  });

  muteBtn.addEventListener('click', ()=>{
    audio.muted = !audio.muted;
    muteBtn.classList.toggle('active', !audio.muted);
    muteBtn.setAttribute('aria-pressed', audio.muted);
  });
})();
</script>
```

- [ ] **Step 2: Verify**

Refresh `chapter-1-the-child.html`. Click anywhere on the page — music should start and the play button should turn red and switch to a pause icon. Mute button should toggle audio muting.

---

### Task 7: Commit and push

**Files:**
- `index.html` (modified)
- `chapter-1-the-child.html` (created)

- [ ] **Step 1: Commit**

```bash
git add index.html chapter-1-the-child.html
git commit -m "Add chapter-1 page with nav, hero, content sections, and fade transitions"
```

- [ ] **Step 2: Push**

```bash
git push origin main
```

- [ ] **Step 3: Verify on GitHub Pages**

Wait 1–2 minutes for GitHub Pages to deploy. Then:
1. Open the GitHub Pages URL — page fades in
2. Click "Enter the Record" — page fades out, chapter-1 loads and fades in
3. "I. THE CHILD" is red in the nav
4. Clicking "THE HOUSE OF THE EMBER" brand fades back to the homepage

---

## How to add more content sections later

To add a new section to the chapter, copy one of the two section templates from Task 5 and paste it inside `.chapter-sections`. Use these class combinations:

| Image position | Image shape | Classes on `.entry-section` | Classes on `.framed-image` |
|---|---|---|---|
| Left | Portrait | `entry-section img-left` | `framed-image portrait` |
| Left | Landscape | `entry-section img-left landscape` | `framed-image landscape` |
| Right | Portrait | `entry-section img-right` | `framed-image portrait` |
| Right | Landscape | `entry-section img-right landscape` | `framed-image landscape` |

To add an image, place this inside `.img-wrap`:
```html
<img src="Pics n Stuff/your-filename.jpg" alt="brief description">
```
