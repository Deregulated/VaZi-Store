# Contributing to VaZi Website

Thank you for contributing to the VaZi website project! This guide will help you understand how to contribute effectively while maintaining code quality and consistency.

---

## 🎯 Quick Start for New Contributors

### Before You Start
1. Read the [README.md](README.md) file thoroughly
2. Familiarize yourself with the project structure
3. Set up your local development environment
4. Understand which file you'll be editing

### Your First Contribution

**If you're filling in a profile:**
1. Navigate to the `profiles/` folder
2. Find your assigned file (e.g., `director-2.html`)
3. Follow the in-file instructions (look for `<!-- INSERT ... HERE -->` comments)
4. Save and test locally in your browser
5. Submit your changes

---

## 📋 Contribution Guidelines

### What You Can Edit

#### ✅ **SAFE TO EDIT** (Encouraged)
- **Your assigned profile file** in `/profiles/` folder
  - Add your name, image, biography
  - Uncomment and fill optional sections
  - Customize content sections

#### ⚠️ **EDIT WITH CAUTION** (Technical changes)
- **`styles.css`** - Only if making approved design changes
  - Theme color variables (`:root` section)
  - Adding new reusable classes
  - Fixing visual bugs

- **`index.html`** - Only if making approved structural changes
  - Adding new sections
  - Modifying main page layout
  - Updating company information

#### 🚫 **DO NOT EDIT** (Unless discussed with team)
- JavaScript functionality in `index.html`
- Core CSS layout classes
- File structure or naming conventions
- Other contributors' profile files

---

## 🔧 How to Make Changes

### Method 1: Direct File Edit (Beginners)

1. **Locate Your File**
   ```
   vazi-website/profiles/director-X.html
   ```

2. **Open in Text Editor**
   - Use VS Code, Sublime Text, or any text editor
   - Notepad works but isn't recommended

3. **Make Your Changes**
   - Find `<!-- INSERT ... HERE -->` comments
   - Replace or add content
   - Save the file

4. **Test Locally**
   - Double-click the HTML file to open in browser
   - Check all content displays correctly
   - Click "Back to VaZi Home" to ensure navigation works

5. **Submit Changes**
   - Follow git workflow (see below)
   - Or send file to technical lead for review

### Method 2: Git Workflow (Recommended)

```bash
# 1. Create a new branch
git checkout -b profile/your-name

# 2. Make your changes
# Edit your profile file

# 3. Stage your changes
git add profiles/director-X.html

# 4. Commit with descriptive message
git commit -m "Add Director X profile: [Your Name]"

# 5. Push to remote repository
git push origin profile/your-name

# 6. Create a Pull Request
# Go to repository website and click "New Pull Request"
```

---

## ✍️ Content Guidelines

### Profile Information

**Required Information:**
- Full name
- Professional headshot or portrait photo
- Current title/role
- 2-4 paragraph biography

**Optional Information:**
- Education
- Professional experience
- Areas of expertise
- Key achievements
- Professional philosophy
- Personal interests (appropriate for professional context)

### Writing Style

**DO:**
- Write in third person (e.g., "Jane leads the marketing team...")
- Keep tone professional yet personable
- Use active voice
- Proofread for spelling and grammar
- Keep paragraphs concise (3-5 sentences)

**DON'T:**
- Use first person (avoid "I lead the team...")
- Include confidential or sensitive information
- Use jargon without explanation
- Write overly long paragraphs
- Include unverified or exaggerated claims

### Image Guidelines

**Technical Requirements:**
- Format: JPG or PNG
- Minimum size: 1200px wide × 500px tall
- Maximum file size: 500KB (compress if larger)
- Aspect ratio: Approximately 2:1 to 5:3

**Content Requirements:**
- Professional photograph (headshot or portrait)
- Good lighting and image quality
- Neutral or branded background
- Business or business-casual attire
- Consistent style across all team members

**How to Add Your Image:**

```html
<!-- Option 1: Using img tag (Recommended) -->
<div class="profile-image">
    <img src="images/your-name.jpg" 
         alt="Your Full Name" 
         style="width: 100%; height: 100%; object-fit: cover;">
</div>

<!-- Option 2: Using background-image -->
<div class="profile-image" 
     style="background-image: url('images/your-name.jpg'); 
            background-size: cover; 
            background-position: center;">
</div>
```

---

## 🐛 Reporting Issues

Found a bug or problem? Here's how to report it:

### Before Reporting
1. Check if the issue has already been reported
2. Verify the issue occurs in multiple browsers
3. Clear your browser cache and test again

### Creating an Issue

**Include:**
- Clear, descriptive title
- Steps to reproduce the problem
- Expected behavior vs. actual behavior
- Browser and operating system
- Screenshots (if applicable)

