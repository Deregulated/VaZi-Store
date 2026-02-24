# VaZi Website - Architecture & Logic Flow

This document explains the technical architecture and problem-solving logic behind the VaZi website framework.

---

## 🏗️ System Architecture

### High-Level Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        USER BROWSER                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────────────────────────────────────────────┐    │
│  │              index.html (Main Page)                 │    │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐         │    │
│  │  │   Home   │  │  About   │  │ Gallery  │  etc... │    │
│  │  │ #home    │  │ #about   │  │ #gallery │         │    │
│  │  └────┬─────┘  └──────────┘  └──────────┘         │    │
│  │       │                                             │    │
│  │       │ Click on team member card                   │    │
│  │       ▼                                             │    │
│  │  [Opens New Window]                                 │    │
│  └────────────────────────────────────────────────────┘    │
│           │                                                  │
│           ▼                                                  │
│  ┌────────────────────────────────────────────────────┐    │
│  │         profiles/ceo-profile.html                   │    │
│  │              OR                                      │    │
│  │         profiles/director-X.html                    │    │
│  │                                                      │    │
│  │  (Imports same styles.css for consistency)          │    │
│  └────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### File Dependency Map

```
vazi-website/
│
├── index.html ──────────┐
│                        ├──> styles.css (shared styles)
├── profiles/            │
│   ├── ceo-profile.html ┘
│   ├── director-1.html ──┘
│   ├── director-2.html ──┘
│   └── ... (all import same CSS)
```

**Key Insight:** All pages share one stylesheet, ensuring visual consistency.

---

## 🔀 Navigation Logic Flow

### Problem: How does smooth scrolling work?

```
USER ACTION                 SYSTEM RESPONSE                 RESULT
───────────                 ───────────────                 ──────

User clicks                 JavaScript intercepts           Prevents
"About Us"     ────────>    click event         ────────>   page jump
   │                              │
   │                              ▼
   │                        Gets target: #about
   │                              │
   │                              ▼
   │                        Finds <section id="about">
   │                              │
   │                              ▼
   │                        Calls scrollIntoView()
   │                        with behavior: 'smooth'
   │                              │
   ▼                              ▼
Page smoothly scrolls      Updates URL hash    ────────>   User can
to About section           to #about                       bookmark
```

### Implementation Code Flow

```javascript
// STEP 1: Select all navigation links
document.querySelectorAll('.nav-link').forEach(link => {
    
    // STEP 2: Add click event listener to each
    link.addEventListener('click', function(e) {
        
        // STEP 3: Prevent default behavior (instant jump)
        e.preventDefault();
        
        // STEP 4: Get the target section ID from href
        const targetId = this.getAttribute('href'); // e.g., "#about"
        
        // STEP 5: Find the actual section element
        const targetSection = document.querySelector(targetId);
        
        // STEP 6: Scroll to that section smoothly
        if (targetSection) {
            targetSection.scrollIntoView({
                behavior: 'smooth',  // CSS animation
                block: 'start'       // Align to top
            });
            
            // STEP 7: Update URL for bookmarking
            history.pushState(null, null, targetId);
        }
    });
});
```

### Active Link Highlighting Logic

```
SCROLL EVENT               DETECTION LOGIC                ACTION
────────────               ───────────────                ──────

User scrolls    ───────>   For each section:             Remove 'active'
down page                  Calculate position            from all links
                                  │                              │
                                  ▼                              │
                           Is section in viewport?              │
                                  │                              │
                              ┌───┴───┐                          │
                              │       │                          │
                            YES      NO                          │
                              │       │                          │
                              ▼       └─> Continue               │
                           Get section ID                        │
                              │                                  │
                              ▼                                  │
                           Find matching nav link               │
                              │                                  │
                              └──────────────────────────────────┘
                                                                 │
                                                                 ▼
                                                          Add 'active' class
                                                          to matching link
```

---

## 🔗 Profile Link Architecture

### Problem: How do profiles open without affecting main page?

