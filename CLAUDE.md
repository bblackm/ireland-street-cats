# CLAUDE.md — Community Cat Website Project

> Project instructions for Claude. This file guides every future session on this
> project. It lives at the repo root (so it's auto-loaded as context) and can also
> be pasted into a Claude Project's custom-instructions field.

---

## 1. What this project is

Brian cares for a colony of community (free-roaming) cats in his Burlington, NC
neighborhood. He live-traps them and takes them to **Burlington Animal Services**
for vaccinations and spay/neuter, then returns them to the colony where he provides
ongoing food and care. This is classic **TNR — Trap, Neuter, Return**.

This project builds:

1. A public **website** documenting the colony and each cat.
2. Physical **QR-code tags/signs** that link to a cat's page (and to the site).
3. **Names** for the community and the website (see §9).

Audience: neighbors, potential sponsors/adopters, and anyone who scans a tag and
wants to know "whose cat is this?" Most visitors arrive **on a phone via a QR code**,
so mobile comes first.

---

## 2. Goals (from Brian)

1. Informative **homepage**.
2. A **page per cat**: name, photo, bio/description, vaccination list + dates,
   spay/neuter status.
3. **Easy to add or remove a cat.**
4. QR-code **tags** linking to a cat's page/info.
5. Extra pages Brian chose: **How to Help + Donations**, **Sponsor / Adopt a Cat**,
   **About TNR & Colony Story**, **Colony Map & Contact**.

---

## 3. Tech stack & hosting

- **Jekyll** static site, hosted free on **GitHub Pages**. GitHub Pages builds
  Jekyll automatically on every push — no build server, CI, or database needed.
- Each cat is a Markdown file in a Jekyll **collection** (`_cats/`). This is the key
  decision that makes goal #3 easy: **adding a cat = add one file; removing = delete
  the file.** Each cat gets a clean, stable URL (`/cats/<slug>/`) so QR codes and
  shared links never break.
- No backend, no login, no tracking. Everything is static files in one Git repo.
- **Mobile-first** and accessible.

### Repo structure

```
/
├─ CLAUDE.md                ← this file
├─ _config.yml              ← site title, url, collections, defaults
├─ index.md                 ← homepage
├─ _cats/                   ← ONE FILE PER CAT — this is the "database"
│  ├─ luna.md
│  └─ shadow.md
├─ _layouts/
│  ├─ default.html
│  └─ cat.html              ← renders a single cat page
├─ _includes/               ← header, footer, cat-card, badges, etc.
├─ pages/
│  ├─ help.md               ← How to Help + Donations
│  ├─ sponsor.md            ← Sponsor / Adopt
│  ├─ about-tnr.md          ← About TNR & colony story
│  └─ contact.md            ← Colony map & contact
├─ assets/
│  ├─ css/
│  ├─ images/cats/          ← cat photos
│  └─ qr/                   ← generated QR PNGs, one per cat + colony
└─ .gitignore
```

> Confirm current GitHub Pages + Jekyll setup specifics (Ruby/gem versions, whether
> to build via the classic Pages build or a GitHub Actions workflow) at scaffold time.

---

## 4. Cat data model (front matter schema)

Every file in `_cats/` begins with YAML front matter, followed by the bio in
Markdown. Keep field names consistent — the `cat.html` layout reads them.

```yaml
---
name: Luna
slug: luna                       # URL + filename; lowercase, hyphenated
photo: /assets/images/cats/luna.jpg
sex: female                      # female | male | unknown
color: "Gray tabby, white chest"
status: resident                 # resident | adoptable | adopted | missing | passed
fixed: true                      # spayed / neutered?
fixed_date: 2025-11-02
ear_tipped: true                 # universal visual sign a community cat is fixed
microchip: ""                    # optional
intake_date: 2025-10-20          # first trapped / tagged
vaccinations:
  - name: Rabies
    date: 2025-11-02
    due: 2026-11-02              # optional next-due date
  - name: FVRCP
    date: 2025-11-02
sponsored: false
sponsor_note: ""                 # optional
temperament: "Shy but warms up at feeding time"
zone: "Colony A"                 # GENERAL area label only — never a precise location
---

Luna first showed up in the fall of 2025... (bio in Markdown here)
```

Notes:
- `status` drives badges and filtering (e.g., show "Adoptable" cats on the sponsor page).
- `vaccinations` is a list so the cat page can render a dated table.
- Keep an existing file as the canonical template to copy.

---

## 5. Adding / removing a cat (the easy workflow)

**Easiest — with Claude:** Brian says "add a cat named X" (and drops a photo / gives
details). Claude creates `_cats/<slug>.md`, saves the photo to
`assets/images/cats/`, generates the QR code into `assets/qr/`, and commits. To
remove: "remove <name>" → Claude deletes the file (plus its photo and QR) and pushes.

**Manual (so Brian can do it solo):**
1. Copy any file in `_cats/`, rename it `<slug>.md`.
2. Fill in the front matter; write the bio below the `---`.
3. Add the photo to `assets/images/cats/`.
4. Commit & push — GitHub Pages rebuilds in ~1 minute.
5. To remove, delete the file (and its photo/QR) and push.

---

## 6. QR codes & physical tags

- **Per-cat QR:** encodes that cat's page URL (`https://<site>/cats/<slug>/`). Save
  as `assets/qr/<slug>.png` **and** display it on the cat's own page so it's trivial
  to reprint.
- **Colony QR:** one QR to the homepage for general signage.
- Generate QR PNGs **offline** (e.g. Python `qrcode` library) — no third-party
  service, no tracking, no expiring links.

**Physical format — important animal-welfare nuance:**
- **Ear-tipping** is the standard universal sign a community cat is already fixed —
  keep ear-tipping every cat; it prevents re-trapping.
- **Collars are risky for feral cats** (snag hazard) unless breakaway *and* the cat
  is socialized. So:
  - **Friendly/socialized cats** → breakaway collar with a small tag: *"Community
    cat — cared for. Scan to meet me."* + QR + a contact number.
  - **Whole colony** → laminated cards or a small yard / feeding-station sign with
    the colony QR: *"These cats are ear-tipped, vaccinated & neutered and cared for
    by a neighbor. Please don't remove or relocate them."*
- Consider a "please don't feed elsewhere / don't remove" line to reduce well-meaning
  interference.

---

## 7. Design direction — warm & cozy

- Soft, homey palette (warm creams, terracotta/rust, a muted sage or dusty-blue
  accent). Rounded corners, generous spacing, a friendly humanist sans or soft serif
  for headings.
- **Homepage:** short mission statement, a warm hero photo, a grid of cat **cards**
  (photo, name, one-line temperament, "fixed / vaccinated / ear-tipped" badges), and
  clear buttons to *Help / Donate* and *About TNR*.
- **Cat page:** large photo, name, status badges (ear-tipped ✓, neutered ✓,
  vaccinated ✓), bio, a **vaccination table with dates**, and the cat's QR code.
- Accessible: strong contrast, **alt text on every photo**, keyboard-friendly, fast.
  Mobile-first layout.

---

## 8. Privacy & safety (please follow)

Community-cat locations are sensitive. Publicizing them invites people to dump more
cats, harass the colony, or harm the animals.

- **Do NOT publish Brian's home address (806 Harris St) or precise feeding/colony
  locations** anywhere on the site or in image metadata.
- "Colony map" means a **general, neighborhood-level** area — never pin-precise
  feeding stations. Use zone labels ("Colony A") instead of coordinates.
- Contact via a **form or a dedicated project email**, not a home address; only show
  a phone number if Brian explicitly wants to.
- Watch for photos that reveal exact locations (house numbers, distinctive yards).

---

## 9. Naming — pick your favorites (open decision)

**Community name candidates**
- *Place-based:* The Harris Street Cats · Harris Street Colony · Harris Street Feline Friends
- *Warm / community:* Harris Street Cat Collective · Neighborhood Cats of Harris Street
- *Playful:* The Purrlington Colony (Purr + Burlington) · Harris Street Alley Cats · The Harris Street Prowl

**Website / domain candidates**
- `harrisstreetcats.org` — clear and clean *(strong default)*
- `purrlington.org` — memorable, playful
- `harrisstreetcolony.org`
- Free to start: `<github-username>.github.io/harris-street-cats`

**Suggested starting point:** community = **"The Harris Street Cats,"** site =
**harrisstreetcats.org** (or the free `github.io` URL until a domain is purchased).
Everything is easy to rename later.

---

## 10. Roadmap

1. **Scaffold** — Jekyll skeleton, `_config.yml`, layouts, warm-cozy CSS, homepage,
   one sample cat, deployed to GitHub Pages.
2. **Content** — real cats + photos; the Help / Sponsor / About-TNR / Contact pages.
3. **QR & tags** — generate per-cat and colony QR codes; design printable tag/sign cards.
4. **Polish** — optional custom domain, donation link, sponsor/adopt flow, accessibility pass.

---

## 11. Open decisions (need Brian's input)

- Final **community name** + **site name / domain**.
- **Contact method** (form vs. email) and whether to show a phone number.
- **Donation platform** (PayPal, Ko-fi, GoFundMe, Venmo, or link directly to
  Burlington Animal Services / a rescue).
- Which cats (if any) are **adoptable** vs. return-only.
- **Custom domain now**, or start on the free `github.io` URL.
