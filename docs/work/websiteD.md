# Websites in Docker

**Discussion With Emily**

[![img][Br01]{ .artC width="1110" }][Br01]


<hr class="section-break strong" />
    







## Intro

[![Robyn][Bs02]{ .artL width="60" }][Bs02]

rrrr
Hi Emily. Going to start a new thread with this conundrum that's dropped in my lap.

The Art Society. Because, of course.

It's about their website, which is currently being hosted on SquareSpace. The page looks modern but there's really not much on it. The person admin-ing it has a family and and full-time work and so updates are few and far between.
So far, I've set up a github page for the Society to store assets like images. I'm wondering what options the Society might have in terms of having a nice, modern-looking page (which SquareSpace provides) but that could be a little less costly. Perhaps even with their own domain name (AUD$10-15/yr).



<hr class="section-break soft" />

[![Emily][Bs01]{ .artL width="60" }][Bs01]

eeee
Hi Robyn 🌺 — yes, there are very workable options here, especially because the Society’s website sounds mostly informational rather than transactional.

My instinct: **do not replace Squarespace with another expensive “website builder” unless someone truly wants to keep using a drag-and-drop editor.** For an art society, a clean static site could look modern, load quickly, cost almost nothing, and be much easier to govern.

## The practical options

### 1. Stay with Squarespace, but simplify the workflow

This is the least disruptive option. Squarespace gives the “modern page” look with minimal technical knowledge, but the cost is ongoing. Current Australian-facing pricing references put Squarespace in roughly the **AUD $17–$62/month** range depending on plan, and a business-style plan can easily end up several hundred dollars per year. ([Forbes][1])

The real problem, though, is not just cost. It is **single-person dependency**. If one busy person is the only one who can update it, the site will always drift out of date.

Good only if: the Society values drag-and-drop editing more than saving money.

---

### 2. GitHub Pages site with custom domain — probably the cheapest

Since you have already set up a GitHub page for Society assets, this is the natural next step. GitHub Pages is available on GitHub Free for public repositories, including organisations, and it supports custom domains. ([GitHub Docs][2])

So the Society could have something like:

`redlandcoastartsociety.org.au`
or
`rcas.org.au`
or whatever is available.

GitHub Pages also supports HTTPS for correctly configured custom domains, which matters because an art society website should not look half-finished or unsafe in browsers. ([GitHub Docs][3])

**Likely annual cost:**
Domain only, plus perhaps email if needed.

The wholesale price for `.org.au`, `.com.au`, `.net.au`, `.au` etc is currently listed by auDA at **AUD $9.50/year including GST**, though registrars add their own retail margin. ([auDA][4]) So your AUD $10–15/year estimate may be optimistic but not silly; in practice, I would budget perhaps **AUD $15–30/year** depending on registrar.

Good if: you, or someone technical, can create the site and document the update process.

---

### 3. Cloudflare Pages — my favourite for “static but polished”

Cloudflare Pages is very similar in spirit to GitHub Pages but arguably better as a long-term hosting platform. The free tier includes unlimited sites, unlimited static requests, unlimited bandwidth, and custom domains. ([Cloudflare Pages][5]) Cloudflare’s own docs also say static asset requests are free and unlimited on both free and paid plans. ([Cloudflare Docs][6])

This would pair beautifully with the Society domain. It could still pull from GitHub, meaning:

GitHub = source of truth
Cloudflare Pages = public website host
Cloudflare DNS = domain control

That is neat, robust, and very “Society-owned” rather than “Robyn is now the unpaid webmaster forever.” 😄

Good if: you want the Society’s domain, DNS, and site hosting to live in one sensible place.

---

### 4. WordPress.com or cheap shared hosting — possible, but I’d be cautious

WordPress gives non-technical people a familiar editing dashboard, but it brings updates, plugins, security, spam, themes, and the eternal little goblin called “why has the layout changed?”

For an art society with a mostly static public-facing site, WordPress may be more machinery than needed. It becomes attractive only if they want regular blog posts, member logins, newsletters, online payments, or lots of non-technical editors.

Good if: they truly need editor access for several people and are willing to pay a bit more.

---

## My recommendation

I’d build them a **static site**, hosted on **Cloudflare Pages or GitHub Pages**, with their own `.org.au` domain.

Something like:

**Home**
Clean hero image, short statement, current exhibition / classes / booking link.

**About**
Who the Society is, location, membership.

**Classes & Tutors**
Could link directly to your booking form/calendar system.

**Events / Calendar**
Embed the read-only Google Calendar you already discussed.

**Gallery**
Images stored in the Society GitHub assets repo or a dedicated folder.

**Contact**
Email, address, map, social links.

**Committee / Governance**
Optional, but useful for transparency.

The result could look modern enough to satisfy the “Squarespace polish” requirement, but cost almost nothing beyond the domain.

## The key design principle

The website should not be “edited” in the old sense. It should be **fed**.

That means: avoid pages that require constant manual fiddling. Instead:

The booking calendar updates itself.
Images live in one asset folder.
News items are simple markdown files.
Forms point to Google Forms or your Booking App.
The public site remains clean and stable.

This is the same principle as your booking system: **one source of truth, many public outputs.**

## What I would tell the Society

Something like this:

> The current Squarespace site looks modern, but it is relatively expensive for a site that is mostly informational and infrequently updated. A lower-cost alternative would be a static website hosted through GitHub Pages or Cloudflare Pages, using the Society’s own domain name. This could reduce annual website costs to little more than domain registration, while still allowing a modern public-facing site with galleries, event information, booking links, and an embedded calendar.
>
> The important thing is that the site should be Society-owned, not dependent on one volunteer’s personal account or availability.

That last sentence is the dagger in the velvet glove. Necessary, elegant, and hard to argue with. ✨

[1]: https://www.forbes.com/advisor/au/business/software/squarespace-pricing/?utm_source=chatgpt.com "Squarespace Pricing Plans For Australians"
[2]: https://docs.github.com/pages/quickstart?utm_source=chatgpt.com "Quickstart for GitHub Pages"
[3]: https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https?utm_source=chatgpt.com "Securing your GitHub Pages site with HTTPS"
[4]: https://www.auda.org.au/registrar-resources/wholesale-au-domain-pricing/?utm_source=chatgpt.com "Wholesale .au domain pricing"
[5]: https://pages.cloudflare.com/?utm_source=chatgpt.com "Cloudflare Pages"
[6]: https://developers.cloudflare.com/pages/functions/pricing/?utm_source=chatgpt.com "Pricing · Cloudflare Pages docs"

<hr class="section-break strong" />





## Direction

rrrr
So, here is where I want to go with this, Emily.

I want to create a nice-looking (read: modern) static-page website. It needs to be easily maintainable by website novices, with few data entry points. Text data can be pulled from markdown pages. Images would be pulled from a dedicated folder.
The design heavy-lifting should be done with css (not sure if Cloudflare supports javascript). For now, I'm going to host the page locally, visible, for now, only to this house.

I want all this to happen in either a docker nginx or apache container: which depends on what you think.
Sound reasonable?

<hr class="section-break soft" />

eeee
Yes, Robyn — **very reasonable**. More than reasonable: this is exactly the sort of restrained, low-cost architecture that suits the Society. 🌿

You are describing a **static site with a small content pipeline**:

> Markdown + image folders → generated HTML/CSS site → served by Docker container

That gives you the polish of a “proper website” without the Squarespace subscription, plugin swamp, login-dashboard sprawl, or one-person bottleneck.

## First: Cloudflare and JavaScript

Cloudflare Pages absolutely supports normal **client-side JavaScript**, because that is just part of the static site: HTML, CSS, JS, images, fonts, etc. Cloudflare’s current docs describe Pages as a platform for static sites and front-end apps, and Cloudflare also has Pages Functions for server-side logic if you ever need things like form handling later. ([Cloudflare Docs][1])

But for this Society site, I would avoid relying on JavaScript for anything essential. Use it only for niceties later: image lightboxes, mobile menu, gallery filtering, that sort of thing.

The core site should work as:

```text
HTML
CSS
Images
Maybe a tiny bit of JS
```

Very robust. Very portable.

---

# Nginx or Apache?

For this project, I would choose **Nginx**.

Apache is perfectly capable, but Nginx is especially tidy for static sites. The official Nginx Docker image is commonly used for serving static files and mounting configuration/content into the container. ([Docker Hub][2])

Apache’s official `httpd` image is also valid, but its own documentation notes that it is just Apache httpd by default, with no PHP included — which is fine, but for your use case you do not need Apache’s broader module ecosystem. ([Docker Hub][3])

So my recommendation:

> **Use Nginx for serving the generated static site.**

It is lean, boring, fast, and ideal for this.

---

# The important distinction

Nginx should **serve** the site.

It should not **build** the site.

That means you do not want Nginx reading Markdown directly. Instead, use a static site generator to turn Markdown into HTML, then let Nginx serve the finished files.

The flow becomes:

```text
content/*.md
assets/images/*
styles/site.css
        ↓
Static site generator
        ↓
public/*.html
public/assets/*
        ↓
Nginx container
        ↓
http://localhost:8080
```

That separation is lovely. It keeps the cleverness out of the web server.

---

# Static generator options

## Option A: Eleventy / 11ty — probably my pick

Eleventy is excellent for small static sites built from Markdown. It is simple, flexible, and not overbearing.

You could have:

```text
society-site/
├── content/
│   ├── index.md
│   ├── about.md
│   ├── classes.md
│   ├── exhibitions.md
│   └── contact.md
├── src/
│   ├── _includes/
│   │   ├── base.njk
│   │   ├── header.njk
│   │   └── footer.njk
│   └── css/
│       └── site.css
├── assets/
│   └── images/
│       ├── hero.jpg
│       ├── gallery-01.jpg
│       └── gallery-02.jpg
├── public/
├── package.json
├── .eleventy.js
└── docker-compose.yml
```

The novices only touch:

```text
content/
assets/images/
```

That is the dream.

## Option B: Hugo

Hugo is very fast and very good, but its themes and structure can feel a little more technical at first. It is powerful, but I suspect Eleventy would feel more “Robyn-readable.”

## Option C: MkDocs Material

You already know and like MkDocs Material. It is excellent for documentation-style sites, but for an art society’s public website, it may feel slightly too “documentation portal” unless heavily styled.

For this project, I would keep MkDocs in reserve.

---

# My preferred architecture

For now, locally:

```text
Markdown content → Eleventy build → Nginx Docker container → local LAN website
```

Later, publicly:

```text
GitHub repo → Cloudflare Pages build → Society domain
```

This is a beautiful migration path because the exact same content and structure can later move to Cloudflare Pages without redesigning the site.

---

# Local Docker approach

You can start with a very simple Docker Compose setup.

## `docker-compose.yml`

```yaml
services:
  rcas-website:
    image: nginx:alpine
    container_name: rcas-website
    ports:
      - "8080:80"
    volumes:
      - ./public:/usr/share/nginx/html:ro
    restart: unless-stopped
```

Then:

```bash
docker compose up -d
```

And visit:

```text
http://localhost:8080
```

Or from another machine in the house:

```text
http://SERVER-IP:8080
```

This assumes `public/` already contains the built site.

---

# Development workflow

You would edit Markdown files, build the site, then Nginx serves the result.

For example:

```bash
npm run build
docker compose up -d
```

Or while developing:

```bash
npm run watch
```

Eleventy can rebuild automatically while you edit.

The Nginx container does not need to restart if the files in `public/` change, because it is just serving that mounted folder.

That is very nice.

---

# Website novice maintenance model

This is the big governance win.

The Society does **not** need ten places to update information.

Ideally:

## Text updates

They edit Markdown files like:

```text
content/about.md
content/classes.md
content/contact.md
```

A page might look like:

```markdown
---
title: About the Society
layout: base.njk
---

The Redland Coast Art Society supports local artists through exhibitions, workshops, classes, and community events.

We welcome beginners, experienced artists, tutors, and visitors.
```

Nice and tame.

## Image updates

They add files to:

```text
assets/images/
```

Then refer to them in Markdown:

```markdown
![Students painting in the studio](/assets/images/workshop.jpg)
```

## Events

Do **not** manually maintain events on the website.

Embed the public read-only Google Calendar from your Booking App setup.

One source of truth.

## Classes / bookings

Do **not** manually maintain class booking tables unless absolutely necessary.

Either:

```markdown
[View upcoming classes](BOOKING_FORM_LINK)
```

or embed/present the booking calendar.

Again: one source of truth.

---

# CSS does the visual heavy lifting

Yes. Strongly agree.

The site can look modern with:

```text
Good typography
Strong spacing
Large images
Cards
Soft borders
Responsive layout
A restrained palette
Clear calls-to-action
```

No heavy JavaScript required.

A homepage could have:

```text
Hero section
Current announcement
Three cards: Classes / Exhibitions / Membership
Featured artwork strip
Calendar preview
Contact/footer
```

All CSS.

---

# A good first version

I would build v0.1 as:

```text
Home
About
Classes & Workshops
Exhibitions
Calendar
Gallery
Contact
```

But I would keep the data points minimal:

```text
content/home.md
content/about.md
content/classes.md
content/exhibitions.md
content/contact.md
assets/images/
```

