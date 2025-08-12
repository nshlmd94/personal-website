# Anshul’s Web Notes — HTML, CSS, JS, CLI & GitHub

A compact, practical reference distilled from our build session. Use this when you forget a command, a CSS trick, or what to click in GitHub Pages.

---

## 0) What you built

* A single‑page, minimalist “About” site using **HTML + CSS** and light **JavaScript**.
* Typography with **Google Fonts** (Inter, Figtree, etc.).
* A **random accent color** on each refresh via a CSS variable set by JS.
* Custom **link underline animation** with `::after`.
* Local preview using `python3 -m http.server`.
* Deployed on **GitHub Pages**.

---

## 1) HTML essentials you used

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Your Name</title>
  <!-- Google Fonts example (Inter) -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@100;400;600;700&display=swap" rel="stylesheet">
  <style>/* CSS lives here */</style>
</head>
<body>
  <p class="intro">Hi, there.</p>
  <p>Content...</p>
  <script>/* JS lives here */</script>
</body>
</html>
```

**Notes**

* Put meta viewport for mobile.
* Fonts load via a `<link>` from Google Fonts; then use in CSS `font-family: 'Inter', sans-serif;`.

---

## 2) Typography (Google Fonts)

**Common patterns**

* Inter: `https://fonts.googleapis.com/css2?family=Inter:wght@100;400;600;700&display=swap`
* Figtree: `https://fonts.googleapis.com/css2?family=Figtree:wght@100;400;600;700&display=swap`
* Lora: `https://fonts.googleapis.com/css2?family=Lora:wght@400;500;600;700&display=swap`
* Nunito: `https://fonts.googleapis.com/css2?family=Nunito:wght@300;400;600;700&display=swap`
* Noto Serif: (weights vary by family/style). **Tip:** If a font doesn’t include a weight (e.g., 100/Thin), the browser will **fallback to the nearest available**.

**Using the font**

```css
body {
  font-family: 'Inter', sans-serif;
}
.intro {
  font-weight: 100; /* Thin if available */
  font-size: 2rem;
}
```

---

## 3) Layout, spacing & readability

* Constrain measure for readability: `max-width: 65ch;` or `max-width: 700–800px;`
* Vertical rhythm: increase `line-height` (`1.7–1.8`) and paragraph margins.
* **Padding shorthands**

  * `padding: 2rem;` → all four sides 2rem.
  * `padding: 0 2rem;` → top/bottom 0, left/right 2rem.
* **`rem` vs `px`**: by default `1rem = 16px` → `2rem = 32px`.

---

## 4) Colors & CSS variables

Define a fallback + a variable that JS can update:

```css
:root { --link-color: #6c5ce7; } /* fallback */

a { color: var(--link-color); text-decoration: none; position: relative; }
```

**Deep + bright palette (examples)**

```js
const colors = [
  "#e63946", // deep rosy red
  "#ff5733", // fiery orange
  "#ff006e", // vivid pink
  "#6a00f4", // electric purple
  "#3a0ca3", // indigo glow
  "#0f4c81", // rich ocean blue
  "#007f5f", // deep teal
  "#118ab2", // bold aqua
  "#f72585", // hot raspberry
  "#ef233c"  // crimson
];
```

**Random color each refresh**

```html
<script>
  const randomColor = colors[Math.floor(Math.random() * colors.length)];
  document.documentElement.style.setProperty('--link-color', randomColor);
</script>
```

**Optional: keep the same color across sessions**

```html
<script>
  const key = 'accent';
  let c = localStorage.getItem(key);
  if (!c) { c = colors[Math.floor(Math.random() * colors.length)]; localStorage.setItem(key, c); }
  document.documentElement.style.setProperty('--link-color', c);
</script>
```

---

## 5) Link underline animation (CSS)

**Why you saw a vertical line earlier**: you added a left border to the body (see §7). The underline effect itself needs `a { position: relative; }` so `::after` positions under the link.

**Animated underline**