```
MAIN PAGE                  CLICK EVENT                   PROFILE PAGE
─────────                  ───────────                   ────────────

┌──────────────┐
│ Team Card    │
│              │           window.open()
│ [View Profile]  ─────>   ├─ URL: 'profiles/ceo-profile.html'
│              │           ├─ Target: '_blank' (new window)
└──────────────┘           ├─ Features: 'noopener,noreferrer'
      │                    │
      │                    ▼
      │           ┌─────────────────────┐
      │           │  Security Applied:  │
      │           │  - No access to     │
      │           │    parent window    │
      │           │  - No referrer      │
      │           │    information sent │
      │           └─────────────────────┘
      │                    │
      │                    ▼
      │           Profile opens in new tab
      │                    │
      ▼                    ▼
Main page           Profile page loads
stays active        with shared styles
```

### Code Implementation

```javascript
// On team card:
onclick="window.open('profiles/ceo-profile.html', '_blank', 'noopener,noreferrer')"

// Breakdown:
window.open(
    'profiles/ceo-profile.html',  // URL to open
    '_blank',                      // Open in new window/tab
    'noopener,noreferrer'          // Security features
)
```

**Why this approach?**
- ✅ Keeps main page active
- ✅ User can reference both pages
- ✅ Prevents accidental navigation away
- ✅ Secure (no cross-window access)

---

## 🎨 Theming System Logic

### Problem: How to maintain consistent styling across all pages?

```
CSS VARIABLES CONCEPT:
─────────────────────

Define Once                Use Everywhere              Change Once
───────────               ───────────────             ───────────

:root {                    .button {                   Update :root
  --primary: #0a0a0a; ──>    color: var(--primary); ──> ALL buttons
  --accent: #ff3366;  ──>  }                             update
}                                                        automatically
                           .heading {
                             color: var(--primary);
                           }
                           
                           .card {
                             border: var(--accent);
                           }
```

### Implementation Flow

```
┌─────────────────────────────────────────────────────────────┐
│                      styles.css                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  STEP 1: Define Variables                                   │
│  ──────────────────────                                     │
│  :root {                                                     │
│    --primary-color: #0a0a0a;      /* Black */               │
│    --accent-color: #ff3366;        /* Pink */               │
│  }                                                           │
│                                                              │
│  STEP 2: Create Reusable Classes                            │
│  ────────────────────────────────                           │
│  .button {                                                   │
│    background: var(--primary-color);  /* Uses variable */   │
│  }                                                           │
│                                                              │
│  .ceo-profile .button {                                     │
│    background: var(--accent-color);   /* Override for CEO */│
│  }                                                           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
         ┌────────────────────┴────────────────────┐
         │                                          │
         ▼                                          ▼
  ┌─────────────┐                          ┌──────────────┐
  │ index.html  │                          │ Profiles     │
  │             │                          │              │
  │ <link rel=  │                          │ <link rel=   │
  │  "styles.css">                         │  "../styles. │
  │             │                          │   css">      │
  └─────────────┘                          └──────────────┘
         │                                          │
         └────────────────┬─────────────────────────┘
                          ▼
                  Same styles applied
                  Variables work everywhere
```

### CEO vs Director Styling Differentiation

```
PROBLEM: How to make CEO profile distinctive but maintain consistency?

SOLUTION: CSS Specificity & Class Overrides

┌─────────────────────────────────────────────────────────┐
│  BASE STYLES (Applied to ALL profiles)                  │
├─────────────────────────────────────────────────────────┤
│  .profile-page {                                         │
│    background: var(--secondary-color);  /* Light */     │
│  }                                                       │
│                                                          │
│  .profile-main {                                         │
│    background: white;                                    │
│    color: black;                                         │
│  }                                                       │
└─────────────────────────────────────────────────────────┘
                          │
              ┌───────────┴──────────┐
              │                      │
              ▼                      ▼
    ┌──────────────────┐   ┌──────────────────┐
    │ Director Profiles│   │   CEO Profile    │
    │                  │   │                  │
    │ (Uses base only) │   │ OVERRIDE:        │
    │                  │   │                  │
    │ Result:          │   │ .ceo-profile     │
    │ - White bg       │   │   .profile-main {│
    │ - Black text     │   │    background:   │
    │                  │   │      gradient(); │
    │                  │   │    color: white; │
    │                  │   │  }               │
    └──────────────────┘   │                  │
                           │ Result:          │
                           │ - Dark gradient  │
                           │ - White text     │
                           │ - Pink accents   │
                           └──────────────────┘
```

---

## 📦 Template System Logic

### Problem: How to create fillable templates without code knowledge?