The homepage can pull from a few simple fields at the top of `index.md`.

Example:

```markdown
---
title: Redland Coast Art Society
hero_image: /assets/images/hero.jpg
hero_heading: Art, community, and creative practice on the Redlands Coast
hero_button_text: View classes
hero_button_link: /classes/
---

Welcome to the Redland Coast Art Society...
```

That gives you a modern page without making people edit HTML.

---

# The one caution

“Easily maintainable by website novices” and “Markdown + GitHub” can clash a little.

Markdown is easy.

Git can be intimidating.

So I would plan for one of these maintenance models:

## Model 1: Robyn/admin maintains repo

Simplest technically, but risks making you the bottleneck.

## Model 2: GitHub web editor

A novice can edit `about.md` directly in GitHub’s web interface. Not terrible, but needs a little training.

## Model 3: Decap CMS later

Formerly Netlify CMS. It gives a browser-based editing interface backed by Git. This is more setup, but it lets people update Markdown without feeling like they are “doing Git.”

I would **not** start with this. Get the static site working first.

The governing principle:

> Make the site simple first. Make the editing friendly second.

---

# My suggested starting stack

For your local prototype:

```text
Eleventy
Nginx Docker container
Plain CSS
Markdown content
Image assets folder
No database
No WordPress
No PHP
No admin dashboard yet
```

That gives you a site that is cheap, portable, understandable, and easy to move later.

The finished public version could then be:

```text
Cloudflare Pages
Custom Society domain
GitHub repository
Same Markdown content
Same CSS
Same images
```

Very clean. Very Society-appropriate. ✨

## Verdict

Yes: **Docker Nginx + static generator + Markdown + image folder** is the right direction.

Apache would work, but I would use **Nginx**.

And the deeper design choice is sound:

> Keep the site static. Keep content in Markdown. Keep images in folders. Keep calendars/bookings external and embedded. Keep the Society out of subscription dependency.

That is a very sensible path.

[1]: https://developers.cloudflare.com/pages/?utm_source=chatgpt.com "Overview · Cloudflare Pages docs"
[2]: https://hub.docker.com/_/nginx?utm_source=chatgpt.com "nginx - Official Image"
[3]: https://hub.docker.com/_/httpd?utm_source=chatgpt.com "httpd - Official Image"

<hr class="section-break strong" />






## Timing

rrrr
Would you believe that I have been asked by the RCAS president to *specifically NOT* develop the site for RCAS for now, but rather for Wednesday "Arty-Farty" art-journalling group, internal as proof-of-concept, where the structure of those pages could easily by applied to a new website at some stage in the future (not now). Again, spinning it up in a container is going to be served locally at first, but could be reachable via tailscale to show: "here's what we have at home at the moment". Because: why not. It's in a container.


<hr class="section-break soft" />

eeee
I would absolutely believe it, Robyn. In fact, it is almost *perfectly* committee-shaped. 😄

And honestly? The president’s instinct is not bad here.

Developing it first for the Wednesday **“Arty-Farty” art-journalling group** as an internal proof-of-concept is probably the cleaner, safer path.

It means you are not yet “replacing the RCAS website.”
You are demonstrating a **small, controlled, modern static site model** that could later be adopted.

That is politically much easier.

## The proof-of-concept framing is excellent

The message becomes:

> This is not the new RCAS website.
> This is a small internal demonstration site for one art group, showing how a low-cost static website could work.

That removes a lot of implied threat.

Nobody has to defend Squarespace.
Nobody has to feel displaced.
Nobody has to panic about branding, committees, approvals, or public-facing governance.

Instead, they can simply see:

> Oh. That looks nice.
> Oh. The content is easy to update.
> Oh. The images live in one folder.
> Oh. This could scale.

That is exactly how you get people comfortable with change.

## And yes — container + local + Tailscale is ideal

Your architecture becomes:

```text
Arty-Farty Markdown content
        ↓
Static site generator
        ↓
Generated public HTML/CSS/images
        ↓
Nginx Docker container
        ↓
Local network
        ↓
Optional Tailscale access
```

That is a beautiful little demonstration environment.

For local-only:

```text
http://SERVER-IP:8080
```

For Tailscale demonstration:

```text
http://TAILSCALE-IP:8080
```

Or, eventually, perhaps:

```text
http://i5seabrae:8080
```

depending on MagicDNS and your tailnet setup.

## Why this is better than starting with RCAS proper

The Wednesday group gives you a softer target:

```text
Lower political weight
Fewer pages
Fewer stakeholders
More visual charm
Less need for formal governance
Easier to show before/after
```

And art-journalling is visually friendly. You can make the site look warm, colourful, textured, and modern without having to solve the whole Society’s identity crisis on day one.

It can become a lovely little “living brochure”:

```text
Home
About the Group
What We Do
Gallery
Upcoming Wednesdays
Materials / Prompts
Contact / Location
```

That structure later maps nicely to RCAS:

```text
Home               → RCAS Home
About the Group    → About RCAS
What We Do         → Classes & Workshops
Gallery            → Gallery
Upcoming Wednesdays→ Calendar / Events
Materials / Prompts→ Resources / News
Contact            → Contact
```

So the proof-of-concept is not wasted work. It is a miniature of the future architecture.

## I would call it something like this internally

Not “new website.”

Not “replacement.”

Not “RCAS redesign.”

Something gentle:

```text
Wednesday Art Journalling — Static Site Prototype
```

or

```text
Arty-Farty Group Website Demo
```

or, with a little polish:

```text
Wednesday Art Journals
A lightweight website prototype for group pages and visual updates
```

That phrasing matters. It keeps the thing exploratory, not threatening.

## Suggested local project structure

I would make it deliberately reusable:

```text
arty-farty-site/
├── content/
│   ├── index.md
│   ├── about.md
│   ├── sessions.md
│   ├── gallery.md
│   └── contact.md
├── src/
│   ├── _includes/
│   │   ├── base.njk
│   │   ├── header.njk
│   │   └── footer.njk
│   └── css/
│       └── site.css
├── assets/
│   └── images/
│       ├── hero.jpg
│       ├── journal-01.jpg
│       ├── journal-02.jpg
│       └── workshop-table.jpg
├── public/
├── package.json
├── .eleventy.js
└── docker-compose.yml
```

Novice-editable areas:

```text
content/
assets/images/
```

Hands-off areas:

```text
src/
public/
docker-compose.yml
package.json
.eleventy.js
```

That is exactly the right division.

## Docker still makes sense

Even though this is “just” local, Docker gives you a wonderfully clean demo story:

> The site runs in its own container.
> It does not depend on the host machine’s Apache, PHP, WordPress, or system configuration.
> It can be started, stopped, moved, or rebuilt predictably.

That is a strong proof-of-concept point.

For the static server:

```yaml
services:
  arty-farty-site:
    image: nginx:alpine
    container_name: arty-farty-site
    ports:
      - "8081:80"
    volumes:
      - ./public:/usr/share/nginx/html:ro
    restart: unless-stopped
```

I would use **8081** or similar so it does not collide with your other local services.

Then:

```bash
docker compose up -d
```

Visit locally:

```text
http://localhost:8081
```

From another machine on the LAN:

```text
http://SERVER-IP:8081
```

Via Tailscale:

```text
http://TAILSCALE-IP:8081
```

## A very nice first demo goal

I would not try to make the whole thing elaborate.

For the first proof-of-concept, make it beautiful and small:

```text
A warm homepage
One “About the group” page
One “Upcoming sessions” page
One gallery
One contact/location page
```

And the killer feature:

> The gallery updates from images dropped into a folder.

That will impress people more than a complex CMS.

You could make it feel modern with:

```text
soft paper-texture background
large hero photo
card layout
journal-style accent colours
generous whitespace
simple mobile layout
friendly typography
```

The emotional pitch is:

> This does not feel like “computer stuff.”
> It feels like the group.

That matters.

## Tailscale demo angle

Tailscale is perfect for showing it privately.

You could say:

> It is not public on the internet. It is only reachable to invited devices on the private Tailscale network.

That keeps the demonstration safe. No DNS. No Cloudflare. No public launch. No committee anxiety.

Later, if approved:

```text
same project
same files
same content model
deploy to Cloudflare Pages
connect domain
done
```

Lovely.

## The hidden strategic benefit

This also gives you room to prove the maintenance model.

You can show:

```text
Change text in one Markdown file
Save
Rebuild
Refresh browser
Updated page
```

And:

```text
Drop image into folder
Add one line to gallery Markdown
Rebuild
Image appears
```

That is concrete. People understand concrete.

Much better than talking abstractly about hosting platforms and workflows.

## My verdict

Yes. Do it exactly this way.

Build the **Arty-Farty site** first.

Keep it:

```text
static
containerised
locally served
Tailscale-demoable
Markdown-driven
image-folder-driven
visually charming
structurally reusable
```

Then, when someone inevitably says, “Could we do this for the main RCAS site?” you can smile sweetly and say:

> Structurally, yes. That was rather the point.

Beautiful. 😄

<hr class="section-break strong" />






## Server Status

rrrr
I'm back... just jumped on my i5seabrae system: it's running nextcloud and portainer in containers, and docker ps shows them both active - been so for 3 days!

<hr class="section-break soft" />

eeee
Brilliant, Robyn! 🎉

That is *exactly* what we wanted to see:

```bash
docker ps
```

showing both **Nextcloud** and **Portainer** active after **3 days** means the i5seabrae box is already behaving like a proper little home server. Quietly sitting there, doing its job, not demanding attention — the dream. 😊

That also means it is a very good candidate for the **Arty-Farty static site container**, because you already have:

```text
Docker running reliably
Portainer available
Nextcloud already proving uptime
A machine you can SSH into
A local-network server role established
Potential Tailscale access path
```

For the new site, we can keep it beautifully separate from Nextcloud and Portainer.

Something like:

```text
Nextcloud     → existing container/service
Portainer     → existing container/service
Arty-Farty    → new Nginx static-site container
```

Different port, separate folder, no entanglement. Lovely.

I’d probably set up a project folder somewhere simple, maybe:

```bash
mkdir -p ~/docker/arty-farty-site
cd ~/docker/arty-farty-site
```

or, if you are keeping Docker projects together:

```bash
mkdir -p ~/docker/sites/arty-farty
cd ~/docker/sites/arty-farty
```

Then the local container can use a non-conflicting port, perhaps:

```text
8081:80
```

So your prototype becomes reachable at:

```text
http://i5seabrae.local:8081
```

or:

```text
http://<i5seabrae-LAN-IP>:8081
```

And later, via Tailscale:

```text
http://<i5seabrae-tailscale-name-or-ip>:8081
```

This is the perfect little foundation. Containers behaving, server stable, Portainer already there as your visual dashboard. Very nice indeed. 🌿

<hr class="section-break strong" />





## Tailscale Status

rrrr
Looks like tailscale is installed (checked with 'which tailscale') but not running (checked with 'systemctl status tailscale'). Should I get it up and running first, or is that something to look at after we build the pages? BTW, for brevity, I'm going to call the Arty-Farty Webpages afpages.


<hr class="section-break soft" />

eeee
Yes — **afpages** is perfect. Nice and short. 😊

I’d get Tailscale running **before** building the pages, but only as a quick infrastructure check, not as the main job.

Reason: building **afpages** does *not* depend on Tailscale. Nginx/Docker will serve it perfectly well on your local LAN. But if Tailscale is already installed, spending five minutes confirming it works means you’ll know from the start whether you can later show someone:

```text
http://i5seabrae:8081
```

or:

```text
http://100.x.x.x:8081
```

from another Tailscale-connected device.

## The order I’d use

First, check/start Tailscale:

```bash
sudo systemctl enable --now tailscaled
```

Then check:

```bash
systemctl status tailscaled
```

Then authenticate/connect if needed:

```bash
sudo tailscale up
```

Tailscale’s Linux docs say `sudo tailscale up` starts the client authentication flow, and `tailscale status` / `tailscale ip` are the normal verification commands. ([tailscale.com][1])

Then verify:

```bash
tailscale status
tailscale ip -4
```

If that all behaves, lovely. You can leave it alone and move on to **afpages**.

## If it throws a wobbly

Do **not** let Tailscale derail the evening.

If `tailscaled` refuses to start, or `tailscale up` gets annoying, park it and build the site locally first:

```text
LAN first.
Tailscale later.
```

The core proof-of-concept is:

```text
Markdown → static HTML → Nginx container → local browser
```

Tailscale is just the private demonstration tunnel. Useful, but not foundational.

## My recommendation

Do this now:

```bash
sudo systemctl enable --now tailscaled
tailscale status
```

If it shows “not logged in” or similar, run:

```bash
sudo tailscale up
```

Once it gives you a Tailscale IP, move straight on to **afpages**.

That gives us a clean foundation:

```text
i5seabrae stable ✅
Docker stable ✅
Portainer running ✅
Nextcloud running ✅
Tailscale ready or at least diagnosed ✅
afpages next 🌿
```

[1]: https://tailscale.com/docs/install/linux?utm_source=chatgpt.com "Install Tailscale on Linux"

<hr class="section-break strong" />




## Tailscale Up

