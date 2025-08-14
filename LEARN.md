Web Development & Git/GitHub Learning Notes

A comprehensive reference from building my first HTML/CSS/JS project, hosting it on GitHub Pages, and managing it via CLI.

⸻

1. HTML — Structure of the Webpage

What:
HTML (HyperText Markup Language) defines the structure/content of a webpage.

Why:
It’s the foundation of every web project; browsers read HTML to render text, images, and links.

Core Concepts Learned:
	•	<!DOCTYPE html> tells the browser it’s HTML5.
	•	<html lang="en"> wraps all content.
	•	<head> contains metadata, links to fonts, CSS.
	•	<body> holds the visible content.
	•	<p> for paragraphs, <a> for links, <em> for emphasis.
	•	target="_blank" makes links open in new tabs.
	•	Semantic tags matter for accessibility & SEO.

Example:

<p>I was part of the early team at <a href="https://example.com" target="_blank">Sprinto</a>.</p>


⸻

2. CSS — Styling the Webpage

What:
CSS (Cascading Style Sheets) controls design — colors, fonts, spacing, layout.

Why:
It separates style from content, making design changes easy without touching HTML.

Core Concepts Learned:
	•	body { padding: 0 2rem; } → sets horizontal padding only.
	•	body { padding: 2rem; } → sets padding on all sides.
	•	Variables: --link-color for dynamic styling.
	•	Hover effects with a:hover or a:hover::after for animations.
	•	Media queries for responsiveness:

@media (max-width: 600px) {
  body {
    padding: 0 1rem;
  }
}

	•	Random color changes via JS: document.documentElement.style.setProperty('--link-color', randomColor);

Example Hover Animation:

a {
  position: relative;
}
a::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: -1px;
  height: 1px;
  width: 100%;
  background-color: var(--link-color);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.3s ease;
}
a:hover::after {
  transform: scaleX(1);
}


⸻

3. JavaScript — Adding Interactivity

What:
JavaScript controls dynamic behavior (e.g., changing colors on refresh).

Why:
It lets you make static pages interactive without reloading from the server.

Core Concepts Learned:
	•	Arrays for color options.
	•	Math.floor(Math.random() * colors.length) to pick a random element.
	•	Modifying CSS variables from JS.

Example:

const colors = ["#e63946", "#457b9d", "#6c5ce7"];
const randomColor = colors[Math.floor(Math.random() * colors.length)];
document.documentElement.style.setProperty('--link-color', randomColor);


⸻

4. Command Line Interface (CLI)

What:
A text-based way to control your computer and Git.

Why:
Essential for working with Git, running local servers, and file navigation.

Core Commands Learned:

pwd       # show current directory
ls        # list files
cd folder # change directory
git status

Mistake to Avoid:
	•	If you get fatal: not a git repository, run git init or navigate to the repo folder.

⸻

5. Git & GitHub — Version Control & Hosting

What:
Git tracks file changes. GitHub stores your code online. GitHub Pages hosts static sites for free.

Why:
	•	Roll back mistakes.
	•	Collaborate.
	•	Publish websites instantly.

Core Workflow Learned:

git add filename        # stage changes
git commit -m "message" # commit
git push origin main    # push to GitHub

Common Fixes:
	•	Wrong folder? → Check pwd.
	•	File missing from commit? → Save file, then git add again.

GitHub Pages Setup:
	1.	Push code to GitHub.
	2.	In repo → Settings → Pages → set source to main branch.
	3.	URL format: https://username.github.io/reponame.

⸻

6. Privacy & Linking

Learned:
	•	Linking to a site (<a href>) doesn’t notify them unless they check referral logs.
	•	Removing the link quickly means it’s unlikely they’ll see it.
	•	To avoid sending referral info, add rel="noreferrer".

Example:

<a href="https://example.com" target="_blank" rel="noreferrer">Example</a>


⸻