**Example:**
```
Title: "Profile image not displaying on Safari"

Description:
My profile image shows correctly in Chrome but not in Safari 15.2.

Steps to reproduce:
1. Open profiles/director-3.html in Safari
2. Profile image area is blank

Expected: Image should display
Actual: Gray placeholder visible

Browser: Safari 15.2
OS: macOS Monterey 12.1

Screenshot: [attached]
```

---

## 💬 Communication

### Where to Ask Questions

1. **Technical Issues:** Open an issue on repository
2. **Content Questions:** Contact team lead
3. **Design Decisions:** Discuss in team meeting
4. **Urgent Problems:** Use designated team communication channel

### Response Times

- **Profile edits:** Review within 2 business days
- **Bug reports:** Acknowledgment within 1 business day
- **Feature requests:** Discussion at next team meeting

---

## ✅ Code Review Process

### What Happens When You Submit Changes

1. **Automatic Checks** (if configured)
   - HTML validation
   - File size check
   - Link verification

2. **Human Review**
   - Technical lead reviews changes
   - Checks for consistency with brand guidelines
   - Verifies no breaking changes

3. **Feedback**
   - Approved → Merged into main branch
   - Changes requested → You'll receive comments
   - Rejected → Explanation provided

### Making Changes After Review

```bash
# 1. Make requested changes in your branch
# Edit files based on feedback

# 2. Stage and commit changes
git add .
git commit -m "Address review comments: [specific changes]"

# 3. Push updated branch
git push origin profile/your-name

# Pull request automatically updates
```

---

## 🎨 Design Standards

### Maintaining Visual Consistency

**Colors:**
- Use CSS variables defined in `styles.css`
- Never hardcode colors in profile files
- If you need a new color, request it be added to theme

**Typography:**
- Use existing heading and text classes
- Don't change font sizes or families in individual profiles
- Maintain consistent formatting

**Layout:**
- Follow the template structure
- Don't add custom CSS to profile HTML files
- If you need custom styling, discuss with team first

### Accessibility

Ensure your content is accessible:
- Use descriptive alt text for images
- Maintain proper heading hierarchy
- Ensure sufficient color contrast
- Test with screen reader if possible

---

## 📝 Best Practices

### File Management

**DO:**
- Create images folder in profiles: `profiles/images/`
- Use descriptive file names: `john-smith-headshot.jpg`
- Compress images before uploading
- Test all links before submitting

**DON'T:**
- Upload unnecessarily large files
- Use special characters in file names
- Create nested folder structures
- Include temporary or backup files

### Version Control

**Good Commit Messages:**
```
✅ "Add Director 2 profile with biography and photo"
✅ "Fix broken image link in director-3.html"
✅ "Update CEO bio to reflect new achievements"
```

**Bad Commit Messages:**
```
❌ "Update"
❌ "Changes"
❌ "asdf"
❌ "Fixed stuff"
```

### Testing Checklist

Before submitting your changes:
- [ ] File opens correctly in browser
- [ ] All images load properly
- [ ] Text has no spelling or grammar errors
- [ ] "Back to VaZi Home" link works
- [ ] Page looks good on mobile device
- [ ] Tested in at least 2 browsers (Chrome, Firefox, Safari, or Edge)

---

## 🆘 Getting Help

### Resources

**Documentation:**
- [README.md](README.md) - Complete project documentation
- In-file comments - Each template has detailed instructions
- This file - Contribution guidelines

**Learning Resources:**
- [HTML Basics](https://developer.mozilla.org/en-US/docs/Learn/HTML) - Mozilla Developer Network
- [Git Basics](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics) - Official Git documentation
- [Markdown Guide](https://www.markdownguide.org/) - For README files

**Technical Support:**
- Email: tech-support@vazi.com (example)
- Team chat: #vazi-website channel
- Issue tracker: Repository issues tab

---

## 🎉 Recognition

We appreciate all contributions! Contributors will be:
- Credited in project documentation
- Acknowledged in team communications
- Thanked during project milestones

---

## 📜 Code of Conduct

### Our Commitment

We're committed to providing a welcoming and inclusive environment for all contributors.

### Expected Behavior

- Be respectful and professional
- Accept constructive criticism gracefully
- Focus on what's best for the project
- Show empathy toward other contributors

### Unacceptable Behavior

- Harassment or discriminatory language
- Personal attacks or insults
- Deliberate breaking of code
- Sharing others' private information

### Enforcement

Violations may result in:
1. Warning
2. Temporary suspension
3. Permanent ban from project

Report issues to: conduct@vazi.com (example)

---

## 📞 Contact

**Questions about contributing?**
- Technical Lead: [Name/Email]
- Project Manager: [Name/Email]
- General inquiries: team@vazi.com

**Found a security issue?**
- Email: security@vazi.com
- Do NOT post publicly

---

## 🔄 Document Updates

This contributing guide is a living document and may be updated as the project evolves.

**Last Updated:** February 2026  
**Version:** 1.0.0

---

Thank you for contributing to VaZi! Your work helps make our website better for everyone. 🎊