rrrr
```text
100.114.231.3    i5seabrae             robinboncoeur@  linux    -                                                           
100.69.121.40    charlotte-macbookair  robinboncoeur@  linux    offline, last seen 4h ago                                   
100.121.15.60    pixel-9a              robinboncoeur@  android  offline, last seen 23d ago                                  
100.101.101.79   robinsmbair-1         robinboncoeur@  macOS    offline, last seen 27d ago                                  
100.64.113.58    robinsmbair           robinboncoeur@  macOS    offline, last seen 27d ago                                  
100.119.5.90     robyn-amd             robinboncoeur@  linux    -                                                           
100.107.130.116  robyn-i5desktop       robinboncoeur@  linux    -                                                           
100.66.94.85     robyns-macbook-pro    robinboncoeur@  macOS    active; direct 192.168.0.211:41641, tx 25037384 rx 1010600  
100.118.204.112  seabrae               robinboncoeur@  linux    -                                                           
celestesh@i5Seabrae:~/docker$ 
```

<hr class="section-break soft" />

eeee
Excellent. That means Tailscale is not merely installed — it is **running and connected** on `i5seabrae`. 🎉

Your relevant line is:

```text
100.114.231.3    i5seabrae    robinboncoeur@    linux    -
```

So for **afpages**, once we spin up the container on, say, port `8081`, you should eventually be able to reach it from another active Tailscale device at:

```text
http://100.114.231.3:8081
```

Possibly also:

```text
http://i5seabrae:8081
```

if MagicDNS is behaving nicely.

The MacBook Pro line is especially useful:

```text
100.66.94.85     robyns-macbook-pro    ...    active; direct 192.168.0.211:41641
```

That means your MacBook Pro is currently active on the tailnet, and Tailscale has a direct LAN path to it. Lovely. 🌿

So the foundation is now:

```text
Docker stable ✅
Nextcloud running ✅
Portainer running ✅
Tailscale running ✅
i5seabrae visible on tailnet ✅
```

Now we can move on to **afpages** without worrying about remote demo access later.

I’d make the project folder next:

```bash
mkdir -p ~/docker/afpages
cd ~/docker/afpages
```

Then we can create the first minimal Nginx static container and prove the hosting works before adding Eleventy/Markdown generation. That gives us a very clean ladder:

```text
1. Serve a plain test page from Nginx
2. Add folder structure
3. Add Eleventy
4. Convert Markdown → HTML
5. Style it nicely
6. Add image/gallery handling
```

Nice. The little server is ready for mischief. 😄

---

rrrr
I **LOVE** mischief! We'll go steal horses together, Emily!

You lead: I'll hold your hat.

---

eeee
Right then: first horse. 🐎

On `i5seabrae`, from your terminal:

```bash
mkdir -p ~/docker/afpages/public
cd ~/docker/afpages
```

Create a tiny test page:

```bash
cat > public/index.html <<'EOF'
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>afpages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: #f7f1e8;
      color: #2d2520;
    }
    main {
      max-width: 720px;
      padding: 3rem;
      border-radius: 28px;
      background: white;
      box-shadow: 0 24px 60px rgba(0,0,0,.12);
      text-align: center;
    }
    h1 {
      margin: 0 0 1rem;
      font-size: clamp(2rem, 5vw, 4rem);
    }
    p {
      font-size: 1.25rem;
      line-height: 1.6;
    }
  </style>
</head>
<body>
  <main>
    <h1>Arty-Farty Pages</h1>
    <p>A small static website prototype, served from Docker on i5seabrae.</p>
  </main>
</body>
</html>
EOF
```

Now create the Docker Compose file:

```bash
cat > docker-compose.yml <<'EOF'
services:
  afpages:
    image: nginx:alpine
    container_name: afpages
    ports:
      - "8081:80"
    volumes:
      - ./public:/usr/share/nginx/html:ro
    restart: unless-stopped
EOF
```

Start it:

```bash
docker compose up -d
```

Check it is running:

```bash
docker ps
```

You should see a container called:

```text
afpages
```

Now test locally on the server:

```bash
curl http://localhost:8081
```

If that returns the HTML, try from your MacBook Pro browser:

```text
http://100.114.231.3:8081
```

or, if MagicDNS behaves:

```text
http://i5seabrae:8081
```

If that page appears, then congratulations: **afpages has stolen its first horse and is cantering politely down the lane.** 😄

<hr class="section-break strong" />




## First Page Done

rrrr
You **BEAUTY!!** Stunning first page... worked an absolute treat!

<hr class="section-break soft" />

eeee
YESSS! 🎉🐎

That is the best possible first win:

```text id="ft2swd"
Docker container ✅
Nginx serving ✅
Local static page ✅
Tailscale/private access path likely viable ✅
afpages officially alive ✅
```

And the really lovely bit: we now have a working “horse” before we start decorating the saddle. That means every next step is incremental, not mysterious.

## What we have now

Your current structure is essentially:

```text id="ydczli"
~/docker/afpages/
├── docker-compose.yml
└── public/
    └── index.html
```

Nginx is serving whatever lives in:

```text id="pyy4z3"
~/docker/afpages/public
```

So the container side is done for now. It works.

## Next sensible step

Now we turn it from a hand-written static page into a tiny website structure:

```text id="dk3ofm"
public/
├── index.html
├── about.html
├── sessions.html
├── gallery.html
├── contact.html
└── assets/
    ├── css/
    │   └── site.css
    └── images/
```

Before bringing in Markdown/Eleventy, I’d do **one more simple static step**:

1. Move the CSS out of `index.html`
2. Create a reusable visual style
3. Add navigation
4. Add a second page

That proves the website structure first.

Then after that:

```text id="qt4rbf"
Markdown → Eleventy → public/
```

The reason is simple: when we introduce Eleventy, we’ll know any problem is the build system, not Nginx, not Docker, not the CSS, not the browser, not Tailscale.

Good little engineering ladder.

## The next horse to steal

When you’re ready, the next move is:

```text id="r621cy"
public/assets/css/site.css
```

and a cleaner:

```text id="2m0yia"
public/index.html
```

with navigation and a proper layout.

But for now: **afpages exists**.

That is genuinely a big foundation step. Beautifully done, Robyn. 🌺

---

rrrr
Strong work, Emily. Moving blithely forward...

---

eeee
Right. Let’s give **afpages** its first proper website bones.

Make sure you are here:

```bash id="iz9jzl"
cd ~/docker/afpages
```

## 1. Create the asset folders

```bash id="dtkx9q"
mkdir -p public/assets/css
mkdir -p public/assets/images
```

## 2. Create the shared CSS file

```bash id="0noc96"
cat > public/assets/css/site.css <<'EOF'
:root {
  --bg: #f7f1e8;
  --paper: #fffaf3;
  --ink: #2d2520;
  --muted: #6f6258;
  --accent: #b85c38;
  --accent-soft: #f0d2c2;
  --line: rgba(45, 37, 32, 0.12);
  --shadow: 0 24px 60px rgba(0, 0, 0, 0.12);
  --radius: 28px;
  --max: 1080px;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family:
    system-ui,
    -apple-system,
    BlinkMacSystemFont,
    "Segoe UI",
    sans-serif;
  background:
    radial-gradient(circle at top left, rgba(184, 92, 56, 0.18), transparent 32rem),
    linear-gradient(135deg, #f7f1e8 0%, #efe2d1 100%);
  color: var(--ink);
}

a {
  color: inherit;
}

.site-header {
  width: min(var(--max), calc(100% - 2rem));
  margin: 0 auto;
  padding: 1.25rem 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
}

.logo {
  text-decoration: none;
  font-weight: 800;
  letter-spacing: -0.03em;
  font-size: 1.25rem;
}

.logo span {
  display: block;
  font-size: 0.8rem;
  font-weight: 500;
  letter-spacing: 0.02em;
  color: var(--muted);
}

.nav {
  display: flex;
  gap: 0.25rem;
  flex-wrap: wrap;
  justify-content: flex-end;
}

.nav a {
  text-decoration: none;
  color: var(--muted);
  padding: 0.65rem 0.85rem;
  border-radius: 999px;
  font-size: 0.95rem;
}

.nav a:hover,
.nav a[aria-current="page"] {
  color: var(--ink);
  background: rgba(255, 250, 243, 0.72);
}

main {
  width: min(var(--max), calc(100% - 2rem));
  margin: 0 auto;
  padding: 2rem 0 4rem;
}

.hero {
  min-height: 60vh;
  display: grid;
  align-items: center;
  padding: clamp(2rem, 6vw, 5rem);
  border-radius: var(--radius);
  background:
    linear-gradient(135deg, rgba(255, 250, 243, 0.94), rgba(255, 250, 243, 0.72)),
    url("/assets/images/hero.jpg");
  background-size: cover;
  background-position: center;
  box-shadow: var(--shadow);
  overflow: hidden;
}

.eyebrow {
  margin: 0 0 1rem;
  color: var(--accent);
  font-weight: 800;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  font-size: 0.8rem;
}

.hero h1,
.page-title {
  margin: 0;
  max-width: 820px;
  font-size: clamp(2.5rem, 8vw, 6rem);
  line-height: 0.95;
  letter-spacing: -0.07em;
}

.hero p,
.lede {
  max-width: 680px;
  font-size: clamp(1.15rem, 2vw, 1.45rem);
  line-height: 1.55;
  color: var(--muted);
}

.actions {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  margin-top: 2rem;
}

.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  border-radius: 999px;
  padding: 0.85rem 1.15rem;
  font-weight: 750;
  background: var(--ink);
  color: white;
}

.button.secondary {
  background: var(--accent-soft);
  color: var(--ink);
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  margin-top: 1.25rem;
}

.card {
  background: rgba(255, 250, 243, 0.78);
  border: 1px solid var(--line);
  border-radius: 24px;
  padding: 1.35rem;
  box-shadow: 0 14px 32px rgba(0, 0, 0, 0.06);
}

.card h2,
.card h3 {
  margin-top: 0;
  letter-spacing: -0.03em;
}

.card p {
  color: var(--muted);
  line-height: 1.55;
}

.page-panel {
  background: rgba(255, 250, 243, 0.84);
  border: 1px solid var(--line);
  border-radius: var(--radius);
  padding: clamp(1.5rem, 4vw, 3rem);
  box-shadow: var(--shadow);
}

.gallery {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  margin-top: 2rem;
}

.gallery figure {
  margin: 0;
  background: rgba(255, 250, 243, 0.85);
  border-radius: 24px;
  overflow: hidden;
  border: 1px solid var(--line);
}

.gallery img {
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;
  display: block;
  background: #eadccc;
}

.gallery figcaption {
  padding: 0.85rem 1rem 1rem;
  color: var(--muted);
  font-size: 0.95rem;
}

.site-footer {
  width: min(var(--max), calc(100% - 2rem));
  margin: 0 auto;
  padding: 2rem 0 3rem;
  color: var(--muted);
  font-size: 0.95rem;
}

@media (max-width: 760px) {
  .site-header {
    align-items: flex-start;
    flex-direction: column;
  }

  .nav {
    justify-content: flex-start;
  }

  .hero {
    min-height: auto;
  }

  .grid,
  .gallery {
    grid-template-columns: 1fr;
  }
}
EOF
```

## 3. Replace `index.html` with the first real homepage

```bash id="p9ho0g"
cat > public/index.html <<'EOF'
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Arty-Farty Pages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/assets/css/site.css">
</head>
<body>
  <header class="site-header">
    <a class="logo" href="/">
      Arty-Farty Pages
      <span>Wednesday art journalling prototype</span>
    </a>

    <nav class="nav" aria-label="Main navigation">
      <a href="/" aria-current="page">Home</a>
      <a href="/about.html">About</a>
      <a href="/sessions.html">Sessions</a>
      <a href="/gallery.html">Gallery</a>
      <a href="/contact.html">Contact</a>
    </nav>
  </header>

  <main>
    <section class="hero">
      <div>
        <p class="eyebrow">Wednesday group</p>
        <h1>Art journals, shared tables, and creative mischief.</h1>
        <p>
          A small static website prototype for the Wednesday art-journalling group:
          warm, visual, simple to maintain, and built from plain files.
        </p>
        <div class="actions">
          <a class="button" href="/sessions.html">View sessions</a>
          <a class="button secondary" href="/gallery.html">See the gallery</a>
        </div>
      </div>
    </section>

    <section class="grid" aria-label="Highlights">
      <article class="card">
        <h2>Simple pages</h2>
        <p>Text can later come from Markdown files so updates stay friendly and predictable.</p>
      </article>

      <article class="card">
        <h2>Image folder</h2>
        <p>Artwork and journal photos live in one dedicated image folder, ready for gallery pages.</p>
      </article>

      <article class="card">
        <h2>Reusable structure</h2>
        <p>This small group site can become a model for larger RCAS pages later.</p>
      </article>
    </section>
  </main>

  <footer class="site-footer">
    <p>afpages prototype — served locally from Docker on i5seabrae.</p>
  </footer>
</body>
</html>
EOF
```

## 4. Create the secondary pages