```
APPROACH: Progressive Disclosure with HTML Comments

STEP 1: Create structure with placeholders
────────────────────────────────────────────

<h1 class="profile-name">
    <!-- INSERT DIRECTOR FULL NAME HERE -->
</h1>

STEP 2: Provide examples in comments
─────────────────────────────────────

<div class="profile-bio">
    <!-- INSERT DIRECTOR BIOGRAPHY HERE -->
    <!-- Example structure:
    <p>First paragraph about background...</p>
    <p>Second paragraph about role...</p>
    -->
</div>

STEP 3: Use commented sections for optional content
────────────────────────────────────────────────────

<!-- EDUCATION SECTION -->
<!--
<div class="profile-section">
    <h3>Education</h3>
    <ul>
        <li>Degree, University Name, Year</li>
    </ul>
</div>
-->
```

### Collaborator Workflow

```
COLLABORATOR RECEIVES FILE
          │
          ▼
    Opens in text editor
          │
          ▼
    Searches for "INSERT"
          │
          ▼
    Finds: <!-- INSERT NAME HERE -->
          │
          ▼
    Replaces comment with actual name
          │
          ▼
    Saves file
          │
          ▼
    Tests in browser
          │
          ▼
    Name displays correctly ✓
```

---

## 🔍 Scroll Animation Logic

### Problem: How to reveal elements as user scrolls?

```
INTERSECTION OBSERVER API
─────────────────────────

┌─────────────────────────────────────────────────────────┐
│                    Browser Viewport                      │
│  ┌─────────────────────────────────────────────────┐   │
│  │                                                  │   │
│  │     Visible Area                                 │   │
│  │                                                  │   │
│  │  ┌──────────────┐                               │   │
│  │  │ Element A    │ ← INTERSECTING                │   │
│  │  │ (visible)    │   Trigger: Add 'revealed'     │   │
│  │  └──────────────┘   class                       │   │
│  │                                                  │   │
│  └─────────────────────────────────────────────────┘   │
│                                                          │
│  ┌──────────────┐                                       │
│  │ Element B    │ ← NOT INTERSECTING                    │
│  │ (below fold) │   Action: Wait                        │
│  └──────────────┘                                       │
└─────────────────────────────────────────────────────────┘

User scrolls down ↓

┌─────────────────────────────────────────────────────────┐
│                    Browser Viewport                      │
│  ┌─────────────────────────────────────────────────┐   │
│  │                                                  │   │
│  │  Element A (now above, still revealed)          │   │
│  │                                                  │   │
│  │  ┌──────────────┐                               │   │
│  │  │ Element B    │ ← NOW INTERSECTING            │   │
│  │  │ (visible)    │   Trigger: Add 'revealed'     │   │
│  │  └──────────────┘   class                       │   │
│  │                                                  │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Code Implementation

```javascript
// STEP 1: Define what "intersecting" means
const observerOptions = {
    threshold: 0.1,              // Element 10% visible
    rootMargin: '0px 0px -100px' // Trigger 100px before viewport
};

// STEP 2: Create the observer
const observer = new IntersectionObserver((entries) => {
    
    // STEP 3: Loop through all observed elements
    entries.forEach(entry => {
        
        // STEP 4: Check if element is in viewport
        if (entry.isIntersecting) {
            
            // STEP 5: Add 'revealed' class
            entry.target.classList.add('revealed');
            
            // Element CSS transitions from:
            // opacity: 0, transform: translateY(30px)
            // to:
            // opacity: 1, transform: translateY(0)
        }
    });
}, observerOptions);

// STEP 6: Tell observer which elements to watch
document.querySelectorAll('.team-card, .gallery-item').forEach(el => {
    observer.observe(el);
});
```

---

## 🔐 Security Considerations

### Profile Link Security

```
SECURITY RISK                      MITIGATION
─────────────                      ──────────

Window.opener attack    ───────>   rel="noopener"
(New window can                    Prevents access to
access parent window)              window.opener object

Referrer leakage       ───────>    rel="noreferrer"
(Profile page knows                Removes referrer
where it was opened from)          information

Combined:
onclick="window.open(..., '_blank', 'noopener,noreferrer')"
```

### File Path Security

```
RELATIVE PATHS (Secure)
───────────────────────

profiles/director-1.html:
<link rel="stylesheet" href="../styles.css">
                                 ↑
                                 Goes up one directory level
                                 Safe: Can't escape project folder

ABSOLUTE PATHS (Avoid)
──────────────────────