```css
a { position: relative; text-decoration: none; }

a::after {
  content: "";
  position: absolute;
  left: 0; bottom: -1px;
  height: 1px; width: 100%;
  background-color: var(--link-color);
  transform: scaleX(0);          /* starts hidden (squashed) */
  transform-origin: left;        /* grow from the left edge */
  transition: transform 0.3s ease;
}

a:hover::after { transform: scaleX(1); } /* grow to full width */
```

**Concepts**

* `transform: scaleX(0→1)` = horizontal growth.
* `transform-origin: left` = animate from the left edge.
* `transition` = smooth animation.

---

## 6) Responsive tweaks (media queries)

Remove decorative elements and tighten padding on small screens:

```css
@media (max-width: 600px) {
  body { padding: 0 1rem; border-left: none; }
}
```

---

## 7) The left accent line

You added a visual signature using a left border on the main container/body:

```css
body { border-left: 3px solid var(--link-color); }
```

This is **intentionally** the vertical line you saw. The media query above removes it on narrow screens.

---

## 8) Micro‑typography & italics

* Single‑word emphasis: semantic `<em>word</em>` or styling with a class.

```html
I’m drawn to <em>possibility</em>...
```

---

## 9) Local preview (CLI)

**Open file directly**

* macOS: `open index.html`
* Windows: `start index.html`
* Linux: `xdg-open index.html`

**Run a local HTTP server** (serves current folder; defaults to `index.html`)

```bash
python3 -m http.server         # port 8000
python3 -m http.server 8080    # custom port
```

* Stop server: **Ctrl + C** in the terminal.
* If `run` says “command not found”: there is no `run` command; use `open`/`start` or the Python server.

---

## 10) Links, privacy & crawlers

* **Referrer header**: when someone clicks your link, the destination **may** see your site as the referrer.
* Hide referrer:

```html
<a href="https://example.com" rel="noreferrer">link</a>
```

* Ask search engines not to index your page:

```html
<meta name="robots" content="noindex, nofollow">
```

* If a link was live briefly and nobody clicked/crawled it, it’s effectively unseen. Backlinks can be found by tools (Ahrefs/SEMrush) **only if crawled/known**.

---

## 11) Git & GitHub workflow (from VS Code)

**First time in a folder**

```bash
git init
git add index.html
git commit -m "Initial commit"
```

**Connect to your GitHub repo**

```bash
git remote add origin https://github.com/USER/REPO.git
# If you see "remote origin already exists":
# git remote set-url origin https://github.com/USER/REPO.git

git branch -M main
```

**Authenticate & push**

```bash
git push -u origin main
```

* GitHub now requires a **Personal Access Token (PAT)** when the terminal asks for a password.
* macOS: cache creds securely:

```bash
git config --global credential.helper osxkeychain
```

* First‑time macOS Xcode tool prompt:

```bash
sudo xcodebuild -license   # type your Mac password (hidden) → scroll → type 'agree'
```

**Common errors & fixes**

* `fatal: not a git repository`: you’re not in an initialized repo → `git init` in the right folder.
* `remote origin already exists`: you’ve set a remote before → `git remote set-url origin <new-url>`.
* `Updates were rejected because the remote contains work that you do not have locally` (you edited on GitHub):

```bash
git pull origin main --rebase
# resolve conflicts if any, then
git push origin main
```

**Next changes**

```bash
git add .
git commit -m "Update content/design"
git push
```

---

## 12) GitHub Pages (hosting)

**Enable Pages**