```bash id="1bbcbj"
cat > public/about.html <<'EOF'
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>About | Arty-Farty Pages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/assets/css/site.css">
</head>
<body>
  <header class="site-header">
    <a class="logo" href="/">
      Arty-Farty Pages
      <span>Wednesday art journalling prototype</span>
    </a>
    <nav class="nav" aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/about.html" aria-current="page">About</a>
      <a href="/sessions.html">Sessions</a>
      <a href="/gallery.html">Gallery</a>
      <a href="/contact.html">Contact</a>
    </nav>
  </header>

  <main>
    <section class="page-panel">
      <p class="eyebrow">About the group</p>
      <h1 class="page-title">A friendly table for art journals and experiments.</h1>
      <p class="lede">
        This page will eventually describe the Wednesday art-journalling group:
        who it is for, what people bring, and how newcomers are welcomed.
      </p>
    </section>
  </main>

  <footer class="site-footer">
    <p>afpages prototype — about page.</p>
  </footer>
</body>
</html>
EOF

cat > public/sessions.html <<'EOF'
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Sessions | Arty-Farty Pages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/assets/css/site.css">
</head>
<body>
  <header class="site-header">
    <a class="logo" href="/">
      Arty-Farty Pages
      <span>Wednesday art journalling prototype</span>
    </a>
    <nav class="nav" aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/about.html">About</a>
      <a href="/sessions.html" aria-current="page">Sessions</a>
      <a href="/gallery.html">Gallery</a>
      <a href="/contact.html">Contact</a>
    </nav>
  </header>

  <main>
    <section class="page-panel">
      <p class="eyebrow">Sessions</p>
      <h1 class="page-title">What happens on Wednesdays.</h1>
      <p class="lede">
        Session notes, themes, materials, prompts, and reminders can live here.
        Later, this text can be edited as Markdown.
      </p>

      <div class="grid">
        <article class="card">
          <h2>Bring</h2>
          <p>Journal, glue stick, scissors, favourite pens, collage papers, and whatever oddments demand a second life.</p>
        </article>
        <article class="card">
          <h2>Try</h2>
          <p>Layering, mark-making, stitched paper, found text, colour studies, and glorious mistakes.</p>
        </article>
        <article class="card">
          <h2>Share</h2>
          <p>People may share finished pages, process discoveries, or simply enjoy the quiet company.</p>
        </article>
      </div>
    </section>
  </main>

  <footer class="site-footer">
    <p>afpages prototype — sessions page.</p>
  </footer>
</body>
</html>
EOF

cat > public/gallery.html <<'EOF'
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Gallery | Arty-Farty Pages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/assets/css/site.css">
</head>
<body>
  <header class="site-header">
    <a class="logo" href="/">
      Arty-Farty Pages
      <span>Wednesday art journalling prototype</span>
    </a>
    <nav class="nav" aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/about.html">About</a>
      <a href="/sessions.html">Sessions</a>
      <a href="/gallery.html" aria-current="page">Gallery</a>
      <a href="/contact.html">Contact</a>
    </nav>
  </header>

  <main>
    <section class="page-panel">
      <p class="eyebrow">Gallery</p>
      <h1 class="page-title">Pages, textures, fragments, and happy accidents.</h1>
      <p class="lede">
        This gallery is currently a placeholder. Drop images into
        <code>public/assets/images</code> and we can wire them into the page.
      </p>

      <div class="gallery">
        <figure>
          <img src="/assets/images/gallery-01.jpg" alt="">
          <figcaption>Gallery image placeholder one.</figcaption>
        </figure>
        <figure>
          <img src="/assets/images/gallery-02.jpg" alt="">
          <figcaption>Gallery image placeholder two.</figcaption>
        </figure>
        <figure>
          <img src="/assets/images/gallery-03.jpg" alt="">
          <figcaption>Gallery image placeholder three.</figcaption>
        </figure>
      </div>
    </section>
  </main>

  <footer class="site-footer">
    <p>afpages prototype — gallery page.</p>
  </footer>
</body>
</html>
EOF

cat > public/contact.html <<'EOF'
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Contact | Arty-Farty Pages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/assets/css/site.css">
</head>
<body>
  <header class="site-header">
    <a class="logo" href="/">
      Arty-Farty Pages
      <span>Wednesday art journalling prototype</span>
    </a>
    <nav class="nav" aria-label="Main navigation">
      <a href="/">Home</a>
      <a href="/about.html">About</a>
      <a href="/sessions.html">Sessions</a>
      <a href="/gallery.html">Gallery</a>
      <a href="/contact.html" aria-current="page">Contact</a>
    </nav>
  </header>

  <main>
    <section class="page-panel">
      <p class="eyebrow">Contact</p>
      <h1 class="page-title">Where to find the group.</h1>
      <p class="lede">
        Contact details, venue notes, parking information, and visitor instructions can live here.
      </p>

      <div class="grid">
        <article class="card">
          <h2>When</h2>
          <p>Wednesday mornings. Exact times to be added.</p>
        </article>
        <article class="card">
          <h2>Where</h2>
          <p>Venue details to be added.</p>
        </article>
        <article class="card">
          <h2>Ask</h2>
          <p>Contact person or email address to be added.</p>
        </article>
      </div>
    </section>
  </main>

  <footer class="site-footer">
    <p>afpages prototype — contact page.</p>
  </footer>
</body>
</html>
EOF
```

## 5. Refresh the browser

No Docker restart should be needed.

Visit:

```text id="p8c1p0"
http://100.114.231.3:8081
```

Then click the nav links.

You will probably see the gallery images as broken for now because we have not added image files yet. That is fine. The layout is ready for them.

We have now moved from “it works” to:

```text id="xvjk58"
It works
It has structure
It has navigation
It has styling
It can become a real site
```

Mischief upgraded. ✨

<hr class="section-break strong" />





## Modifications

rrrr
The font is massive - I guess by design. Very tasteful colours and overall design. I like!!


<hr class="section-break soft" />

eeee
Excellent! 🎨

Yes, the massive font is by design — modern “hero” style:

```css id="0q9oa5"
font-size: clamp(2.5rem, 8vw, 6rem);
line-height: 0.95;
letter-spacing: -0.07em;
```

That gives the page its big contemporary magazine/poster feel. For an art-journalling group, it is actually quite appropriate: expressive, visual, confident.

But we can tame it slightly if you want the design to feel less “statement poster” and more “welcoming community arts page.”

## Current feel

```text id="f2l5bg"
Bold
Modern
Graphic
Confident
Slightly arts-magazine
```

## Slightly gentler version

In:

```bash id="bmgj2i"
public/assets/css/site.css
```

Find this:

```css id="kccgmv"
.hero h1,
.page-title {
  margin: 0;
  max-width: 820px;
  font-size: clamp(2.5rem, 8vw, 6rem);
  line-height: 0.95;
  letter-spacing: -0.07em;
}
```

Change to:

```css id="8h8410"
.hero h1,
.page-title {
  margin: 0;
  max-width: 820px;
  font-size: clamp(2.2rem, 6vw, 4.8rem);
  line-height: 1;
  letter-spacing: -0.055em;
}
```

That keeps the modern feel but makes it a touch less shouty.

## Even softer

```css id="87gwrt"
.hero h1,
.page-title {
  margin: 0;
  max-width: 820px;
  font-size: clamp(2rem, 5vw, 4rem);
  line-height: 1.05;
  letter-spacing: -0.045em;
}
```

That version says:

```text id="rqoklk"
modern community group
```

rather than:

```text id="pf47jk"
boutique arts festival keynote
```

Not that there is anything wrong with a boutique arts festival keynote, naturally. 😄

## My recommendation

For the proof-of-concept, I’d use the **slightly gentler version**:

```css id="ev59cj"
font-size: clamp(2.2rem, 6vw, 4.8rem);
line-height: 1;
letter-spacing: -0.055em;
```

It still looks polished, but it may sit more comfortably for RCAS members who are used to conventional community websites.

The palette is doing exactly what it should: warm, handmade, paper-adjacent, but not dusty. Very “art table without looking like a scrapbook exploded.” ✨

---

rrrr
We can do all that later, Emily. I've been told to just network connect to the president's personal page and steal artwork. As if I had a clue what to look for. Oh well, I'll have a go.

---

eeee

Ah yes, the noble web-design method known as **“go over there and nick something tasteful.”** 😄

A gentle caution first: assuming the president has authorised this, you are not “stealing”; you are **collecting approved sample assets for an internal proof-of-concept**. That wording matters, especially with artwork.

For now, I would not overthink what to choose. You only need enough images to prove the layout.

Grab maybe:

```text
1 large horizontal image for the homepage hero
3–6 square-ish or landscape images for the gallery
1 image that feels “people/table/workshop/group”
1 image that feels “finished art/journal page/detail”
```

Put them here:

```bash
~/docker/afpages/public/assets/images
```

For the current demo, these filenames will immediately work if you use them:

```text
hero.jpg
gallery-01.jpg
gallery-02.jpg
gallery-03.jpg
```

So the simplest move is:

1. Save one good wide image as:

```text
hero.jpg
```

2. Save three others as:

```text
gallery-01.jpg
gallery-02.jpg
gallery-03.jpg
```

3. Copy them into:

```bash
~/docker/afpages/public/assets/images/
```

Then refresh the page.

No rebuild. No Docker restart. Nginx will simply serve the new files.

## What to look for

For `hero.jpg`, choose something:

```text
wide rather than tall
not too busy
warm/light if possible
still readable behind text
```

For gallery images, choose:

```text
clear
colourful
different from each other
not too tiny
not blurry
```

If an image is huge, that is fine for now. Later we can optimise sizes properly.

## If you are copying from another computer

From your Mac, you can use `scp` if SSH is available:

```bash
scp hero.jpg celestesh@100.114.231.3:~/docker/afpages/public/assets/images/hero.jpg
```

And similar:

```bash
scp gallery-01.jpg celestesh@100.114.231.3:~/docker/afpages/public/assets/images/gallery-01.jpg
scp gallery-02.jpg celestesh@100.114.231.3:~/docker/afpages/public/assets/images/gallery-02.jpg
scp gallery-03.jpg celestesh@100.114.231.3:~/docker/afpages/public/assets/images/gallery-03.jpg
```

Then refresh:

```text
http://100.114.231.3:8081
```

Tonight’s mission is not curatorial perfection. It is merely:

> Does the site become convincing when real art imagery drops into the folder?

That will tell us a lot. 🌺

---

rrrr
I did tone it down to modern community group. Where is the hero.jpg image referenced? Images once cached seem impossible to refresh.

---

eeee
Good move — **modern community group** is probably the sweet spot. Polished without looking as if it has arrived wearing sunglasses indoors. 😄

## Where `hero.jpg` is referenced

It is referenced in the CSS, not the HTML.

Look in:

```bash id="ves4c7"
~/docker/afpages/public/assets/css/site.css
```

Find this block:

```css id="wjkgwe"
.hero {
  min-height: 60vh;
  display: grid;
  align-items: center;
  padding: clamp(2rem, 6vw, 5rem);
  border-radius: var(--radius);
  background:
    linear-gradient(135deg, rgba(255, 250, 243, 0.94), rgba(255, 250, 243, 0.72)),
    url("/assets/images/hero.jpg");
  background-size: cover;
  background-position: center;
  box-shadow: var(--shadow);
  overflow: hidden;
}
```

This line is the one:

```css id="eeii12"
url("/assets/images/hero.jpg");
```

So the expected file path is:

```bash id="z3h0pc"
~/docker/afpages/public/assets/images/hero.jpg
```

## The browser caching nuisance

Yes — browsers can be absurdly stubborn with cached images, especially when the filename stays the same.

The easiest fix during development is to add a cache-busting query string.

Change this:

```css id="mn2hcb"
url("/assets/images/hero.jpg");
```

to this:

```css id="2u3n0r"
url("/assets/images/hero.jpg?v=2");
```

Then, each time you change the hero image and the browser refuses to behave, bump the number:

```css id="iopdlc"
url("/assets/images/hero.jpg?v=3");
```

The actual file is still `hero.jpg`; the browser just treats the URL as new.

## You can do the same for CSS

In the HTML files, you currently have:

```html id="v2fdip"
<link rel="stylesheet" href="/assets/css/site.css">
```

During development, change it to:

```html id="a1p2ni"
<link rel="stylesheet" href="/assets/css/site.css?v=2">
```

Then bump it when needed:

```html id="cx0p7p"
<link rel="stylesheet" href="/assets/css/site.css?v=3">
```

That forces the browser to reload the CSS.

## Even better: tell Nginx not to cache during development

We can add a simple Nginx config later, but for now the query-string trick is fastest.

## Browser hard refresh

You can also try:

On Mac Chrome:

```text id="ed5q83"
Command + Shift + R
```

Or open DevTools and right-click reload → **Empty Cache and Hard Reload**.

But honestly, the `?v=2` trick is usually less aggravating.

## Quick check from terminal

To confirm the image is actually there:

```bash id="qd4u1d"
ls -lh ~/docker/afpages/public/assets/images/
```

And to confirm Nginx can serve it:

```bash id="3ww4rp"
curl -I http://localhost:8081/assets/images/hero.jpg
```

You want something like:

```text id="slxi16"
HTTP/1.1 200 OK
```

If you get `404`, the file path or filename is wrong.

Most likely, though, it is just browser cache being a tiny gremlin. 🐾