7. Debugging & Fixes
	•	Local server stopped? Likely pressed Ctrl+C to exit.
	•	File not found? Confirm with ls.
	•	CSS not showing? Check that <link rel="stylesheet"> path is correct.
	•	JS not running? Ensure <script> is before </body>.

⸻

8. Terms & Concepts
	•	Media query → CSS that applies only on certain screen sizes.
	•	Pseudo-element → Fake element used for effects (::after).
	•	transform: scaleX() → Expands/shrinks element horizontally.
	•	transform-origin → Where the scaling starts from.
	•	Deterministic vs. probabilistic → Certain outcome vs. outcome with probability.
	•	Management theater → Activities that look like leadership but add no value.

⸻

9. Next Steps
	•	Practice adding new sections to HTML and styling them.
	•	Experiment with CSS animations.
	•	Try fetching data with JavaScript.
	•	Learn branching in Git.
	•	Use Markdown (.md) for documentation.

⸻

Tip: Keep updating this file (NOTES.md) as you learn — it becomes your personal dev wiki.

⸻

First‑Principles Deep Dive: What • Why • How

A conceptual map of everything we touched — explained from the ground up so you can reason, not memorize.

1) HTML: Why structure matters

What it is: A tree (DOM) of elements that give meaning to content.

Why it exists: Browsers, assistive tech, crawlers, and CSS/JS all need a predictable structure to interpret. Semantics → accessibility, SEO, and consistent rendering.

Mental model: Think of HTML as the data model of your page. CSS = how it looks; JS = how it behaves.

Core concepts
	•	Semantics > styling: <em> means emphasis (screen readers convey it). <i> is just italics (visual only). Use meaning wherever possible — it future‑proofs your content and improves accessibility/search.
	•	Single responsibility: Keep content (HTML), presentation (CSS), behavior (JS) separate so each can evolve independently.
	•	Links: <a href> creates navigation across the graph of the web. It’s the most important element on the internet.
	•	Head vs body:
	•	<head>: metadata, fonts, icons, social preview, SEO instructions. It’s read before paint.
	•	<body>: everything users see and interact with.

How to act on this
	•	Prefer meaningful tags: <p>, <ul>/<ol>/<li>, <section>, <header>, <footer>, <main>, <nav>.
	•	Use target="_blank" rel="noopener noreferrer" for external links (security + no referrer if desired).
	•	Keep markup minimal; remove decorative wrappers and use pseudo‑elements in CSS instead.

⸻

2) CSS: Cascade, inheritance, specificity — why styles sometimes “don’t work”

What it is: A constraint system that resolves conflicts by rules.

Why it exists: To let many styles apply at once and still end up with a deterministic result.

Mental model: CSS calculates a final style for each DOM node by walking rules in order of specificity, source order, and inheritance.

The three pillars
	1.	Cascade / source order: Last matching rule wins when specificity ties.
	2.	Specificity: inline > id > class/pseudo‑class/attribute > element. When things don’t apply, check specificity first.
	3.	Inheritance: some properties (color, font) inherit; others (margin, padding) don’t. If a value seems “sticky,” it may be inherited.

Box model (why spacing feels tricky)
	•	Content → Padding → Border → Margin. Layout issues usually come from confusing padding vs margin.
	•	box-sizing: border-box; makes width/height include padding/border → saner layouts.

Units & typography — why rem:
	•	rem scales with the root font size → accessibility (zoom) and consistent rhythm.
	•	Default 1rem = 16px (unless root changed).

Responsive intent — why media queries:
	•	Different viewports need different density and touch targets. Media queries let you express design constraints, not device names.

How to act on this
	•	Start with a typographic container: max-width: 65ch; line-height: 1.7–1.8;.
	•	Set global sizing: * { box-sizing: border-box; }.
	•	Use variables for theme: :root { --link-color: … } and consume with var(--link-color).
	•	Mobile adjustments with @media (max-width: 600px) { … }.

⸻

3) CSS effects you used — and the physics behind them

