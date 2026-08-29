# Academic website — setup guide

A four-page static site: **About**, **Research**, **Teaching**, **Contact**.
Plain HTML and CSS, no build step, no software to install. You edit the files in
a text editor, push to GitHub, and the live site updates within a minute.

```
index.html          About page (the homepage)
research.html       Publications, working papers, work in progress
teaching.html       Courses and teaching materials
contact.html        Contact details and profile links
style.css           All the styling — you rarely need to touch this
.nojekyll           Tells GitHub not to pre-process the files. Leave it alone.
assets/img/         Photos. Contains a placeholder photo.jpg
assets/files/       PDFs you want stored in the repo (CV, etc.)
```

---

## 1. Getting it online (about 10 minutes, one time only)

**a. Create the repository.** On github.com, click **+ → New repository**.
Name it exactly `yourusername.github.io`, replacing `yourusername` with your GitHub
username. Set it to **Public**. Don't add a README (you already have one). Create it.

> The name matters. A repo named `username.github.io` is served at
> `https://username.github.io`. Any other name is served at
> `https://username.github.io/reponame/`, which is uglier but works fine too.

**b. Upload the files.** On the empty repo page, click
**uploading an existing file**, then drag in everything from this folder —
the four `.html` files, `style.css`, `README.md`, and the `assets` folder.

The `.nojekyll` file is hidden on Mac (files starting with a dot are). In Finder,
press `Cmd+Shift+.` to reveal hidden files, drag it in, and press it again to
re-hide them. If you skip it, the site still works.

Scroll down, write "first version" in the commit box, click **Commit changes**.

**c. Turn on Pages.** In the repo, go to **Settings → Pages**. Under *Source*
choose **Deploy from a branch**, set branch to **main** and folder to **/ (root)**,
and click **Save**. Wait a minute, reload, and the page shows your live URL.

**d. Later edits.** Two ways:

- *In the browser* — open a file on GitHub, click the pencil icon, edit, commit.
  Fine for a quick fix.
- *On your computer* — install [GitHub Desktop](https://desktop.github.com),
  clone the repo, edit files locally, and click "Commit" then "Push". Better for
  real work, because you can open the file in a browser to check it before publishing.

To preview a change locally, just double-click the `.html` file — it opens in your
browser and looks exactly as it will online.

---

## 2. Linking working papers from Dropbox

This is what makes the site low-maintenance: the PDF lives in Dropbox, the site
only points at it. Overwrite the PDF in Dropbox and visitors immediately see the
new draft — no GitHub push needed.

**Step by step:**

1. In Dropbox, right-click the PDF → **Copy link**. You get something like:

   ```
   https://www.dropbox.com/scl/fi/a1b2c3/jmp_draft.pdf?rlkey=x9y8z7&dl=0
   ```

2. Change the **`dl=0`** at the very end to **`raw=1`**:

   ```
   https://www.dropbox.com/scl/fi/a1b2c3/jmp_draft.pdf?rlkey=x9y8z7&raw=1
   ```

   `raw=1` opens the PDF in the browser. `dl=1` forces a download instead —
   use whichever you prefer. `dl=0` (the default) sends visitors to a Dropbox
   preview page with sign-up prompts, which is what you want to avoid.

   Keep the `rlkey=...` part. It's what authorises the link; without it the link breaks.

3. In the HTML, find the paper's link and paste the URL in place of
   `PASTE_DROPBOX_LINK_HERE`:

   ```html
   <div class="links">
     <a href="https://www.dropbox.com/scl/fi/a1b2c3/jmp_draft.pdf?rlkey=x9y8z7&raw=1">PDF</a>
     <a href="PASTE_DROPBOX_LINK_HERE">Slides</a>
   </div>
   ```

**Two things that will bite you eventually:**

- **Replace, don't delete-and-re-upload.** Saving over the file (same name, same
  folder) keeps the link alive. Deleting it and uploading a new copy creates a
  *new* file with a *new* link, and your website link dies silently. If you compile
  to `paper.pdf` in a Dropbox folder from LaTeX, you're already doing the right thing.