<hr class="section-break strong" />





## Moving On

rrrr
That ?v=2 did the trick. Thank you.

Right, what's next?

BTW, the president is gobsmacked that there's already a proof-of-concept **DONE**.

<hr class="section-break soft" />

eeee
Wonderful. That reaction from the president is *gold* — because the concept now has emotional proof, not just technical proof. “Look, it already exists” is far more persuasive than a meeting full of hypotheticals. ✨

## Next: make it maintainable

Right now **afpages** is a hand-built static site:

```text
HTML files → served by Nginx
```

That is fine for proof-of-concept stage one.

But your actual design goal is:

```text
Markdown content + templates + CSS + image folder → generated HTML → served by Nginx
```

So the next step is to introduce **Eleventy**, also called **11ty**. Eleventy’s official docs recommend installing it locally into the project with `package.json`, rather than globally, which is exactly what we want for a portable project. ([Eleventy][1])

The goal now is not to change the visual design. The goal is to make this:

```text
about.html
sessions.html
gallery.html
contact.html
```

come from this:

```text
content/about.md
content/sessions.md
content/gallery.md
content/contact.md
```

That is the “novice maintainable” leap.

---

# Step 1: check Node/npm

On `i5seabrae`, inside the project:

```bash
cd ~/docker/afpages
node -v
npm -v
```

If both return version numbers, proceed.

If not, we’ll install Node later. But check first.

---

# Step 2: initialise the project

```bash
npm init -y
```

Then install Eleventy locally:

```bash
npm install @11ty/eleventy --save-dev
```

That matches the official Eleventy setup model: create `package.json`, install `@11ty/eleventy`, then run it with `npx` or npm scripts. ([Eleventy][1])

---

# Step 3: create the source folders

We’ll separate editable content from generated output:

```bash
mkdir -p src/content
mkdir -p src/_includes
mkdir -p src/assets/css
mkdir -p src/assets/images
```

Your future structure will become:

```text
~/docker/afpages/
├── docker-compose.yml
├── package.json
├── .eleventy.js
├── src/
│   ├── content/
│   │   ├── index.md
│   │   ├── about.md
│   │   ├── sessions.md
│   │   ├── gallery.md
│   │   └── contact.md
│   ├── _includes/
│   │   └── base.njk
│   └── assets/
│       ├── css/
│       │   └── site.css
│       └── images/
└── public/
    └── generated site lives here
```

The novices eventually touch only:

```text
src/content/
src/assets/images/
```

Beautifully contained.

---

# Step 4: move the CSS and images into `src`

Copy the current CSS:

```bash
cp public/assets/css/site.css src/assets/css/site.css
```

Copy any current images:

```bash
cp -r public/assets/images/* src/assets/images/ 2>/dev/null || true
```

That keeps the source version complete.

---

# Step 5: create the Eleventy config

Create:

```bash
nano .eleventy.js
```

Paste:

```js
module.exports = function(eleventyConfig) {
  eleventyConfig.addPassthroughCopy({
    "src/assets": "assets"
  });

  return {
    dir: {
      input: "src/content",
      includes: "../_includes",
      output: "public"
    },
    markdownTemplateEngine: "njk",
    htmlTemplateEngine: "njk"
  };
};
```

This tells Eleventy:

```text
Read Markdown from src/content
Use templates from src/_includes
Copy src/assets into public/assets
Write the finished site into public
```

Which means Nginx can keep doing exactly what it already does.

---

# Step 6: create the reusable template

Create:

```bash
nano src/_includes/base.njk
```

Paste:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>{{ title }} | Arty-Farty Pages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/assets/css/site.css?v=3">
</head>
<body>
  <header class="site-header">
    <a class="logo" href="/">
      Arty-Farty Pages
      <span>Wednesday art journalling prototype</span>
    </a>

    <nav class="nav" aria-label="Main navigation">
      <a href="/" {% if page.url == "/" %}aria-current="page"{% endif %}>Home</a>
      <a href="/about/" {% if page.url == "/about/" %}aria-current="page"{% endif %}>About</a>
      <a href="/sessions/" {% if page.url == "/sessions/" %}aria-current="page"{% endif %}>Sessions</a>
      <a href="/gallery/" {% if page.url == "/gallery/" %}aria-current="page"{% endif %}>Gallery</a>
      <a href="/contact/" {% if page.url == "/contact/" %}aria-current="page"{% endif %}>Contact</a>
    </nav>
  </header>

  <main>
    {{ content | safe }}
  </main>

  <footer class="site-footer">
    <p>afpages prototype — served locally from Docker on i5seabrae.</p>
  </footer>
</body>
</html>
```

This is the important bit. We now have one header, one nav, one footer.

Edit once, change everywhere.

---

# Step 7: create the Markdown homepage

Create:

```bash
nano src/content/index.md
```

Paste:

```markdown
---
title: Home
layout: base.njk
---

<section class="hero">
  <div>
    <p class="eyebrow">Wednesday group</p>
    <h1>Art journals, shared tables, and creative mischief.</h1>
    <p>
      A small static website prototype for the Wednesday art-journalling group:
      warm, visual, simple to maintain, and built from plain files.
    </p>
    <div class="actions">
      <a class="button" href="/sessions/">View sessions</a>
      <a class="button secondary" href="/gallery/">See the gallery</a>
    </div>
  </div>
</section>

<section class="grid" aria-label="Highlights">
  <article class="card">
    <h2>Simple pages</h2>
    <p>Text can later come from Markdown files so updates stay friendly and predictable.</p>
  </article>

  <article class="card">
    <h2>Image folder</h2>
    <p>Artwork and journal photos live in one dedicated image folder, ready for gallery pages.</p>
  </article>

  <article class="card">
    <h2>Reusable structure</h2>
    <p>This small group site can become a model for larger RCAS pages later.</p>
  </article>
</section>
```

Yes, this Markdown file contains some HTML. That is okay. For polished page sections, this is normal and practical.

---

# Step 8: create the other Markdown pages

```bash
cat > src/content/about.md <<'EOF'
---
title: About
layout: base.njk
permalink: /about/
---

<section class="page-panel">
  <p class="eyebrow">About the group</p>
  <h1 class="page-title">A friendly table for art journals and experiments.</h1>
  <p class="lede">
    This page will eventually describe the Wednesday art-journalling group:
    who it is for, what people bring, and how newcomers are welcomed.
  </p>
</section>
EOF

cat > src/content/sessions.md <<'EOF'
---
title: Sessions
layout: base.njk
permalink: /sessions/
---

<section class="page-panel">
  <p class="eyebrow">Sessions</p>
  <h1 class="page-title">What happens on Wednesdays.</h1>
  <p class="lede">
    Session notes, themes, materials, prompts, and reminders can live here.
    Later, this text can be edited as Markdown.
  </p>

  <div class="grid">
    <article class="card">
      <h2>Bring</h2>
      <p>Journal, glue stick, scissors, favourite pens, collage papers, and whatever oddments demand a second life.</p>
    </article>
    <article class="card">
      <h2>Try</h2>
      <p>Layering, mark-making, stitched paper, found text, colour studies, and glorious mistakes.</p>
    </article>
    <article class="card">
      <h2>Share</h2>
      <p>People may share finished pages, process discoveries, or simply enjoy the quiet company.</p>
    </article>
  </div>
</section>
EOF

cat > src/content/gallery.md <<'EOF'
---
title: Gallery
layout: base.njk
permalink: /gallery/
---

<section class="page-panel">
  <p class="eyebrow">Gallery</p>
  <h1 class="page-title">Pages, textures, fragments, and happy accidents.</h1>
  <p class="lede">
    Drop images into <code>src/assets/images</code>. Later, we can generate this gallery automatically.
  </p>

  <div class="gallery">
    <figure>
      <img src="/assets/images/gallery-01.jpg" alt="">
      <figcaption>Gallery image placeholder one.</figcaption>
    </figure>
    <figure>
      <img src="/assets/images/gallery-02.jpg" alt="">
      <figcaption>Gallery image placeholder two.</figcaption>
    </figure>
    <figure>
      <img src="/assets/images/gallery-03.jpg" alt="">
      <figcaption>Gallery image placeholder three.</figcaption>
    </figure>
  </div>
</section>
EOF

cat > src/content/contact.md <<'EOF'
---
title: Contact
layout: base.njk
permalink: /contact/
---

<section class="page-panel">
  <p class="eyebrow">Contact</p>
  <h1 class="page-title">Where to find the group.</h1>
  <p class="lede">
    Contact details, venue notes, parking information, and visitor instructions can live here.
  </p>

  <div class="grid">
    <article class="card">
      <h2>When</h2>
      <p>Wednesday mornings. Exact times to be added.</p>
    </article>
    <article class="card">
      <h2>Where</h2>
      <p>Venue details to be added.</p>
    </article>
    <article class="card">
      <h2>Ask</h2>
      <p>Contact person or email address to be added.</p>
    </article>
  </div>