Underline animation
	•	Why ::after? Decorative content shouldn’t be in HTML; pseudo‑elements let you draw without extra tags.
	•	Why transform? GPU‑friendly and doesn’t trigger layout, so it’s smooth.
	•	Why transform-origin: left? Defines the hinge point so the line grows left→right.

a { position: relative; text-decoration: none; }
a::after {
  content: ""; position: absolute; left: 0; bottom: -1px;
  height: 1px; width: 100%; background: var(--link-color);
  transform: scaleX(0); transform-origin: left; transition: transform .3s ease;
}
a:hover::after { transform: scaleX(1); }

Left accent line
	•	Why a border, not a div? Borders are cheaper than extra DOM; they participate in layout naturally.
	•	Why hide on mobile? Space is scarce; reduce non‑essential ornamentation.

body { border-left: 3px solid var(--link-color); }
@media (max-width: 600px) { body { border-left: none; } }


⸻

4) JavaScript: state, DOM, and persistence

What it is: A single‑threaded language running in the browser event loop.

Why it exists here: To bind state (chosen color, interactions) to the DOM without reloading.

Mental model:
	•	The DOM is your view; JS is the controller updating CSS variables / attributes.
	•	Prefer small, declarative changes (set a CSS var) over DOM rewrites (innerHTML).

Random accent color — why this design
	•	CSS variable = single source of truth for theme.
	•	JS picks a color at load; everything that uses var(--link-color) updates in one go.

const colors = ["#e63946","#ff5733","#ff006e","#6a00f4","#3a0ca3","#0f4c81","#007f5f","#118ab2","#f72585","#ef233c"];
const c = colors[Math.floor(Math.random()*colors.length)];
document.documentElement.style.setProperty('--link-color', c);

Persisting choices — why localStorage
	•	Keeps the same accent across page reloads/sessions without a backend.

const KEY='accent';
let c2 = localStorage.getItem(KEY) || colors[Math.floor(Math.random()*colors.length)];
localStorage.setItem(KEY, c2);
document.documentElement.style.setProperty('--link-color', c2);

When to run JS
	•	Put <script> before </body> or use defer so it runs after DOM is parseable.

⸻

5) Local preview & servers: why an HTTP server at all?

What “open file” does: Browser loads file:///… paths directly — no headers, no routing, some APIs restricted.

Why python3 -m http.server:
	•	Serves via HTTP (http://localhost:8000) with correct MIME types (e.g., text/html, text/css).
	•	Mirrors how a site behaves when deployed.
	•	Easier debugging for relative paths and CORS‑sensitive features.

Signals & processes — why Ctrl+C works:
	•	The server is a foreground process. Ctrl+C sends SIGINT → process exits → terminal returns to prompt.

How to use

python3 -m http.server        # defaults to port 8000 and current folder
# visit http://localhost:8000
# stop with Ctrl + C


⸻

6) Git: the model, not just the commands

What it is: A content‑addressed Merkle DAG of snapshots (commits). The staging area (index) selects what goes into the next snapshot.

Why it matters: Understanding the model makes “weird” errors obvious.

Mental model
	•	Working tree = your files now.
	•	Index = what will be in the next commit (git add).
	•	HEAD = your current commit.
	•	Remote (origin/main) = another branch pointer hosted on GitHub.

Happy path

git add .
git commit -m "Describe change"
git push origin main

When GitHub has newer commits (e.g., you edited in the browser):
	•	Why push fails: Your local main is behind remote main; Git prevents accidental overwrite.
	•	Fix: Replay your commits on top of the remote history.

git pull origin main --rebase
# resolve conflicts if asked
git push origin main

Auth — why tokens, not passwords
	•	GitHub removed password auth over HTTPS for security. Use a Personal Access Token (PAT) and cache it in Keychain (macOS):

git config --global credential.helper osxkeychain

Common diagnostics
	•	fatal: not a git repository → you’re not inside a folder that has a .git directory → git init in the right folder.
	•	remote origin already exists → you already linked a remote; update it:

