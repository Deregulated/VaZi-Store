# VaZi - Online Apparel Store Website

**A modern, single-page website framework for VaZi clothing company**

---

## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Problem-Solving Approach](#problem-solving-approach)
- [File Structure](#file-structure)
- [Getting Started](#getting-started)
- [Collaborator Guide](#collaborator-guide)
- [Customization Guide](#customization-guide)
- [Technical Documentation](#technical-documentation)

---

## 🎯 Project Overview

VaZi is a single-page web application showcasing an online clothing store. The site features:
- **Smooth scroll navigation** between sections (no page reloads)
- **Leadership profiles** for 1 CEO + 6 Directors (opens in new windows)
- **Responsive design** optimized for all devices
- **Modular architecture** for easy collaboration
- **Consistent theming** across all pages

### Design Philosophy
The website uses an **Editorial Minimalism meets Modern Brutalism** aesthetic:
- High-contrast typography pairing
- Monochromatic color scheme with electric pink accent
- Asymmetric layouts with generous whitespace
- Subtle animations and micro-interactions

---

## 🧠 Problem-Solving Approach

### Challenge 1: Single-Page Navigation Without Page Jumps

**Problem:** How do we navigate between sections on one page smoothly?

**Logic:**
```
Traditional links → <a href="page.html"> → Loads new page (BAD)
Anchor links → <a href="#section"> → Instant jump (JARRING)
Solution → Anchor links + smooth scroll behavior → Animated scroll (PERFECT)
```

**Implementation:**
1. Each section has unique ID: `<section id="home">`, `<section id="about">`, etc.
2. Navigation links target IDs: `<a href="#home">`
3. CSS enables smooth scrolling: `scroll-behavior: smooth`
4. JavaScript prevents default behavior and adds custom smooth scroll
5. Active section highlighting on scroll

### Challenge 2: Profile Separation for Collaboration

**Problem:** How do we let others edit profiles without breaking the main site?

**Logic:**
```
IF profiles on same page
  THEN risk of editing main code
  RESULT: High chance of errors

IF profiles in separate files
  THEN isolated from main logic
  RESULT: Safe collaboration
```

**Implementation:**
1. Created `/profiles/` directory
2. Each profile is standalone HTML file
3. Links use `target="_blank"` to open new windows
4. All profiles import shared `styles.css` for consistency

### Challenge 3: Template System for Non-Coders

**Problem:** How can non-developers fill in profile content easily?

**Logic:**
```
Complex code → Intimidating → Mistakes
Clear placeholders → Easy to find → Success
Extensive comments → Guidance → Confidence
```

**Implementation:**
1. HTML comments mark all insertion points: `<!-- INSERT NAME HERE -->`
2. Optional sections commented out (uncomment to use)
3. Step-by-step instructions in file
4. Examples provided for each section
5. Separate templates for CEO (enhanced) and Directors (uniform)

### Challenge 4: Theme Consistency & Maintainability

**Problem:** How do we ensure all pages match visually and changes propagate easily?

**Logic:**
```
Hardcoded colors → Change 50 places → Error-prone
CSS variables → Change 1 place → Updates everywhere
```

**Implementation:**
1. All colors defined in `:root` CSS variables
2. Shared stylesheet linked from all pages
3. Reusable CSS classes for common patterns
4. Clear documentation of theme structure

---

## 📁 File Structure

```
vazi-website/
│
├── index.html              # Main landing page (all sections on one page)
├── styles.css              # Shared stylesheet (contains all styling + theme)
├── README.md               # This file - project documentation
│
└── profiles/               # Directory for team member profiles
    ├── ceo-profile.html    # CEO profile template (dark theme, enhanced)
    ├── director-1.html     # Director 1 profile template
    ├── director-2.html     # Director 2 profile template
    ├── director-3.html     # Director 3 profile template
    ├── director-4.html     # Director 4 profile template
    ├── director-5.html     # Director 5 profile template
    └── director-6.html     # Director 6 profile template
```

### Why This Structure?

**Separation of Concerns:**
- `index.html` → Main website logic (don't edit unless major changes)
- `styles.css` → All styling in one place (edit here for visual changes)
- `profiles/*.html` → Individual profiles (safe for collaborators to edit)

**Scalability:**
- Easy to add more profiles (just copy a template)
- Easy to add more sections to main page
- Easy to update theme globally

---

## 🚀 Getting Started

### Prerequisites
- Any modern web browser (Chrome, Firefox, Safari, Edge)
- Text editor (VS Code, Sublime Text, Atom, or even Notepad)
- Basic HTML knowledge (helpful but not required for profile editing)

### Local Setup

1. **Clone or Download the Repository**
   ```bash
   git clone <repository-url>
   cd vazi-website
   ```

2. **Open the Website**
   - Simply double-click `index.html` to open in your browser
   - Or use a local server (recommended for development):
     ```bash
     # Python 3
     python -m http.server 8000
     
     # Then open http://localhost:8000 in browser
     ```

3. **View Profile Templates**
   - Navigate to `profiles/` folder
   - Open any profile HTML file to see template structure

---

## 👥 Collaborator Guide

### For Profile Editors (Non-Technical)

#### Editing a Director Profile

1. **Open Your Assigned Profile File**
   - Example: If you're Director 3, open `profiles/director-3.html`
   - Use any text editor

2. **Find the Placeholder Sections**
   - Look for comments like `<!-- INSERT DIRECTOR FULL NAME HERE -->`
   - Replace the comment with your actual information

3. **Adding Your Image**
   ```html
   <!-- Find this section: -->
   <div class="profile-image">
       <!-- INSERT DIRECTOR IMAGE HERE -->
   </div>
   
   <!-- Replace with: -->
   <div class="profile-image">
       <img src="images/your-photo.jpg" alt="Your Name" 
            style="width: 100%; height: 100%; object-fit: cover;">
   </div>
   ```

4. **Adding Your Biography**
   ```html
   <!-- Find this section: -->
   <div class="profile-bio">
       <!-- INSERT DIRECTOR BIOGRAPHY HERE -->
   </div>
   
   <!-- Replace with: -->
   <div class="profile-bio">
       <p>Your first paragraph about your background...</p>
       <p>Your second paragraph about your role at VaZi...</p>
       <p>Your third paragraph about expertise...</p>
   </div>
   ```

5. **Adding Optional Sections**
   - Find commented-out sections like education, expertise, etc.
   - Remove `<!--` at the start and `-->` at the end to activate
   - Fill in your information

6. **Save and Test**
   - Save your file
   - Double-click to open in browser
   - Check that everything looks correct
   - Click "Back to VaZi Home" to ensure link works

#### Editing the CEO Profile

Same process as above, but:
- File is `profiles/ceo-profile.html`
- Has enhanced dark theme styling (you'll see the difference)
- May want more detailed content given leadership position

### For Developers

#### Making Structural Changes

**IMPORTANT: Only edit these files if making major changes:**

1. **`index.html`** - Main page structure
   - Modifying section order
   - Adding new sections
   - Changing navigation structure

2. **`styles.css`** - Global styling
   - Theme color changes (edit `:root` variables)
   - Layout modifications
   - Responsive design adjustments

#### Best Practices

```javascript
// DO: Edit CSS variables for theme changes
:root {
    --accent-color: #ff3366;  // Change this
}

// DON'T: Hardcode colors throughout CSS
.button {
    background: #ff3366;  // Avoid this
}
```

#### Adding New Profiles

1. Copy an existing profile template:
   ```bash
   cp profiles/director-1.html profiles/director-7.html
   ```

2. Update the main page (`index.html`) to add the card:
   ```html
   <div class="team-card director-card" 
        onclick="window.open('profiles/director-7.html', '_blank', 'noopener,noreferrer')">
       <!-- Card content -->
   </div>
   ```

---

## 🎨 Customization Guide

### Changing Theme Colors

All colors are defined in `styles.css` at the top:

```css
:root {
    --primary-color: #0a0a0a;      /* Deep black */
    --secondary-color: #f5f5f5;    /* Off-white */
    --accent-color: #ff3366;       /* Electric pink */
    --accent-hover: #e62958;       /* Darker pink */
    /* ... more colors */
}
```

**To change the color scheme:**
1. Edit only these variables
2. Save the file
3. Refresh browser - all pages update automatically

### Changing Fonts

Fonts are loaded from Google Fonts:

```html
<!-- In the <head> of HTML files -->
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display..." rel="stylesheet">
```

Current fonts:
- **Display (Headings):** Playfair Display
- **Body (Text):** Work Sans

To change:
1. Visit [Google Fonts](https://fonts.google.com)
2. Select new fonts
3. Replace the `<link>` tag
4. Update CSS variables:
   ```css
   :root {
       --font-display: 'YourNewFont', serif;
       --font-body: 'YourBodyFont', sans-serif;
   }
   ```

### Modifying Layouts

Key layout sections in `styles.css`:

- **Navigation:** `.main-nav`
- **Team Grid:** `.team-grid` (controls card arrangement)
- **Gallery Grid:** `.gallery-grid` (controls product layout)
- **Responsive Breakpoints:** `@media` queries at bottom of file

---

## 🔧 Technical Documentation

### Navigation System

**Smooth Scrolling Implementation:**

```javascript
// In index.html <script> section
document.querySelectorAll('.nav-link').forEach(link => {
    link.addEventListener('click', function(e) {
        e.preventDefault();  // Stop default jump
        const targetId = this.getAttribute('href');
        const targetSection = document.querySelector(targetId);
        
        targetSection.scrollIntoView({
            behavior: 'smooth',  // Animated scroll
            block: 'start'       // Align to top
        });
        
        history.pushState(null, null, targetId);  // Update URL
    });
});
```

**Active Link Highlighting:**

```javascript
// Highlights current section in navigation
window.addEventListener('scroll', () => {
    // Detects which section is in viewport
    // Adds 'active' class to corresponding nav link
});
```

### Profile Page Architecture

**Security Implementation:**

```html
<!-- Profile links use security attributes -->
<div onclick="window.open('profiles/ceo-profile.html', '_blank', 'noopener,noreferrer')">
```

- `target="_blank"` → Opens in new tab
- `noopener` → Prevents access to `window.opener` (security)
- `noreferrer` → Doesn't send referrer information

**Shared Styling:**

```html
<!-- All profiles link to shared stylesheet -->
<link rel="stylesheet" href="../styles.css">
```

Path uses `../` to go up one directory level from `/profiles/` to root.

### Animation System

**Scroll-Triggered Animations:**

```javascript
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('revealed');
        }
    });
});

// Apply to elements that should animate on scroll
document.querySelectorAll('.team-card, .gallery-item').forEach(el => {
    observer.observe(el);
});
```

Elements start hidden (`opacity: 0`) and become visible when scrolled into view.

### Responsive Design

**Breakpoints:**

- **Desktop:** 1024px and above
- **Tablet:** 768px - 1024px
- **Mobile:** Below 768px

**Key Responsive Features:**

```css
/* Example: Team grid adapts to screen size */
.team-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    /* Cards automatically wrap to new rows as needed */
}
```

---

## 🐛 Troubleshooting

### Common Issues

**1. Profile Link Opens But Shows Broken Styling**
- **Cause:** Incorrect path to `styles.css`
- **Fix:** Ensure profile HTML has `href="../styles.css"` (with `../`)

**2. Images Don't Show**
- **Cause:** Wrong image path
- **Fix:** Check path is relative to HTML file location
  ```html
  <!-- If image is in /profiles/images/ -->
  <img src="images/photo.jpg">
  
  <!-- If image is in root /images/ -->
  <img src="../images/photo.jpg">
  ```

**3. Smooth Scroll Not Working**
- **Cause:** JavaScript disabled or browser compatibility
- **Fix:** Check browser console for errors; ensure JavaScript is enabled

**4. Changes Not Showing**
- **Cause:** Browser cache
- **Fix:** Hard refresh (Ctrl+Shift+R on Windows, Cmd+Shift+R on Mac)

---

## 📝 Version Control

### Recommended Git Workflow

```bash
# 1. Create a feature branch for your profile
git checkout -b profile/director-3

# 2. Make your changes
# Edit profiles/director-3.html

# 3. Commit your changes
git add profiles/director-3.html
git commit -m "Add Director 3 profile information"

# 4. Push to remote
git push origin profile/director-3

# 5. Create pull request for review
```

### What to Commit

**DO commit:**
- Profile content changes
- New images (if small)
- Documentation updates

**DON'T commit:**
- Large images (use Git LFS or host externally)
- Operating system files (.DS_Store, Thumbs.db)
- Editor configurations (.vscode/, .idea/)

---

## 📞 Support

### For Questions or Help

1. **Check this README first** - Most answers are here
2. **Examine the code comments** - Each file has detailed instructions
3. **Ask the team** - Reach out to the technical lead
4. **Submit an issue** - Use repository issue tracker for bugs

### Useful Resources

- [HTML Basics](https://developer.mozilla.org/en-US/docs/Learn/HTML)
- [CSS Guide](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [Google Fonts](https://fonts.google.com)
- [Can I Use](https://caniuse.com) - Browser compatibility checker

---

## 📄 License

Copyright © 2026 VaZi. All rights reserved.

---

## 🎉 Credits

**Design & Development:**
- Framework architecture and problem-solving approach documented above
- Editorial Minimalism meets Modern Brutalism aesthetic
- Responsive, accessible, and maintainable codebase

**Built for:**
- VaZi Online Apparel Store
- Collaborative team environment
- Modern web standards

---

**Last Updated:** February 2026  
**Version:** 1.0.0  
**Maintainer:** Development Team