<link rel="stylesheet" href="/var/www/styles.css">
                                 ↑
                                 Exposes server structure
                                 Security risk
```

---

## 📱 Responsive Design Logic

### Grid System Behavior

```
DESKTOP (>1024px)
─────────────────
┌──────────────────────────────────────────────────────┐
│  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐    │
│  │ Card 1 │  │ Card 2 │  │ Card 3 │  │ Card 4 │    │
│  └────────┘  └────────┘  └────────┘  └────────┘    │
└──────────────────────────────────────────────────────┘

TABLET (768px - 1024px)
───────────────────────
┌──────────────────────────────────────┐
│  ┌────────┐  ┌────────┐             │
│  │ Card 1 │  │ Card 2 │             │
│  └────────┘  └────────┘             │
│  ┌────────┐  ┌────────┐             │
│  │ Card 3 │  │ Card 4 │             │
│  └────────┘  └────────┘             │
└──────────────────────────────────────┘

MOBILE (<768px)
───────────────
┌──────────────┐
│  ┌────────┐  │
│  │ Card 1 │  │
│  └────────┘  │
│  ┌────────┐  │
│  │ Card 2 │  │
│  └────────┘  │
│  ┌────────┐  │
│  │ Card 3 │  │
│  └────────┘  │
│  ┌────────┐  │
│  │ Card 4 │  │
│  └────────┘  │
└──────────────┘
```

### CSS Logic

```css
/* Base: Mobile-first approach */
.team-grid {
    display: grid;
    grid-template-columns: 1fr;  /* Single column */
    gap: 2rem;
}

/* Tablet: 2 columns when space allows */
@media (min-width: 768px) {
    .team-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

/* Desktop: Auto-fit with minimum size */
@media (min-width: 1024px) {
    .team-grid {
        grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
        /* Creates as many columns as fit, min 280px each */
    }
}
```

---

## 🔄 Data Flow Summary

```
┌─────────────────────────────────────────────────────────────┐
│                       USER INTERACTION                       │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              │               │               │
              ▼               ▼               ▼
        Click Nav      Click Profile     Scroll Page
          Link            Card
              │               │               │
              ▼               ▼               ▼
        Smooth Scroll   Open New Window   Reveal Elements
        to Section      with Profile      (Intersection
              │               │            Observer)
              ▼               ▼               │
        Update URL      Load Profile          ▼
        Hash            HTML File        Add 'revealed'
              │               │            Class
              ▼               ▼               │
        Highlight       Import              ▼
        Active Link     styles.css     Trigger CSS
              │               │          Transitions
              ▼               ▼               │
        Visual          Render with          ▼
        Feedback        Same Theme      Animated
                                         Appearance

                All paths lead to:
                Consistent User Experience
```

---

## 🎯 Design Decisions Summary

### Why Single-Page Architecture?
1. Faster perceived performance (no page reloads)
2. Smooth, app-like experience
3. Easier navigation for users
4. Better for showcasing products in gallery

### Why Separate Profile Files?
1. Isolation prevents breaking main site
2. Easier collaboration (each person works on own file)
3. Clearer responsibility boundaries
4. Version control shows who changed what

### Why CSS Variables?
1. One source of truth for colors
2. Easy theme customization
3. Reduced code duplication
4. Maintainable long-term

### Why Template Comments?
1. Non-coders can contribute
2. Self-documenting structure
3. Examples guide proper usage
4. Reduces support requests

---

## 📊 Performance Considerations

### Load Time Optimization

```
INITIAL LOAD
────────────
1. HTML (index.html)        → ~20KB
2. CSS (styles.css)         → ~15KB
3. Fonts (Google Fonts)     → ~30KB (cached)
4. JavaScript (inline)      → ~2KB
   ─────────────────────────
   Total: ~67KB (fast load)

PROFILE LOAD
────────────
1. HTML (profile.html)      → ~8KB
2. CSS (already cached)     → 0KB (reused)
3. Fonts (already cached)   → 0KB (reused)
   ─────────────────────────
   Total: ~8KB (very fast)
```

### Why This Is Fast
- Minimal external dependencies
- CSS reused across pages (browser caching)
- Fonts cached after first load
- No large frameworks (React, Vue, etc.)
- Optimized CSS (no unused styles)

---

This architecture provides a solid foundation for the VaZi website while maintaining flexibility for future enhancements.