git remote set-url origin https://github.com/USER/REPO.git


⸻

7) GitHub Pages: what actually happens when you “publish”

What it is: A static hosting pipeline tied to your repo/branch.

Why it’s simple: No servers to manage; GitHub serves files from your branch as‑is.

Flow
	1.	You push to main.
	2.	Pages checks the configured source (Branch: main, Folder: / (root)).
	3.	Files become available under https://USERNAME.github.io/REPO/.

Why your site might not appear immediately
	•	Propagation takes ~30–60s.
	•	Browser cache can serve old content; hard refresh.
	•	Wrong source configured (e.g., gh‑pages branch vs main).

Taking it offline
	•	Settings → Pages → Source: None (unpublish), or remove index.html (returns 404), or add noindex meta (discourages search, doesn’t block direct visits).

⸻

8) Linking, referrers, and indexing — how the web “notices” things

Referrer header — why others might see you
	•	When a user clicks a link on your page, the destination may log the Referer (sic) header → reveals your page URL.
	•	Suppress it with rel="noreferrer".

Backlinks & crawlers — why removal after 4 minutes likely went unseen
	•	Crawlers discover links by visiting pages they already know. Brand‑new Pages sites aren’t visited immediately.
	•	If no one clicked and no crawler came by, it’s effectively invisible.

Robots

<meta name="robots" content="noindex, nofollow">

	•	Why: Ask honest crawlers to skip indexing and not follow links. Not a security feature; just etiquette.

⸻

9) Accessibility & quality: small choices, big outcomes

Why semantics/contrast/focus matter: Real users navigate with keyboards, screen readers, zoom. Good defaults reduce friction.

How

a:focus { outline: 2px solid var(--link-color); outline-offset: 2px; }
body { color: #222; background: #fff; }

	•	Ensure link color/underline communicates state; don’t rely on color alone.
	•	Prefer em/strong for meaning, not just appearance.

⸻

10) Performance & fonts — avoiding flashes and jank

Why fonts flicker: Custom fonts load after HTML. Browser shows fallback (FOUT) or hides text (FOIT) depending on strategy.

How to be kind to users
	•	Load only the weights you use.
	•	Consider font-display: swap (Google Fonts via &display=swap) so text shows immediately with a fallback, then swaps.
	•	Keep animations transform/opacity‑only to avoid layout thrash.

⸻

11) Debugging method: a checklist that always works
	1.	Reproduce: hard refresh, incognito.
	2.	Inspect: Elements → check computed styles; is your rule overridden (specificity)?
	3.	Log: console.log() values, query selectors, event firing.
	4.	Network: is the file loading? (200 vs 404; correct MIME type).
	5.	Isolate: comment sections until it works; binary search the culprit.

⸻

12) Copy‑paste snippets (curated)

Theme variable + randomizer

<script>
  const colors = ["#e63946","#ff5733","#ff006e","#6a00f4","#3a0ca3","#0f4c81","#007f5f","#118ab2","#f72585","#ef233c"];
  const c = colors[Math.floor(Math.random()*colors.length)];
  document.documentElement.style.setProperty('--link-color', c);
</script>

No index / no referrer external link

<meta name="robots" content="noindex, nofollow">
<a href="https://example.com" target="_blank" rel="noopener noreferrer">Example</a>

Responsive container

:root{ --space: clamp(16px, 3vw, 32px); }
body{ max-width:65ch; margin:5rem auto; padding:0 var(--space); }


⸻

13) How to grow from here (principled roadmap)
	•	Progressive enhancement: Start with solid HTML → layer CSS → add JS that enhances, not replaces.
	•	Design tokens: Move repeated colors/sizes into CSS variables; theme by toggling variables.
	•	Versioning discipline: Small commits with useful messages → easier diffs, easier rollbacks.
	•	Add a11y checks: Keyboard‑test your page; ensure visible focus and sufficient contrast.
	•	Ship & learn: Treat production like a lab — small, reversible changes with a hypothesis attached.