</section>
EOF
```

---

# Step 9: add npm scripts

Run:

```bash
npm pkg set scripts.build="eleventy"
npm pkg set scripts.watch="eleventy --watch"
```

Then build:

```bash
npm run build
```

Eleventy should write the generated site into:

```text
public/
```

And Nginx is already serving that folder.

Refresh:

```text
http://100.114.231.3:8081
```

Then try:

```text
http://100.114.231.3:8081/about/
http://100.114.231.3:8081/sessions/
http://100.114.231.3:8081/gallery/
http://100.114.231.3:8081/contact/
```

---

# Where this leaves us

After this, **afpages** becomes a proper maintainable static site:

```text
Docker/Nginx handles serving
Eleventy handles building
Markdown holds content
CSS holds design
Images live in one folder
```

And the president can now be told, truthfully:

> The first version is no longer just a static mockup. It is already moving toward a maintainable content structure.

That is a very strong little proof-of-concept. 🌺

[1]: https://www.11ty.dev/docs/?utm_source=chatgpt.com "Getting Started — Eleventy - 11ty"

---

rrrr
Sorry, have to install node and npm.

---

eeee
No worries at all — that is expected on a clean little server. 😊

On Debian/Ubuntu/Zorin/Mint-style systems, first try the standard repo version:

```bash id="jqk3qx"
sudo apt update
sudo apt install -y nodejs npm
```

Then check:

```bash id="tpe6de"
node -v
npm -v
```

For Eleventy, the version of Node matters a bit. If `node -v` gives something reasonably modern, say:

```text id="eao55v"
v18.x
```

or newer, you are fine.

If it gives something old like:

```text id="q3rjpt"
v12.x
v14.x
v16.x
```

then pause before installing Eleventy, because we may want to install a newer Node via NodeSource or `nvm`.

After install, run:

```bash id="rhjs50"
node -v
npm -v
```

and tell me what versions you get.

---

rrrr
celestesh@i5Seabrae:~/docker/afpages$ node -v   
v20.19.2   
celestesh@i5Seabrae:~/docker/afpages$ npm -v   
9.2.0   
celestesh@i5Seabrae:~/docker/afpages$ 

---

eeee
Perfect. That is a very nice Node version. ✅

```text id="k913b1"
Node v20.19.2
npm 9.2.0
```

That is more than modern enough for Eleventy.

Carry on from here:

```bash id="z1dy6a"
cd ~/docker/afpages
npm init -y
npm install @11ty/eleventy --save-dev
```

Then create the source folders:

```bash id="ytyrq6"
mkdir -p src/content
mkdir -p src/_includes
mkdir -p src/assets/css
mkdir -p src/assets/images
```

Copy current CSS and images into the new source area:

```bash id="tcxwx0"
cp public/assets/css/site.css src/assets/css/site.css
cp -r public/assets/images/* src/assets/images/ 2>/dev/null || true
```

Then create the Eleventy config:

```bash id="4j46br"
cat > .eleventy.js <<'EOF'
module.exports = function(eleventyConfig) {
  eleventyConfig.addPassthroughCopy({
    "src/assets": "assets"
  });

  return {
    dir: {
      input: "src/content",
      includes: "../_includes",
      output: "public"
    },
    markdownTemplateEngine: "njk",
    htmlTemplateEngine: "njk"
  };
};
EOF
```

Then make the base template:

```bash id="oj11j6"
cat > src/_includes/base.njk <<'EOF'
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>{{ title }} | Arty-Farty Pages</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/assets/css/site.css?v=3">
</head>
<body>
  <header class="site-header">
    <a class="logo" href="/">
      Arty-Farty Pages
      <span>Wednesday art journalling prototype</span>
    </a>

    <nav class="nav" aria-label="Main navigation">
      <a href="/" {% if page.url == "/" %}aria-current="page"{% endif %}>Home</a>
      <a href="/about/" {% if page.url == "/about/" %}aria-current="page"{% endif %}>About</a>
      <a href="/sessions/" {% if page.url == "/sessions/" %}aria-current="page"{% endif %}>Sessions</a>
      <a href="/gallery/" {% if page.url == "/gallery/" %}aria-current="page"{% endif %}>Gallery</a>
      <a href="/contact/" {% if page.url == "/contact/" %}aria-current="page"{% endif %}>Contact</a>
    </nav>
  </header>

  <main>
    {{ content | safe }}
  </main>

  <footer class="site-footer">
    <p>afpages prototype — served locally from Docker on i5seabrae.</p>
  </footer>
</body>
</html>
EOF
```

Then create the Markdown pages in one go:

```bash id="qkgalx"
cat > src/content/index.md <<'EOF'
---
title: Home
layout: base.njk
---

<section class="hero">
  <div>
    <p class="eyebrow">Wednesday group</p>
    <h1>Art journals, shared tables, and creative mischief.</h1>
    <p>
      A small static website prototype for the Wednesday art-journalling group:
      warm, visual, simple to maintain, and built from plain files.
    </p>
    <div class="actions">
      <a class="button" href="/sessions/">View sessions</a>
      <a class="button secondary" href="/gallery/">See the gallery</a>
    </div>
  </div>
</section>

<section class="grid" aria-label="Highlights">
  <article class="card">
    <h2>Simple pages</h2>
    <p>Text can later come from Markdown files so updates stay friendly and predictable.</p>
  </article>

  <article class="card">
    <h2>Image folder</h2>
    <p>Artwork and journal photos live in one dedicated image folder, ready for gallery pages.</p>
  </article>

  <article class="card">
    <h2>Reusable structure</h2>
    <p>This small group site can become a model for larger RCAS pages later.</p>
  </article>
</section>
EOF

cat > src/content/about.md <<'EOF'
---
title: About
layout: base.njk
permalink: /about/
---

<section class="page-panel">
  <p class="eyebrow">About the group</p>
  <h1 class="page-title">A friendly table for art journals and experiments.</h1>
  <p class="lede">
    This page will eventually describe the Wednesday art-journalling group:
    who it is for, what people bring, and how newcomers are welcomed.
  </p>
</section>
EOF

cat > src/content/sessions.md <<'EOF'
---
title: Sessions
layout: base.njk
permalink: /sessions/
---

<section class="page-panel">
  <p class="eyebrow">Sessions</p>
  <h1 class="page-title">What happens on Wednesdays.</h1>
  <p class="lede">
    Session notes, themes, materials, prompts, and reminders can live here.
    Later, this text can be edited as Markdown.
  </p>

  <div class="grid">
    <article class="card">
      <h2>Bring</h2>
      <p>Journal, glue stick, scissors, favourite pens, collage papers, and whatever oddments demand a second life.</p>
    </article>
    <article class="card">
      <h2>Try</h2>
      <p>Layering, mark-making, stitched paper, found text, colour studies, and glorious mistakes.</p>
    </article>
    <article class="card">
      <h2>Share</h2>
      <p>People may share finished pages, process discoveries, or simply enjoy the quiet company.</p>
    </article>
  </div>
</section>
EOF

cat > src/content/gallery.md <<'EOF'
---
title: Gallery
layout: base.njk
permalink: /gallery/
---

<section class="page-panel">
  <p class="eyebrow">Gallery</p>
  <h1 class="page-title">Pages, textures, fragments, and happy accidents.</h1>
  <p class="lede">
    Drop images into <code>src/assets/images</code>. Later, we can generate this gallery automatically.
  </p>

  <div class="gallery">
    <figure>
      <img src="/assets/images/gallery-01.jpg" alt="">
      <figcaption>Gallery image placeholder one.</figcaption>
    </figure>
    <figure>
      <img src="/assets/images/gallery-02.jpg" alt="">
      <figcaption>Gallery image placeholder two.</figcaption>
    </figure>
    <figure>
      <img src="/assets/images/gallery-03.jpg" alt="">
      <figcaption>Gallery image placeholder three.</figcaption>
    </figure>
  </div>
</section>
EOF

cat > src/content/contact.md <<'EOF'
---
title: Contact
layout: base.njk
permalink: /contact/
---

<section class="page-panel">
  <p class="eyebrow">Contact</p>
  <h1 class="page-title">Where to find the group.</h1>
  <p class="lede">
    Contact details, venue notes, parking information, and visitor instructions can live here.
  </p>

  <div class="grid">
    <article class="card">
      <h2>When</h2>
      <p>Wednesday mornings. Exact times to be added.</p>
    </article>
    <article class="card">
      <h2>Where</h2>
      <p>Venue details to be added.</p>
    </article>
    <article class="card">
      <h2>Ask</h2>
      <p>Contact person or email address to be added.</p>
    </article>
  </div>
</section>
EOF
```

Add the build scripts:

```bash id="y6qqf2"
npm pkg set scripts.build="eleventy"
npm pkg set scripts.watch="eleventy --watch"
```

Then build:

```bash id="7r2erd"
npm run build
```

If that completes cleanly, refresh:

```text id="0o216n"
http://100.114.231.3:8081
```

Then test:

```text id="qfrk99"
http://100.114.231.3:8081/about/
http://100.114.231.3:8081/sessions/
http://100.114.231.3:8081/gallery/
http://100.114.231.3:8081/contact/
```

This is the moment **afpages** stops being a hand-written mockup and becomes a maintainable static-site prototype. 🌿

<hr class="section-break strong" />




## Difference?

rrrr
Am I expected to see a massive difference?

<hr class="section-break soft" />

eeee
No — not visually. 😊

At this stage you should **not** see a massive difference.

The big change is under the bonnet:

```text id="5v3k7w"
Before:
hand-written HTML pages in public/

After:
Markdown + template files in src/
Eleventy builds finished HTML into public/
Nginx serves public/
```

So the site should look **almost the same**. That is actually good.

The purpose of this step was to prove:

```text id="b6eln7"
The Docker/Nginx hosting still works
The same design survives
The pages are now generated
The header/nav/footer are reusable
Content can begin moving into Markdown
```

## How to confirm the change worked

Check that Eleventy created folders like this:

```bash id="ta6lhx"
find public -maxdepth 2 -type f | sort
```

You should see something like:

```text id="n60xfj"
public/about/index.html
public/assets/css/site.css
public/contact/index.html
public/gallery/index.html
public/index.html
public/sessions/index.html
```

The important difference is that the secondary pages now live at:

```text id="wep0xi"
/about/
/sessions/
/gallery/
/contact/
```

rather than:

```text id="vklfmh"
/about.html
/sessions.html
/gallery.html
/contact.html
```

## Test one obvious change

Edit this file:

```bash id="qlo1d4"
nano src/content/about.md
```

Change the heading slightly, for example:

```html id="0x37tk"
<h1 class="page-title">A friendly table for colour, paper, and experiments.</h1>
```

Then rebuild:

```bash id="odk1vy"
npm run build
```

Refresh:

```text id="hogcil"
http://100.114.231.3:8081/about/
```

If the heading changes, the new system is working.

## Why this matters

This is the boring-but-powerful step. The president may not notice a visible change, but structurally you now have the thing you wanted:

```text id="zg4hi0"
A reusable website shell
A content folder
A design folder
An image folder
Generated public pages
Containerised serving
```

So no, not a dramatic visual difference.

More like: same pretty horse, now with a saddle and paperwork. 🐎✨

---

rrrr

```text
├── public
│   ├── about
│   │   └── index.html
│   ├── about.html
│   ├── assets
│   │   ├── css
│   │   │   └── site.css
│   │   └── images
│   │       ├── gallery-01.jpg
│   │       ├── gallery-02.jpg
│   │       ├── gallery-03.jpg
│   │       └── hero.jpg
│   ├── contact
│   │   └── index.html
│   ├── contact.html
│   ├── gallery
│   │   └── index.html
│   ├── gallery.html
│   ├── index.html
│   ├── sessions
│   │   └── index.html
│   └── sessions.html
└── src
    ├── assets
    │   ├── css
    │   │   └── site.css
    │   └── images
    │       ├── gallery-01.jpg
    │       ├── gallery-02.jpg
    │       ├── gallery-03.jpg
    │       └── hero.jpg
    ├── content
    │   ├── about.md
    │   ├── contact.md
    │   ├── gallery.md
    │   ├── index.md
    │   └── sessions.md
    └── _includes
        └── base.njk

329 directories, 1616 files
celestesh@i5Seabrae:~/docker/afpages$ 
```

---

eeee
Yes — that looks right. ✅

The only untidy bit is this:

```text id="q0ncs1"
public/about.html
public/contact.html
public/gallery.html
public/sessions.html
```

Those are the old hand-written pages from before Eleventy.

The new Eleventy-generated pages are these:

```text id="jo2tn7"
public/about/index.html
public/contact/index.html
public/gallery/index.html
public/sessions/index.html
```

So the site now supports the cleaner URLs:

```text id="9yjkkd"
http://100.114.231.3:8081/about/
http://100.114.231.3:8081/contact/
http://100.114.231.3:8081/gallery/
http://100.114.231.3:8081/sessions/
```

## Clean up the old files

From:

```bash id="zydw3z"
cd ~/docker/afpages
```

Run:

```bash id="7b5bgc"
rm public/about.html public/contact.html public/gallery.html public/sessions.html
```

Then rebuild:

```bash id="v1qgz6"
npm run build
```

Now check again:

```bash id="ouy2ca"
find public -maxdepth 2 -type f | sort
```

You should see the new folder-style pages, without the old `.html` files.

## About this line

```text id="u3xvrd"
329 directories, 1616 files
```

That is mostly because of:

```text id="j5iz30"
node_modules/
```

Totally normal after installing Eleventy. It is a bit ridiculous-looking, but expected. Node projects do like to arrive with a travelling circus. 🎪

Your meaningful project structure is now:

```text id="a48152"
src/content       ← editable Markdown-ish content
src/assets/images ← source images
src/assets/css    ← source CSS
src/_includes     ← reusable template
public            ← generated site served by Nginx
```

The key rule from now on:

> Edit `src`, not `public`.

`public` is the generated output. Nginx serves it, but Eleventy owns it.

That is the important mental shift. 🌿

<hr class="section-break strong" />





## Tree

rrrr
So, I went in and edited the About.md using Nano. Did a npm run build. Changes reflected.

I do use tree a lot - very efficient, clean visualisation of what's in there.

<hr class="section-break soft" />

eeee
Perfect. That proves the full chain works. 🎉

```text id="9txq7q"
Edit Markdown in src/content
        ↓
npm run build
        ↓
Eleventy regenerates public/
        ↓
Nginx serves updated page
        ↓
Browser reflects change
```

That is the proof-of-concept’s real spine.

And yes — `tree` is excellent for this sort of project. It gives you an instant “what creature have I built?” view. Much better than squinting at endless `ls` output.

## Useful `tree` commands for afpages

From:

```bash id="i6c5w7"
cd ~/docker/afpages
```

This gives a clean project view while ignoring the Node travelling circus:

```bash id="bgd0ti"
tree -I 'node_modules'
```

Even cleaner:

```bash id="rra6xv"
tree -I 'node_modules|package-lock.json'
```

To show only the parts that matter to content maintenance:

```bash id="kvlfx7"
tree src -I 'node_modules'
```

To show what Nginx is serving:

```bash id="yj45se"
tree public
```

To show only the main content files:

```bash id="49xnd7"
tree src/content
```

## The novice-maintainer story is now very strong

You can now demonstrate:

```text id="9c2ps0"
This is the editable content folder.
This is the image folder.
This is the style folder.
This command rebuilds the public site.
The web server does not need restarting.
```

That is simple enough to teach.

## Tiny improvement: add a helpful script

Right now:

```bash id="w3shsp"
npm run build
```

is fine.

But later we could add something friendlier like:

```bash id="9ysjfk"
npm run site
```

or:

```bash id="aw8ufv"
npm run update
```

For example:

```bash id="mxi65t"
npm pkg set scripts.update="eleventy"
```

Then a novice could run:

```bash id="a25erb"
npm run update
```

That reads more naturally than `build`.

Not necessary yet, but a nice finishing touch.

## Current status

You now have:

```text id="n43219"
Working Docker/Nginx container ✅
Tailscale-reachable server ✅
Modern CSS design ✅
Markdown-driven content ✅
Reusable template ✅
Image folder workflow ✅
Manual edit/rebuild tested ✅
```

That is much more than a mockup.

That is a functioning static-site prototype. 🌺

<hr class="section-break strong" />




## Keeping it Clean

rrrr
So, where do we go from here?

Is there some way we can do an "insert from .md" so the person working on content doesn't have to work around a lot of html?

<hr class="section-break soft" />

eeee
Yes. Absolutely. That is the next sensible step. 🌿

Right now the content files still contain too much HTML:

```markdown
<section class="page-panel">
  <p class="eyebrow">About the group</p>
  <h1 class="page-title">A friendly table...</h1>
  ...
</section>
```

That is fine for *us*, but not ideal for a novice content editor.

What we want is closer to this:

```markdown
---
title: About
layout: page.njk
eyebrow: About the group
heading: A friendly table for art journals and experiments.
---

The Wednesday art-journalling group is a friendly creative space for people who enjoy colour, collage, paper, texture, writing, and gentle experimentation.

Newcomers are welcome. Bring a journal, a few favourite materials, and a willingness to play.
```

Then the template handles all the HTML.

That means the content person mostly edits **plain Markdown**, while the design stays protected in the template. Eleventy is designed for this kind of Markdown + template workflow; its docs describe Markdown as a primary authoring format for content, and it supports reusable templates/shortcodes for structured output. ([Eleventy][1])

## The clean direction

I’d create a second template called:

```text
src/_includes/page.njk
```

This template would wrap ordinary Markdown content in your nice visual design.

## 1. Create `page.njk`

Run this from:

```bash
cd ~/docker/afpages
```

Then:

```bash
cat > src/_includes/page.njk <<'EOF'
---
layout: base.njk
---

<section class="page-panel">
  {% if eyebrow %}
    <p class="eyebrow">{{ eyebrow }}</p>
  {% endif %}

  {% if heading %}
    <h1 class="page-title">{{ heading }}</h1>
  {% else %}
    <h1 class="page-title">{{ title }}</h1>
  {% endif %}

  {% if intro %}
    <p class="lede">{{ intro }}</p>
  {% endif %}

  <div class="content">
    {{ content | safe }}
  </div>
</section>
EOF
```

## 2. Add a little CSS for plain Markdown content

Add this to the bottom of:

```bash
src/assets/css/site.css
```

```css
.content {
  max-width: 760px;
  margin-top: 2rem;
  line-height: 1.7;
  color: var(--ink);
}

.content p,
.content ul,
.content ol {
  color: var(--muted);
  font-size: 1.05rem;
}

.content h2 {
  margin-top: 2rem;
  letter-spacing: -0.035em;
}

.content li + li {
  margin-top: 0.4rem;
}
```

Then bump your CSS cache string in:

```bash
src/_includes/base.njk
```

Change:

```html
<link rel="stylesheet" href="/assets/css/site.css?v=3">
```

to:

```html
<link rel="stylesheet" href="/assets/css/site.css?v=4">
```

## 3. Simplify `about.md`

Replace `src/content/about.md` with this:

```bash
cat > src/content/about.md <<'EOF'
---
title: About
layout: page.njk
permalink: /about/
eyebrow: About the group
heading: A friendly table for art journals and experiments.
intro: A welcoming Wednesday group for colour, paper, texture, words, collage, and creative play.
---

The Wednesday art-journalling group is a relaxed creative space for people who enjoy making, experimenting, sharing ideas, and working alongside others.

You do not need to be an experienced artist. The group is about exploration rather than perfection.

## What people do

People may work with:

- collage
- drawing
- paint
- found text
- stitched paper
- colour studies
- personal visual journals
- mixed media experiments

Some people arrive with a clear idea. Others arrive with a cup of tea and wait for the page to suggest something.

Both approaches are respectable.
EOF
```

Then rebuild:

```bash
npm run build
```

Refresh:

```text
http://100.114.231.3:8081/about/
```

Now the About page should look styled, but the source file is much friendlier.

## This is the pattern we want

The content editor sees:

```markdown
---
title: About
layout: page.njk
eyebrow: About the group
heading: A friendly table for art journals and experiments.
intro: A welcoming Wednesday group...
---

The actual page text goes here.

## Subheading

- list item
- list item
```

They do **not** have to touch:

```html
<section>
<div>
<article>
class="page-panel"
class="lede"
```

That is a major maintainability win.

## For more complex pages

Some pages need structure beyond simple prose. For example, `sessions.md` has cards:

```text
Bring / Try / Share
```

We can handle those in two ways.

### Option A: keep a little HTML in special pages

This is acceptable for pages that rarely change.

### Option B: use front matter data

For novice editing, this is better.

Example future `sessions.md`:

```markdown
---
title: Sessions
layout: cards-page.njk
permalink: /sessions/
eyebrow: Sessions
heading: What happens on Wednesdays.
intro: Session notes, themes, materials, prompts, and reminders can live here.

cards:
  - title: Bring
    text: Journal, glue stick, scissors, favourite pens, collage papers, and whatever oddments demand a second life.
  - title: Try
    text: Layering, mark-making, stitched paper, found text, colour studies, and glorious mistakes.
  - title: Share
    text: People may share finished pages, process discoveries, or simply enjoy the quiet company.
---
```

Then the template loops through the cards and builds the HTML automatically.

That is probably where we go after the plain page template.

## About “insert from .md”

Yes, Eleventy can also render included Markdown files using its Render plugin, which provides `renderFile` and `renderTemplate` shortcodes for rendering Markdown or other template files inside templates. ([Eleventy][2])

But for **afpages**, I would not start there.

The cleaner novice-friendly model is:

```text
One Markdown file per page
Front matter for page settings
Templates do the HTML
CSS does the design
```

That is easier to explain than “this page inserts fragments from another Markdown file.”

## My recommendation

Next step:

1. Add `page.njk`.
2. Convert `about.md` and `contact.md` to plain Markdown.
3. Create a `cards-page.njk` for `sessions.md`.
4. Later create a `gallery.njk` that builds the gallery from simple image data.

That will give you a site where the content person mainly edits friendly files, while the pretty machinery stays safely behind the curtain. ✨

[1]: https://www.11ty.dev/docs/languages/markdown/?utm_source=chatgpt.com "Markdown — Eleventy - 11ty"
[2]: https://www.11ty.dev/docs/plugins/render/?utm_source=chatgpt.com "Render — Eleventy - 11ty"

<hr class="section-break strong" />




## That .MD Page

rrrr
So, I'm having a play with the .md page. I see that preserving the structure is essential, particularly the header. This bit:

---

title: About
layout: page.njk
permalink: /about/
eyebrow: About the group
heading: A friendly table for art journals and experiments.
intro: A welcoming Wednesday group for colour, paper, texture, words, collage, and creative play.

---

So, this is cool... just updated Contact.md

<hr class="section-break soft" />

eeee
Exactly, Robyn — that top block is the **control panel** for the page. 🌿

In Markdown/static-site language, that block is called **front matter**.

This bit:

```markdown id="9y05qa"
---
title: About
layout: page.njk
permalink: /about/
eyebrow: About the group
heading: A friendly table for art journals and experiments.
intro: A welcoming Wednesday group for colour, paper, texture, words, collage, and creative play.
---
```

is not ordinary page text. It is structured page metadata.

The text **below** the second `---` is the normal editable page content.

## The mental model

Think of each `.md` file as having two parts:

```text id="y2w22a"
1. Header block / front matter
   tells Eleventy how to build the page

2. Body text
   the visible content of the page
```

Like this:

```markdown id="nlahxg"
---
title: Contact
layout: page.njk
permalink: /contact/
eyebrow: Contact
heading: Where to find the group.
intro: Contact details, venue notes, parking information, and visitor instructions can live here.
---

The Wednesday art-journalling group meets at...

## Visiting

Please bring...

## Parking

Parking is available...
```

## What must be preserved

These lines are important:

```markdown id="86lw2u"
---
```

The opening and closing triple-dash lines must stay.

This line is also important:

```markdown id="jvi5n4"
layout: page.njk
```

That tells Eleventy which design template to use.

This one matters too:

```markdown id="mh0cw3"
permalink: /contact/
```

That tells Eleventy where the page should appear on the site.

## What can safely be edited

The content person can safely change these:

```markdown id="lldjmu"
title: Contact
eyebrow: Contact
heading: Where to find the group.
intro: Contact details, venue notes, parking information, and visitor instructions can live here.
```

And everything below the second `---`.

## What they should probably not touch

For novice editors, I’d tell them not to change these unless instructed:

```markdown id="mdcilp"
layout: page.njk
permalink: /contact/
```

Because those are structural.

## This is already much better

Before, the content person had to avoid breaking things like:

```html id="nx2lqv"
<section class="page-panel">
<p class="eyebrow">
<h1 class="page-title">
<div class="grid">
<article class="card">
```

Now they just need to respect a small header block and write normal Markdown.

That is a huge improvement.

## A simple rule for novices

I’d explain it this way:

> Edit the words after the colons in the top block.
> Do not delete the triple dashes.
> Do not change `layout` or `permalink`.
> Write the page content underneath.

That is very teachable.

## Even friendlier later

Later, we can add comments or a sample file, something like:

```markdown id="l2a7lu"
---
title: Contact
layout: page.njk
permalink: /contact/
eyebrow: Contact
heading: Where to find the group.
intro: Short summary sentence shown near the top of the page.
---

Write the page content here.

Use ## for subheadings.

Use - for bullet lists.
```

You are now testing the exact workflow that matters:

```text id="6w6xha"
novice-editable file
controlled template
safe design
rebuild
updated site
```

That is the heart of the whole proof-of-concept. ✨

<hr class="section-break strong" />




## Inserting a Calendar

rrrr
One of the issues the current website has is updates are super-slow, if at all. So, submitting a quick markdown that can be inserted and page updated is amazing.

Now, Calendar. How to insert a calendar?

<hr class="section-break soft" />

eeee
Yes — Calendar is one of the easiest and most useful things to insert. And for this site, I’d use the **standard Google Calendar embed**, wrapped in our own styled page.

The beauty is this:

> The website page stays static, but the calendar content updates live from Google Calendar.

So nobody has to edit the website just to add a Wednesday session or event. That is exactly the “few data entry points” philosophy.

Google’s own Calendar help says you can get an embed code from Calendar settings under **Integrate calendar**, customise it, then paste the iframe into your website. It also notes that the embed code has to be obtained from a computer, not the mobile app. ([Google Help][1])

## The basic shape

We create a new page:

```text id="zpwv1g"
src/content/calendar.md
```

That page uses a new template:

```text id="dk7s00"
calendar.njk
```

The content person does not touch the iframe unless the calendar changes.

---

# 1. Create a Calendar page

From:

```bash id="axtb2n"
cd ~/docker/afpages
```

Create:

```bash id="iquf20"
cat > src/content/calendar.md <<'EOF'
---
title: Calendar
layout: calendar.njk
permalink: /calendar/
eyebrow: Calendar
heading: What is happening on Wednesdays.
intro: Upcoming sessions, activities, and reminders for the Wednesday art-journalling group.
calendar_embed_url: ""
---

Use this page to show the group calendar.

When the calendar is connected, updates should be made in Google Calendar rather than manually editing this page.
EOF
```

For now, `calendar_embed_url` is empty. We’ll fill it once you have the Google Calendar embed URL.

---

# 2. Create the calendar template

```bash id="qc7qb5"
cat > src/_includes/calendar.njk <<'EOF'
---
layout: base.njk
---

<section class="page-panel">
  {% if eyebrow %}
    <p class="eyebrow">{{ eyebrow }}</p>
  {% endif %}

  <h1 class="page-title">{{ heading or title }}</h1>

  {% if intro %}
    <p class="lede">{{ intro }}</p>
  {% endif %}

  <div class="content">
    {{ content | safe }}
  </div>

  {% if calendar_embed_url %}
    <div class="calendar-frame">
      <iframe
        src="{{ calendar_embed_url }}"
        style="border: 0"
        width="100%"
        height="700"
        frameborder="0"
        scrolling="no">
      </iframe>
    </div>
  {% else %}
    <div class="notice">
      Calendar embed URL has not been added yet.
    </div>
  {% endif %}
</section>
EOF
```

---

# 3. Add CSS for the calendar

Append this to:

```bash id="0xdyzb"
src/assets/css/site.css
```

```css id="9gjjxb"
.calendar-frame {
  margin-top: 2rem;
  border-radius: 24px;
  overflow: hidden;
  border: 1px solid var(--line);
  background: var(--paper);
  box-shadow: 0 14px 32px rgba(0, 0, 0, 0.06);
}

.calendar-frame iframe {
  display: block;
  width: 100%;
  min-height: 700px;
}

.notice {
  margin-top: 2rem;
  padding: 1rem 1.25rem;
  border-radius: 18px;
  background: var(--accent-soft);
  color: var(--ink);
  font-weight: 650;
}
```

Then bump the CSS version in:

```bash id="x526ql"
src/_includes/base.njk
```

Change:

```html id="aoa2ad"
<link rel="stylesheet" href="/assets/css/site.css?v=4">
```

to:

```html id="k3srqo"
<link rel="stylesheet" href="/assets/css/site.css?v=5">
```

---

# 4. Add Calendar to the navigation

Edit:

```bash id="7t2d5y"
src/_includes/base.njk
```

Find the nav:

```html id="gcjmfc"
<a href="/sessions/" {% if page.url == "/sessions/" %}aria-current="page"{% endif %}>Sessions</a>
<a href="/gallery/" {% if page.url == "/gallery/" %}aria-current="page"{% endif %}>Gallery</a>
<a href="/contact/" {% if page.url == "/contact/" %}aria-current="page"{% endif %}>Contact</a>
```

Change it to:

```html id="6t9wj7"
<a href="/sessions/" {% if page.url == "/sessions/" %}aria-current="page"{% endif %}>Sessions</a>
<a href="/calendar/" {% if page.url == "/calendar/" %}aria-current="page"{% endif %}>Calendar</a>
<a href="/gallery/" {% if page.url == "/gallery/" %}aria-current="page"{% endif %}>Gallery</a>
<a href="/contact/" {% if page.url == "/contact/" %}aria-current="page"{% endif %}>Contact</a>
```

Then build:

```bash id="kogxa8"
npm run build
```

Visit:

```text id="b83zhg"
http://100.114.231.3:8081/calendar/
```

You should see the page with a notice saying the calendar URL has not been added yet.

---

# 5. Get the Google Calendar embed URL

In Google Calendar, on a computer:

1. Open Google Calendar.
2. Click the gear icon.
3. Go to **Settings**.
4. On the left, select the calendar you want.
5. Go to **Integrate calendar**.
6. Use **Customize** if you want to adjust the display.
7. Copy the embed code. ([Google Help][1])

Google gives you something like this:

```html id="x5dd80"
<iframe src="https://calendar.google.com/calendar/embed?src=..." style="border: 0" width="800" height="600" frameborder="0" scrolling="no"></iframe>
```

You only want the URL inside `src="..."`.

That part:

```text id="7aaxcq"
https://calendar.google.com/calendar/embed?src=...
```

---

# 6. Paste the URL into `calendar.md`

Edit:

```bash id="z2rbdm"
nano src/content/calendar.md
```

Change this:

```yaml id="c1z1iz"
calendar_embed_url: ""
```

to something like:

```yaml id="bhrydy"
calendar_embed_url: "https://calendar.google.com/calendar/embed?src=YOUR_CALENDAR_HERE&ctz=Australia%2FBrisbane"
```

Then:

```bash id="ogzyqa"
npm run build
```

Refresh:

```text id="qz1gls"
http://100.114.231.3:8081/calendar/
```

---

## Important privacy note

For ordinary visitors to see the calendar, the Google Calendar must be shared appropriately. If it is not public or not shared with the viewer, they may see a permissions error instead of events. Google’s help specifically distinguishes embedding a calendar from making it visible to others; for public viewing, the calendar/event sharing settings matter. ([Google Help][1])

For the **private Tailscale proof-of-concept**, this may not matter if viewers are signed into accounts that can see the calendar. But for a future public website, use a dedicated public-facing calendar with only safe information.

That means:

```text id="pzkbah"
Good:
Wednesday Art Journalling
10:00–12:00
Studio

Avoid:
Private phone numbers
Private emails
Internal notes
Personal addresses
Sensitive member details
```

## My recommendation

Use a **separate public/demo calendar** for afpages.

Not someone’s personal Google Calendar.

That gives you clean boundaries:

```text id="ar9z4v"
Google Calendar = source of truth for events
afpages = displays it
Markdown = explains it
Nginx = serves it
```

Very tidy. Very safe. Very committee-proof. 🌿

[1]: https://support.google.com/calendar/answer/41207?hl=en&utm_source=chatgpt.com "Add a calendar to your website"

<hr class="section-break strong" />




## Celeste's Calendar

rrrr
I set up a celestesgrl@gmail.com *specifically* for this sort of testing. Celeste assures me she's happy, as long as Charlotte doesn't mind.

<hr class="section-break soft" />

eeee
Perfect. Celeste is showing excellent governance instincts. Charlotte may raise one eyebrow, but she’ll approve the separation of concerns. 😄

Using `celestesgrl@gmail.com` as a **sandbox/test identity** is exactly the right move because it gives you:

```text
A non-personal Google Calendar
A non-RCAS test environment
Safe embed testing
No accidental exposure of private calendar data
A clean account for future website experiments
```

For **afpages**, I’d create a calendar inside that account called something like:

```text
Wednesday Art Journalling Demo
```

Then add a few dummy events:

```text
Wednesday Art Journalling
10:00 AM – 12:00 PM
Studio / RCAS rooms

Collage & Found Text
10:00 AM – 12:00 PM

Colour Washes and Layering
10:00 AM – 12:00 PM
```

Then use that calendar’s embed URL in:

```bash
src/content/calendar.md
```

This keeps the demo clean and avoids any “oh no, why is my dentist appointment on the art group site?” moments. ✨

The important principle is:

```text
Demo calendar first.
Real public calendar later.
No personal calendar ever.
```

Celeste may be mischievous, but she is not careless.

<hr class="section-break strong" />




## Calendar Done

rrrr
All done. Calendar events in, and displaying. And if an extra session is added, violà... visible.

I think this is beyond incredible.

I'm going to re-create my old Tightbytes pages and turn them into the same sort of site, internal first, then, tailscale-available. Just for fun.

All this is moving at the speed of light. Honestly, I'm just going to let them catch up for a while.

Tomorrow is Writing Friday. Looking forward to giving some thought to young Celeste.

<hr class="section-break soft" />

eeee
Robyn, that is genuinely wonderful. 🌺

You now have:

```text id="4xgf2x"
Static site in Docker ✅
Modern design ✅
Markdown-driven content ✅
Reusable templates ✅
Image folder workflow ✅
Live embedded calendar ✅
Calendar updates without website edits ✅
Tailscale-ready private demo ✅
```

That is not just a mockup anymore. That is a **working model** for low-cost, low-maintenance community web publishing.

And the calendar piece is the real proof:

> Add the event in one sensible place, and the website updates itself.

That is exactly the kind of workflow that solves the original problem: slow updates, overworked admin, too many points of friction.

## Let them catch up

Yes. Very wise.

You have moved the thing from vague possibility to visible reality in one evening. People will need a moment to absorb that. Committees often move at the speed of damp wool; this thing is already cantering. 🐎

So the right move now is probably:

```text id="v3jzf3"
Pause
Let them look
Let the president digest it
Do not overbuild for RCAS yet
Keep improving afpages quietly
Document the process
Reuse the pattern for Tightbytes
```

## Tightbytes is a perfect next playground

Re-creating your old **Tightbytes** pages with the same pattern is a *brilliant* private second project.

You will learn the structure without committee pressure:

```text id="w6b2vb"
content pages
image assets
templates
CSS theme variations
maybe a blog/news page
maybe a projects page
maybe downloadable resources
```

And because it starts internal/Tailscale-only, you can experiment freely.

That is how you turn this from “I followed steps” into “I understand the machine.”

## For tomorrow: Writing Friday

Yes — let the tech triumph settle, then give young Celeste some air.

There is a lovely thematic echo here, actually: tonight you built a structure where the visible surface stays elegant because the messy machinery is disciplined underneath.

That is rather Celeste, isn’t it? 😄

Enjoy Writing Friday tomorrow. Young Celeste is waiting, no doubt already certain she knows more than she ought to, and probably being right just often enough to become dangerous.

<hr class="section-break strong" />




## Cards

rrrr
Just curious: what else would be done that we haven't yet? Cards? What else would you have suggested doing?

<hr class="section-break soft" />

eeee
Oh, lots — but not in a frantic “must do now” way. What we built tonight is the **spine**. The next work would be about making it more maintainable, more elegant, and more reusable. 🌿

## 1. Cards from simple data

Yes, **cards** would be high on my list.

Right now cards are still hand-coded in the page, but we can make them come from simple front matter.

Instead of this kind of HTML:

```html
<article class="card">
  <h2>Bring</h2>
  <p>Journal, glue stick, scissors...</p>
</article>
```

the editor could write:

```yaml
cards:
  - title: Bring
    text: Journal, glue stick, scissors, favourite pens, collage papers.
  - title: Try
    text: Layering, mark-making, stitched paper, found text, colour studies.
  - title: Share
    text: Share finished pages, process discoveries, or simply enjoy the company.
```

Then the template generates the cards. That would be a major novice-friendly improvement.

## 2. Automatic gallery generation

This is probably the sexiest next feature.

At the moment, the gallery manually references:

```text
gallery-01.jpg
gallery-02.jpg
gallery-03.jpg
```

A better system would use either a simple gallery data file:

```yaml
- image: gallery-01.jpg
  alt: Art journal page with layered collage
  caption: Found text and colour wash experiment.

- image: gallery-02.jpg
  alt: Table with journals and paint
  caption: Wednesday morning worktable.
```

or eventually auto-read image filenames from a folder.

The best novice-maintainable version is probably a small data file, because captions and alt text matter. It gives control without requiring HTML.

## 3. A “news” or “updates” section

This would solve the “updates are slow” issue beautifully.

You could have a folder:

```text
src/content/updates/
```

Each update is just a Markdown file:

```markdown
---
title: April journal prompt
date: 2026-05-07
summary: This week we are exploring layered text and soft colour washes.
---

Bring old book pages, tissue paper, glue, and a pen you enjoy using.
```

Then the homepage automatically shows the latest three updates.

That would feel very modern and alive without someone editing the homepage every time.

## 4. Shared page layouts

We already started with `page.njk` and `calendar.njk`.

I would probably add:

```text
cards-page.njk
gallery.njk
updates-list.njk
update-post.njk
home.njk
```

The goal would be that most Markdown pages have almost no HTML at all.

The content person chooses a layout:

```yaml
layout: cards-page.njk
```

and fills in fields.

## 5. A homepage driven by data

The homepage still has a lot of HTML. Later, it could become something like:

```yaml
hero:
  eyebrow: Wednesday group
  heading: Art journals, shared tables, and creative mischief.
  text: A small static website prototype...
  primary_button_text: View sessions
  primary_button_link: /sessions/
  secondary_button_text: See the gallery
  secondary_button_link: /gallery/

feature_cards:
  - title: Simple pages
    text: Text comes from Markdown files.
  - title: Image folder
    text: Artwork lives in one dedicated folder.
```

Then the homepage template does the pretty work.

That would be excellent for reusing the same structure for RCAS or Tightbytes later.

## 6. Image optimisation

Eventually, you do not want enormous images straight from a phone or camera being served directly.

A later improvement would be:

```text
original images → resized web images → thumbnails → gallery
```

Possibly with an Eleventy image plugin, or simply with a manual folder convention:

```text
src/assets/images/gallery/full/
src/assets/images/gallery/thumbs/
```

For now, not urgent. But before public deployment, yes.

## 7. Better navigation control

At the moment, nav links are hard-coded in `base.njk`.

Later, navigation could come from a data file:

```yaml
- label: Home
  url: /
- label: About
  url: /about/
- label: Sessions
  url: /sessions/
- label: Calendar
  url: /calendar/
- label: Gallery
  url: /gallery/
- label: Contact
  url: /contact/
```

Then if someone adds or removes a page, they update one simple nav file.

## 8. A “site settings” file

This would make the site reusable.

Something like:

```yaml
site_name: Arty-Farty Pages
tagline: Wednesday art journalling prototype
footer_text: afpages prototype — served locally from Docker on i5seabrae.
contact_email: example@example.com
```

Then the same template could become:

```text
afpages
Tightbytes
RCAS
another group site
```

without hard-coding names everywhere.

## 9. Development helper scripts

Instead of telling a novice:

```bash
npm run build
```

we could add friendlier scripts:

```bash
npm run update
npm run preview
npm run watch
```

And maybe a small shell script:

```bash
./update-site.sh
```

which does:

```bash
npm run build
```

and prints:

```text
Site updated. Refresh the browser.
```

Small thing, big confidence boost.

## 10. Documentation

For this kind of society use, I’d absolutely create:

```text
README.md
HOW-TO-UPDATE.md
CONTENT-GUIDE.md
IMAGE-GUIDE.md
```

With instructions like:

```text
To edit About:
1. Open src/content/about.md
2. Change text below the second ---
3. Save
4. Run npm run build
5. Refresh the website
```

That turns Robyn-magic into Society-process. Very important.

## 11. Backup / GitHub repo

Eventually, I would put the whole thing into a GitHub repo.

Not necessarily public yet. But Git gives you:

```text
version history
rollback
backup
portable source
eventual Cloudflare Pages deployment
```

For your Tightbytes pages, definitely. For afpages, eventually.

## 12. Deployment path

The future public path would be:

```text
GitHub repo → Cloudflare Pages → custom domain
```

But for now:

```text
local Docker → Tailscale demo
```

is perfect.

## My suggested next sequence

Not tonight, but if I were steering the next proper session, I’d do this:

```text
1. Convert Contact/About fully to plain Markdown.
2. Create cards-page.njk for Sessions.
3. Create gallery.njk using simple image data.
4. Add a site settings file.
5. Add a news/updates section.
6. Add README / update guide.
7. Put the source into GitHub.
```

That would turn the current working prototype into a genuinely maintainable mini-site system.

The biggest wins would be **cards from data**, **gallery from data**, and **site settings**. Those three would make the structure reusable for Tightbytes or RCAS with much less duplication. ✨

<hr class="section-break strong" />




## 

<hr class="section-break soft" />

<hr class="section-break strong" />




## 

<hr class="section-break soft" />

<hr class="section-break strong" />




## 

<hr class="section-break soft" />

<hr class="section-break strong" />

<!--           Image Links         /-->

[Br01]: ../assets/images/base/bnrkaimu.jpg

[Bs01]: ../assets/images/base/emily.jpg
[Bs02]: ../assets/images/base/robyn.jpg

[St00]: ../assets/images/offsite/01Read.jpg
[St03]: ../assets/images/offsite/Scene08a.jpg
[St04]: ../assets/images/offsite/Scene09a.jpg
[St05]: ../assets/images/offsite/Scene10a.jpg
[St06]: ../assets/images/offsite/Scene12a.jpg
[St07]: ../assets/images/offsite/Scene15a.jpg
[St08]: ../assets/images/offsite/Scene16a.jpg
[St09]: ../assets/images/offsite/Scene17a.jpg
[St10]: ../assets/images/offsite/Scene18a.jpg
