# Publishing the prototype to WordPress at yorkshirecyclocross.com

## Overview of what needs porting

| Prototype file | WordPress equivalent |
|---|---|
| `css/styles.css` | Child theme stylesheet or Customizer → Additional CSS |
| `images/banner.png` | Theme file or Media Library |
| Nav HTML (copy-pasted across pages) | Appearance → Menus + theme header template |
| Footer HTML (copy-pasted across pages) | Theme footer template |
| Page content (between header/footer) | WordPress Pages |
| Mobile nav `<script>` | Enqueued theme script |

---

## Step 1 — Back up the existing site

Before touching anything:
- Export the database (Tools → Export, or via hosting control panel)
- Download a full file backup via FTP or hosting file manager
- Test you can restore it

---

## Step 2 — Create a child theme

A child theme lets you override the existing theme without losing changes when it updates.

In your hosting file manager or via FTP, create `/wp-content/themes/yorkshirecx-child/` with two files:

**`style.css`**
```css
/*
Theme Name: Yorkshire CX Child
Template:   [name of current parent theme — check wp-content/themes/]
*/
```

**`functions.php`**
```php
<?php
add_action('wp_enqueue_scripts', function() {
    wp_enqueue_style('parent-style', get_template_directory_uri() . '/style.css');
    wp_enqueue_style('child-style',  get_stylesheet_uri(), ['parent-style']);
    wp_enqueue_script('ycca-nav', get_stylesheet_directory_uri() . '/nav.js', [], null, true);
});
```

**`nav.js`** — paste the mobile nav toggle from the prototype:
```js
document.querySelector('.nav-toggle').addEventListener('click', function () {
    var nav = document.getElementById('main-nav');
    var open = nav.classList.toggle('open');
    this.setAttribute('aria-expanded', open ? 'true' : 'false');
});
```

Activate the child theme in Appearance → Themes.

---

## Step 3 — Add the CSS

Copy the full contents of `css/styles.css` from the prototype into the child theme's `style.css` (after the theme header comment block).

Alternatively, paste it into **Appearance → Customizer → Additional CSS** to test it without FTP — easier but harder to maintain long term.

---

## Step 4 — Upload the banner image

Upload `images/banner.png` to the child theme folder (same place as `style.css`) so it can be referenced as a theme asset, rather than through the Media Library. Reference it in CSS as:

```css
.site-banner { display: block; width: 100%; height: auto; }
```

---

## Step 5 — Create the header and footer templates

The nav and footer are shared across all pages. In the child theme, create:

- **`header.php`** — copy the `<header class="site-header">...</header>` block from any prototype page, wrapped in standard WordPress header boilerplate (`<!DOCTYPE html>`, `wp_head()` call, etc.)
- **`footer.php`** — copy the `<footer class="site-footer">...</footer>` block, with `wp_footer()` call before `</body>`

The nav links can be hardcoded initially or wired to **Appearance → Menus** using `wp_nav_menu()` — the latter is cleaner but requires a small PHP change.

The banner image line goes in `header.php`, after `</header>`:
```html
<img src="<?php echo get_stylesheet_directory_uri(); ?>/banner.png" alt="" class="site-banner" aria-hidden="true">
```

---

## Step 6 — Create WordPress pages and add content

For each of the 7 prototype pages, create a WordPress Page (Pages → Add New):

| Page title | Slug | Source file |
|---|---|---|
| Home | `/` | `index.html` |
| New to Cyclo-Cross? | `/new-to-cx` | `new-to-cx.html` |
| Your First Race | `/your-first-race` | `your-first-race.html` |
| Junior & Youth Riders | `/juniors` | `juniors.html` |
| Current Season | `/current-season` | `current-season.html` |
| Results Archive | `/results-archive` | `results-archive.html` |
| Rules & Regulations | `/rules` | `rules.html` |

In each page, switch the editor to **Code Editor** (⋮ menu → Code Editor) and paste the content between the `<div class="page-header">` and `</footer>` tags from the prototype HTML. The CSS classes (`section`, `info-grid`, `accordion`, `data-table`, etc.) will apply automatically from the stylesheet.

---

## Step 7 — Set up navigation

In **Appearance → Menus**, create a menu with the 7 pages and assign it to your theme's primary menu location.

The `.active` class on nav links is handled automatically by WordPress (it adds `current-menu-item` to the active page). Update the CSS to also target WordPress's class:

```css
.site-nav a:hover,
.site-nav a.active,
.site-nav .current-menu-item > a { color: white; background: rgba(255,255,255,0.1); }
```

---

## Step 8 — Set the homepage

Go to **Settings → Reading** and set:
- "Your homepage displays" → A static page
- Homepage: **Home**

---

## Step 9 — Test and go live

- Check every page at the live URL
- Check mobile nav toggle works
- Check the banner image loads
- Check all internal links resolve correctly
- Remove the prototype notice bar (the `.prototype-bar` div) from `header.php`

---

## Biggest risks

| Risk | Mitigation |
|---|---|
| Existing theme CSS conflicts with prototype CSS | Use more specific selectors or scope under a wrapper class |
| WordPress block editor strips HTML classes | Use Code Editor mode, or install Classic Editor plugin |
| Current theme header/footer is complex to replace | Simpler to add CSS only and recreate content in blocks |

---

## Fallback approach (no FTP access)

If FTP access or PHP editing isn't available:

1. Paste the full `css/styles.css` into **Appearance → Customizer → Additional CSS**
2. Recreate each page's content using WordPress blocks in the block editor, accepting that some layout details won't match exactly
3. Upload `banner.png` to the Media Library and add it as a full-width image block at the top of each page