1. Repo → **Settings** → **Pages**
2. Source: **main**; Folder: **/** (root) → **Save**
3. After \~30–60s, you’ll see: *“Your site is published at …”*

**Your URL pattern**

```
https://USERNAME.github.io/REPO-NAME/
```

**Temporarily hide the site**

* Turn off Pages: Settings → Pages → Source: **None** (unpublishes immediately), or
* Add `<meta name="robots" content="noindex, nofollow">`, or
* Temporarily remove `index.html` (404 until you restore).
* Making the repo private disables Pages.

**Finding the URL again**

* Repo → Settings → Pages → look for the green published banner.

---

## 13) Small UI patterns you used

* **Left accent line** via `border-left` for a personal signature.
* **Animated link underline** using `::after` + `transform`.
* **Random accent** via CSS var + JS.
* **Mobile tweak** via `@media (max-width: 600px)`.

---

## 14) Helpful snippets (copy‑paste)

**Email line**

```html
<p class="email">📬 <a href="mailto:a@nshlmd.com">a@nshlmd.com</a> — I check it more often than I should.</p>
```

**One‑word italic emphasis**

```html
I like rhythmic <em>production</em>-forward music.
```

**Accessible link focus (keyboard users)**

```css
a:focus { outline: 2px solid var(--link-color); outline-offset: 2px; }
```

---

## 15) Troubleshooting quick table

| Symptom                         | Likely Cause                          | Fix                                               |
| ------------------------------- | ------------------------------------- | ------------------------------------------------- |
| `zsh: command not found: run`   | No such command                       | Use `open index.html` or `python3 -m http.server` |
| Localhost stopped               | You hit `Ctrl + C` or closed terminal | Re-run `python3 -m http.server`                   |
| Browser shows folder list       | No `index.html` in served dir         | Add/rename to `index.html`                        |
| `fatal: not a git repository`   | Not in an initialized folder          | `git init` in the project folder                  |
| `remote origin already exists`  | Remote set earlier                    | `git remote set-url origin <url>`                 |
| Push rejected (remote has work) | You edited on GitHub                  | `git pull origin main --rebase`, then `git push`  |
| Auth failed on push             | Using password instead of PAT         | Create PAT → use as password; cache creds         |
| Xcode license prompt blocks git | macOS first-time tools                | `sudo xcodebuild -license` → `agree`              |

---

## 16) Nice next steps

* **Dark mode toggle** (CSS variables + a button).
* **Favicon** for polish.
* **Analytics** (e.g., Plausible) to see traffic without creepiness.
* **Custom domain** (DNS → CNAME to `username.github.io`).

---

## 17) Mini glossary

* **CSS variable**: `--name`, used with `var(--name)`; can be updated via JS.
* **Pseudo‑element**: `::after`/`::before` create decorative elements without extra HTML.
* **Transform/transition**: GPU‑friendly ways to animate (scale, translate) smoothly.
* **Media query**: Conditional CSS for specific viewport sizes.
* **Git rebase (pull --rebase)**: Replay your commits on top of latest remote history to avoid merge commits.

---

If you want, I can export this as a **Markdown file** in your repo or a **PDF** for your notes.

---

## 18) JavaScript essentials you used (and will forget)

**Language quickies**

* `const` (can’t be reassigned), `let` (block‑scoped), avoid `var`.
* Template literals: `` `Color is ${value}` ``.
* Arrays: `colors[Math.floor(Math.random()*colors.length)]`.
* Functions: `() => { ... }` (arrow) vs `function() { ... }`.

**DOM basics**

```js
// Wait for DOM if you manipulate elements immediately
document.addEventListener('DOMContentLoaded', () => {
  const link = document.querySelector('a');
});

// Set a CSS variable from JS
const c = '#e63946';
document.documentElement.style.setProperty('--link-color', c);
```

**Local storage**

```js
const KEY = 'accent';
let color = localStorage.getItem(KEY);
if (!color) {
  color = colors[Math.floor(Math.random()*colors.length)];
  localStorage.setItem(KEY, color);
}
document.documentElement.style.setProperty('--link-color', color);
```

**Events**

```js
document.querySelectorAll('a').forEach(a => {
  a.addEventListener('focus', () => console.log('focused link'));
});
```

---

## 19) CLI (Terminal) basics you used

**Navigation**

```bash
pwd            # print current folder
ls             # list files
cd <folder>    # change directory
cd ..          # up one level
mkdir <name>   # make folder
```

**Files**

```bash
cp a b         # copy
mv a b         # move/rename
rm file        # remove file (careful)
```

**Open/serve**

```bash
open index.html             # macOS open with default app
python3 -m http.server 8080 # serve current folder
# Stop server: Ctrl + C
```

**Common gotchas**

* `zsh: command not found: run` → there is no `run` command; use `open` or the Python server.
* Terminal asks password for `sudo` → it’s invisible as you type; press **Enter** after typing.

---

## 20) VS Code quick wins

* **Integrated Terminal**: View → Terminal (opens at project path).
* **Go Live** (extension): auto‑reload server; alternative to Python server.
* **Emmet**: `p>a` + **Tab** → `<p><a></a></p>`.
* **Format**: Shift+Option+F (macOS) / Shift+Alt+F (Win/Linux).
* **Multi‑cursor**: Option+Click (Alt+Click) or Cmd+D to select next.

---

## 21) Git deeper (day‑2 commands)

```bash
git status               # what changed
git diff                 # see changes
git log --oneline --graph --decorate --all

git restore <file>       # undo unstaged changes
# or
git checkout -- <file>

git switch -c feature    # create & switch to a new branch
# merge that branch back later
```

**When remote has new work (you edited on GitHub):**

```bash
git pull origin main --rebase
# resolve any conflicts, then
git push
```

**Change or fix the remote URL**

```bash
git remote -v
git remote set-url origin https://github.com/USER/REPO.git
```

**Auth tips**

* Use a **Personal Access Token (classic)** with `repo` scope as the password when pushing over HTTPS.
* Cache it: `git config --global credential.helper osxkeychain` (macOS).

---

## 22) HTML semantics & links

* Emphasis: `<em>` (semantic, usually italic) vs `<i>` (visual only).
* Strong: `<strong>` (semantic, usually bold).
* External links: `target="_blank" rel="noopener noreferrer"` for security & no referrer.
* Accessibility: keep link text meaningful (avoid “click here”).

---

## 23) CSS fundamentals you touched

**Box model**

* Content → Padding → Border → Margin.
* Set global box sizing to make layouts saner:

```css
* { box-sizing: border-box; }
```

**Positioning**

* `position: relative` on `<a>` so `::after` positions under it.
* `position: absolute` for the underline itself.

**Specificity (why a style “doesn’t work”)**

* Inline > ID > class/pseudo > element.
* Later rules win if specificity is equal.

---

## 24) Responsive design (recap)

```css
@media (max-width: 600px) {
  body { padding: 0 1rem; border-left: none; }
}
```

* Use `clamp()` for fluid font sizes:

```css
html { font-size: clamp(15px, 1.1vw + 12px, 18px); }
```

---

## 25) Debugging workflow

* **Inspect Element** (right‑click) → Styles & Computed panes.
* **Console** for JS errors; `console.log()` liberally.
* **Hard reload** if CSS seems stuck (disable cache in DevTools → Network tab).
* **Check selector**: does `querySelector` actually find the element? (`$0` in Console refers to selected element in Elements panel).

---

## 26) Privacy & indexing (quick reference)

```html
<meta name="robots" content="noindex, nofollow">  <!-- keep pages out of search -->
<a href="https://example.com" rel="noreferrer noopener" target="_blank">link</a>
```

* Brief links won’t notify sites unless clicked; crawlers only discover links they see.
* Disabling Pages: Repo **Settings → Pages → Source: None** to unpublish.

---

## 27) GitHub Pages extras

* **Custom domain**: add a `CNAME` file with your domain, set DNS CNAME → `USERNAME.github.io`.
* **`.nojekyll`**: add this empty file if you ever use folders starting with `_`.
* **Custom 404**: create `404.html` in root.

---

## 28) Update checklist (muscle memory)

1. Edit files in VS Code.
2. Preview locally (open or Python server).
3. `git add .`
4. `git commit -m "Describe change"`
5. `git pull origin main --rebase` (if you edited on GitHub too)
6. `git push`
7. Wait \~30s for Pages to update → refresh site.

---

## 29) Expanded glossary

* **Management theater**: performative leadership (optics > outcomes).
* **Deterministic vs probabilistic**: single certain outcome vs range/likelihoods.
* **Media query**: conditional CSS for viewport/device.
* **Pseudo‑element**: generated content (`::before`, `::after`).
* **Referrer**: header that may reveal the linking page on click; suppressed with `rel="noreferrer"`.
* **PAT (Personal Access Token)**: token used instead of password for Git pushes.