- **Check the sharing setting** is "Anyone with the link can view", not
  "Only people invited". Dropbox sometimes defaults to the latter inside shared
  team folders.

**Prefer to store some PDFs in the repo instead?** Drop them into `assets/files/`
and link them as `assets/files/paper.pdf`. Permanent and self-contained, but every
new draft means a GitHub commit. Many people do Dropbox for live drafts and the
repo for the CV and published versions.

---

## 3. Filling in the placeholders

Open any `.html` file in a text editor. Every spot needing your input is marked
one of two ways:

- `[PLACEHOLDER — ...]` — visible text on the page. Replace the whole bracket,
  brackets included.
- `PASTE_DROPBOX_LINK_HERE` / `PASTE_LINK_HERE` — a URL. Replace it, keeping the
  surrounding quotes.

Lines wrapped in `<!-- ... -->` are comments explaining what to do. They don't
appear on the page. Delete them once you no longer need them, or leave them —
they cost nothing.

Each page opens with a grey instruction box (`<div class="note"> ... </div>`).
**Delete those four blocks before you publish.**

**Adding a paper.** Copy an entire block from `<li class="paper">` to `</li>`,
paste it below, and change the contents. Same idea on the teaching page with
`<div class="course"> ... </div>`. Deleting an entry means deleting the whole
block, opening tag to closing tag.

**Adding or removing a link button.** Each is one line:

```html
<a href="PASTE_DROPBOX_LINK_HERE">Slides</a>
```

Delete the line to remove the button; copy it to add one. The visible label
("Slides", "PDF", "Replication") is just the text between the tags — change it freely.

**Your photo.** Save a square headshot as `assets/img/photo.jpg`, overwriting
the placeholder. Around 600×600 pixels is plenty; anything larger just slows the
page down. If your photo is a `.png`, don't just rename the file — instead save it
as `assets/img/photo.png` and change `src="assets/img/photo.jpg"` in `index.html`
to match.

**Your name and affiliation** appear in the header of all four files, plus the
`<title>` tag and the footer. When you change them, change them in all four —
a search for your name in each file finds every spot.

---

## 4. Changing the look

Everything visual lives in `style.css`. The top block is where the useful knobs are:

```css
--accent:  #6b2020;   /* links and the underline under the current tab */
--ink:     #1a1a1a;   /* main text colour */
--measure: 46rem;     /* how wide paragraphs run before wrapping */
```

Change `--accent` to your department's colour if you like — UZH blue is `#0028a5`.
Pick colours with a picker such as [coolors.co](https://coolors.co) and paste the
hex code in. To make text bigger overall, change `font-size: 18px` in the `body`
rule.

The site already adapts to phones and prints cleanly, so avoid changing the
`@media` blocks at the bottom unless you know what they do.

---

## 5. Optional next steps

- **Custom domain** (e.g. `matteofilippi.com`). Buy the domain, then in
  **Settings → Pages → Custom domain** enter it, and at your registrar add the
  DNS records GitHub shows you. Tick "Enforce HTTPS" once it validates.
- **Google Scholar indexing.** Nothing to do — Scholar finds linked PDFs on its
  own, usually within a few weeks.
- **Analytics.** If you want to know who visits, a privacy-friendly option that
  doesn't require a cookie banner is [GoatCounter](https://www.goatcounter.com);
  it gives you one `<script>` line to paste before `</body>` on each page.

---

## Quick troubleshooting

| Symptom | Cause |
|---|---|
| Site shows a 404 after setup | Pages needs a minute or two; also check the file is named `index.html`, lowercase |
| Page appears unstyled, plain black text | `style.css` isn't next to the HTML files, or its name got changed |
| Photo shows as a broken icon | Filename or folder doesn't match the `src` — check capitalisation, GitHub is case-sensitive |
| Dropbox link opens a sign-up wall | You left `dl=0`, or the file isn't shared with "anyone with the link" |
| Dropbox link 404s | The file was deleted and re-uploaded rather than overwritten — get a fresh link |
| Edit doesn't appear on the live site | Commit landed but Pages is still building; wait a minute, then hard-refresh (`Cmd+Shift+R`) |
