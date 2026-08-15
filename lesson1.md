# Lesson 1: HTML Fundamentals

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: What is HTML? (10 minutes)

**Definition:**
- HTML stands for **HyperText Markup Language**
- It is **NOT a programming language**
- It is a **Markup Language** used to structure web pages

**The Web Development Trinity:**
- HTML → Skeleton (Structure)
- CSS → Design & Colors (Style)
- JavaScript → Brain & Interactions (Functionality)

**Live Coding Example:**
```html
<h1>Hello World</h1>
```

**Output in Browser:**
# Hello World

---

### Section 2: HTML Versions & History (5 minutes)

**Old Version:**
- HTML 4 (outdated)

**Current Version:**
- HTML5 (standard since 2014)

**HTML5 Introduced:**
- Semantic Elements
- Audio & Video support
- Better Forms
- Canvas element
- Local Storage
- Improved Accessibility

---

### Section 3: Why Learn HTML? (5 minutes)

**Essential for All Web Developers:**
- Front-End Developer
- Back-End Developer
- Full Stack Developer
- Mobile Developer (React Native, Flutter Web)
- UI Developer
- Email Template Developer
- CMS Developer
- Dashboard Developer

**HTML is the foundation of every website.**

---

### Section 4: Tools You Need (10 minutes)

**Text Editor (Choose One):**
- Visual Studio Code (Recommended)
- Cursor
- Atom
- Sublime Text
- Notepad++

**Browser (Choose One):**
- Google Chrome (Recommended)
- Firefox Developer Edition
- Microsoft Edge

**Important Advice:**
- Always Search
- Build Projects
- Have a Goal
- Focus
- Practice Daily
- Don't Memorize Everything

---

### Section 5: HTML Document Structure (15 minutes)

**Complete HTML Structure:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
</head>
<body>
    <h1>Welcome</h1>
    <p>This is my first website.</p>
</body>
</html>
```

**Explanation:**
- `<!DOCTYPE html>` - Tells browser to use HTML5
- `<html>` - Root element
- `<head>` - Contains page information (not visible)
- `<body>` - Contains visible content

**Live Coding - Create Your First Page:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Page</title>
</head>
<body>
    <h1>Hello World</h1>
    <p>I am learning HTML!</p>
</body>
</html>
```

---

### Section 6: HTML Tags (10 minutes)

**Tag Structure:**
- Opening Tag: `<p>`
- Content: `Hello`
- Closing Tag: `</p>`

**Example:**
```html
<p>Hello</p>
```

**Self-Closing Tags:**
```html
<img />
<br />
<input />
```

**Tag Rules:**
- Tags must be properly nested
- Always close your tags
- Tags are case-insensitive (use lowercase)

---

### Section 7: Head Section (15 minutes)

**What Goes in Head:**
```html
<head>
    <title>Page Title</title>
    <meta charset="UTF-8">
    <meta name="description" content="Page description">
    <link rel="stylesheet" href="style.css">
</head>
```

**Meta Tags:**
```html
<!-- Character Encoding -->
<meta charset="UTF-8">

<!-- Page Description -->
<meta name="description" content="Learning HTML">

<!-- Viewport for Mobile -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- SEO Meta Tags -->
<meta name="keywords" content="HTML, web development, coding">
<meta name="author" content="Your Name">

<!-- Open Graph Tags for Social Media -->
<meta property="og:title" content="My Website">
<meta property="og:description" content="Learn HTML with me">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com">

<!-- Favicon -->
<link rel="icon" href="favicon.ico" type="image/x-icon">
<link rel="icon" href="favicon.png" type="image/png">
```

**Style Tag (Internal CSS):**
```html
<style>
    h1 {
        color: red;
    }
</style>
```

**Script Tag (JavaScript):**
```html
<script>
    console.log("Hello");
</script>
```

**Link Tag (External CSS):**
```html
<link rel="stylesheet" href="style.css">
```

---

### Section 8: Comments (5 minutes)

**Single Line Comment:**
```html
<!-- This is a comment -->
```

**Multi-line Comment:**
```html
<!--
This is a
multi-line comment
-->
```

**Why Use Comments:**
- Explain your code
- Temporarily disable code
- Leave notes for other developers

---

### Section 9: Headings (10 minutes)

**HTML Headings:**
```html
<h1>Main Title</h1>
<h2>Section</h2>
<h3>Subsection</h3>
<h4>Heading</h4>
<h5>Heading</h5>
<h6>Heading</h6>
```

**Important Rule:**
- Only **ONE H1 per page**
- Use headings in order (don't skip levels)
- Headings are important for SEO

**Live Coding Example:**
```html
<h1>My Website</h1>
<h2>About Me</h2>
<h3>My Skills</h3>
<h4>HTML</h4>
<h4>CSS</h4>
```

---

### Section 10: Paragraphs (5 minutes)

**Paragraph Tag:**
```html
<p>This is a paragraph.</p>
<p>This is another paragraph.</p>
```

**Paragraph is a Block Element** (starts on new line)

---

### Section 11: Block vs Inline Elements (10 minutes)

**Block Elements:**
- Start on a new line
- Take full width available
- Examples: `div`, `p`, `h1-h6`, `section`, `header`

**Inline Elements:**
- Stay on the same line
- Take only necessary width
- Examples: `span`, `a`, `strong`, `em`, `img`

**Live Coding Comparison:**
```html
<!-- Block Elements -->
<div>Block 1</div>
<div>Block 2</div>

<!-- Inline Elements -->
<span>Inline 1</span>
<span>Inline 2</span>
```

---

### Section 12: Attributes (10 minutes)

**What are Attributes:**
- Provide extra information about elements
- Always in the opening tag
- Format: `name="value"`

**Example:**
```html
<img src="cat.jpg" alt="Cat">
```

**Common Attributes:**
- `src` - Source (for images, scripts)
- `href` - Link destination
- `alt` - Alternative text
- `class` - CSS class
- `id` - Unique identifier

---

### Section 13: Global Attributes (5 minutes)

**Available on Almost Every Element:**
- `class` - For styling
- `id` - Unique identifier
- `title` - Tooltip text
- `style` - Inline CSS
- `hidden` - Hide element

**Data Attributes (Custom Data):**
```html
<!-- data-* attributes store custom data for JavaScript -->
<div data-id="123" data-user="john" data-role="admin">
    User Information
</div>

<button data-action="delete" data-item="post-5">
    Delete Post
</button>
```

**Why Data Attributes:**
- Store custom data on HTML elements
- Accessible via JavaScript
- Essential for dynamic interactions
- Doesn't affect styling or default behavior

**Example:**
```html
<p class="important" id="para1" title="Important paragraph" data-priority="high">
    This is important!
</p>
```

---

### Section 14: Text Formatting (10 minutes)

**Bold Text:**
```html
<b>Bold</b> <!-- Visual only -->
<strong>Important</strong> <!-- Semantic meaning -->
```

**Italic Text:**
```html
<i>Italic</i> <!-- Visual only -->
<em>Emphasis</em> <!-- Semantic meaning -->
```

**Other Formatting:**
```html
<mark>Highlight</mark>
<u>Underline</u>
<small>Small Text</small>
<del>Old Price</del>
<ins>New Text</ins>
H<sub>2</sub>O <!-- Subscript -->
x<sup>2</sup> <!-- Superscript -->
```

---

### Section 15: Links (10 minutes)

**Basic Link:**
```html
<a href="https://google.com">Google</a>
```

**Important Attributes:**
- `href` - URL destination
- `target` - How to open link
- `title` - Tooltip

**Open in New Tab:**
```html
<a href="https://google.com" target="_blank">Google</a>
```

**With Tooltip:**
```html
<a href="https://google.com" title="Visit Google">Google</a>
```

**Internal Link:**
```html
<a href="about.html">About Page</a>
```

**Link to Section:**
```html
<a href="#section1">Go to Section 1</a>
```

---

### Section 16: Images (10 minutes)

**Basic Image:**
```html
<img src="image.jpg" alt="Mountain">
```

**Important Rules:**
- Always write `alt` text for accessibility
- Avoid `width` and `height` in HTML (use CSS)
- Use descriptive file names

**Image with Folder Path:**
```html
<img src="images/mountain.jpg" alt="Mountain">
```

**Image from URL:**
```html
<img src="https://example.com/image.jpg" alt="Description">
```

---

### Section 16.5: Responsive Images (10 minutes)

**srcset Attribute:**
```html
<!-- Browser chooses best image based on screen size -->
<img src="small.jpg"
     srcset="small.jpg 500w,
             medium.jpg 1000w,
             large.jpg 2000w"
     alt="Responsive image">
```

**sizes Attribute:**
```html
<img src="default.jpg"
     srcset="small.jpg 500w,
             medium.jpg 1000w,
             large.jpg 2000w"
     sizes="(max-width: 600px) 500px,
            (max-width: 1200px) 1000px,
            2000px"
     alt="Responsive image with sizes">
```

**picture Element:**
```html
<!-- Different images for different conditions -->
<picture>
    <source media="(max-width: 600px)" srcset="mobile.jpg">
    <source media="(max-width: 1200px)" srcset="tablet.jpg">
    <img src="desktop.jpg" alt="Responsive image with picture">
</picture>
```

**Why Responsive Images:**
- Faster loading on mobile devices
- Better user experience
- Saves bandwidth
- Prepares for CSS Responsive Design

---

### Section 16.7: iframe (5 minutes)

**Basic iframe:**
```html
<iframe src="https://example.com" width="600" height="400"></iframe>
```

**YouTube Embed:**
```html
<iframe 
    width="560" 
    height="315" 
    src="https://www.youtube.com/embed/VIDEO_ID"
    title="YouTube video"
    frameborder="0"
    allowfullscreen>
</iframe>
```

**Google Maps Embed:**
```html
<iframe 
    width="600" 
    height="450" 
    src="https://www.google.com/maps/embed?pb=MAP_ID"
    style="border:0;" 
    allowfullscreen="" 
    loading="lazy">
</iframe>
```

**Common Uses:**
- Embed YouTube videos
- Google Maps
- External websites
- PDF documents
- Social media widgets

**Security:**
- Use `sandbox` attribute for untrusted content
- Only embed from trusted sources

---

### Section 17: Lists (15 minutes)

**Unordered List (Bullets):**
```html
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
```

**Ordered List (Numbers):**
```html
<ol>
    <li>Wake Up</li>
    <li>Study</li>
    <li>Practice</li>
</ol>
```

**Ordered List Attributes:**
```html
<ol start="5" reversed type="A">
    <li>Item E</li>
    <li>Item D</li>
    <li>Item C</li>
</ol>
```

**Description List:**
```html
<dl>
    <dt>HTML</dt>
    <dd>Markup Language</dd>
    <dt>CSS</dt>
    <dd>Styling Language</dd>
</dl>
```

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Create Your First HTML Page (20 minutes)

**Task:**
Create a file named `index.html` with the following:
- Proper HTML5 structure
- A title in the head
- An H1 heading
- A paragraph about yourself
- Save and open in browser

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Page</title>
</head>
<body>
    <h1>Welcome to My Page</h1>
    <p>My name is [Your Name] and I am learning HTML.</p>
</body>
</html>
```

---

### Exercise 2: Create an About Me Page (25 minutes)

**Task:**
Create a page with:
- H1: Your name
- H2: About Me
- Paragraph: Description of yourself
- H2: My Skills
- Unordered list of your skills
- H2: My Goals
- Ordered list of your goals

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>About Me</title>
</head>
<body>
    <h1>[Your Name]</h1>
    
    <h2>About Me</h2>
    <p>I am a web development student. I love coding and building websites.</p>
    
    <h2>My Skills</h2>
    <ul>
        <li>HTML</li>
        <li>CSS</li>
        <li>JavaScript</li>
    </ul>
    
    <h2>My Goals</h2>
    <ol>
        <li>Learn HTML</li>
        <li>Learn CSS</li>
        <li>Build a portfolio</li>
        <li>Get a job as a developer</li>
    </ol>
</body>
</html>
```

---

### Exercise 3: Create a Links Page (20 minutes)

**Task:**
Create a page with:
- H1: Useful Links
- Link to Google (opens in new tab)
- Link to YouTube
- Link to a local page (create a second page)
- Link to a section on the same page

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Useful Links</title>
</head>
<body>
    <h1>Useful Links</h1>
    
    <h2>Search Engines</h2>
    <ul>
        <li><a href="https://google.com" target="_blank" title="Search on Google">Google</a></li>
        <li><a href="https://bing.com" target="_blank">Bing</a></li>
    </ul>
    
    <h2>Learning Resources</h2>
    <ul>
        <li><a href="https://youtube.com" target="_blank">YouTube</a></li>
        <li><a href="https://mdn.mozilla.org" target="_blank">MDN Web Docs</a></li>
    </ul>
    
    <h2>Internal Links</h2>
    <ul>
        <li><a href="about.html">About Page</a></li>
        <li><a href="#contact">Contact Section</a></li>
    </ul>
    
    <h2 id="contact">Contact</h2>
    <p>Email: example@email.com</p>
</body>
</html>
```

---

### Exercise 4: Create an Image Gallery (25 minutes)

**Task:**
Create a page with:
- H1: My Image Gallery
- At least 3 images (use placeholder images)
- Each image should have alt text
- Add descriptions below each image

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Image Gallery</title>
</head>
<body>
    <h1>My Image Gallery</h1>
    
    <h2>Nature Photos</h2>
    <img src="https://via.placeholder.com/300" alt="Mountain landscape">
    <p>A beautiful mountain landscape.</p>
    
    <img src="https://via.placeholder.com/300" alt="Ocean view">
    <p>A peaceful ocean view at sunset.</p>
    
    <img src="https://via.placeholder.com/300" alt="Forest path">
    <p>A mysterious path through the forest.</p>
</body>
</html>
```

---

### Exercise 5: Create a Resume Page (25 minutes)

**Task:**
Create a professional resume page with:
- Your name as H1
- Contact information (with links to email/social media)
- H2: Education
- Ordered list of education
- H2: Experience
- Description list of experience
- H2: Skills
- Unordered list of skills
- Use text formatting tags appropriately

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Resume</title>
</head>
<body>
    <h1>John Doe</h1>
    
    <h2>Contact Information</h2>
    <p>
        Email: <a href="mailto:john@example.com">john@example.com</a><br>
        LinkedIn: <a href="https://linkedin.com" target="_blank">LinkedIn Profile</a>
    </p>
    
    <h2>Education</h2>
    <ol>
        <li>Bachelor of Computer Science - University Name (2020-2024)</li>
        <li>High School Diploma - School Name (2016-2020)</li>
    </ol>
    
    <h2>Experience</h2>
    <dl>
        <dt>Web Developer Intern - Tech Company (2023)</dt>
        <dd>Developed responsive websites using HTML and CSS</dd>
        
        <dt>Freelance Developer (2022-Present)</dt>
        <dd>Built websites for small businesses</dd>
    </dl>
    
    <h2>Skills</h2>
    <ul>
        <li><strong>HTML5</strong> - Expert</li>
        <li><strong>CSS3</strong> - Intermediate</li>
        <li><em>JavaScript</em> - Beginner</li>
        <li><mark>Problem Solving</mark></li>
    </ul>
    
    <h2>About Me</h2>
    <p>
        I am a passionate web developer with experience in building 
        <strong>modern and responsive</strong> websites. I love learning 
        new technologies and solving complex problems.
    </p>
</body>
</html>
```

---

### Exercise 6: Create a Page with SEO Meta Tags (15 minutes)

**Task:**
Create a page with proper SEO meta tags:
- Add description meta tag
- Add keywords meta tag
- Add author meta tag
- Add Open Graph tags for social media
- Add favicon link
- Include proper title tag

**Solution:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- SEO Meta Tags -->
    <title>My Portfolio | Web Developer</title>
    <meta name="description" content="Professional web developer portfolio showcasing HTML, CSS, and JavaScript projects.">
    <meta name="keywords" content="web developer, HTML, CSS, JavaScript, portfolio">
    <meta name="author" content="Your Name">
    
    <!-- Open Graph Tags for Social Media -->
    <meta property="og:title" content="My Portfolio | Web Developer">
    <meta property="og:description" content="Professional web developer portfolio showcasing HTML, CSS, and JavaScript projects.">
    <meta property="og:image" content="https://example.com/og-image.jpg">
    <meta property="og:url" content="https://example.com">
    <meta property="og:type" content="website">
    
    <!-- Favicon -->
    <link rel="icon" href="favicon.ico" type="image/x-icon">
    <link rel="icon" href="favicon.png" type="image/png">
</head>
<body>
    <h1>Welcome to My Portfolio</h1>
    <p>This page is optimized for search engines and social media sharing.</p>
</body>
</html>
```

---

### Exercise 7: Create a Page with Data Attributes (15 minutes)

**Task:**
Create a page that uses data attributes:
- Create a product list with data-id, data-price, data-category
- Create buttons with data-action attributes
- Add user elements with data-role and data-status
- Include comments explaining what each data attribute does

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Data Attributes Example</title>
</head>
<body>
    <h1>Product List</h1>
    
    <!-- Product items with data attributes -->
    <div class="product" data-id="1" data-price="29.99" data-category="electronics">
        <h3>Laptop</h3>
        <p>Price: $29.99</p>
        <button data-action="add-to-cart" data-product-id="1">Add to Cart</button>
    </div>
    
    <div class="product" data-id="2" data-price="49.99" data-category="electronics">
        <h3>Headphones</h3>
        <p>Price: $49.99</p>
        <button data-action="add-to-cart" data-product-id="2">Add to Cart</button>
    </div>
    
    <div class="product" data-id="3" data-price="19.99" data-category="books">
        <h3>Programming Book</h3>
        <p>Price: $19.99</p>
        <button data-action="add-to-cart" data-product-id="3">Add to Cart</button>
    </div>
    
    <!-- User management with data attributes -->
    <div class="user" data-user-id="101" data-role="admin" data-status="active">
        <h3>Admin User</h3>
        <button data-action="edit" data-user-id="101">Edit</button>
        <button data-action="delete" data-user-id="101">Delete</button>
    </div>
    
    <div class="user" data-user-id="102" data-role="user" data-status="inactive">
        <h3>Regular User</h3>
        <button data-action="edit" data-user-id="102">Edit</button>
        <button data-action="activate" data-user-id="102">Activate</button>
    </div>
</body>
</html>
```

---

### Exercise 8: Create a Page with Responsive Images (20 minutes)

**Task:**
Create a page with responsive images:
- Use srcset attribute with different image sizes
- Use sizes attribute for breakpoints
- Use picture element for art direction
- Add fallback images
- Include proper alt text

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Responsive Images</title>
</head>
<body>
    <h1>Responsive Image Gallery</h1>
    
    <h2>Using srcset and sizes</h2>
    <img 
        src="images/default.jpg"
        srcset="images/small.jpg 500w,
                images/medium.jpg 1000w,
                images/large.jpg 2000w"
        sizes="(max-width: 600px) 500px,
               (max-width: 1200px) 1000px,
               2000px"
        alt="Responsive landscape image">
    
    <h2>Using picture element for art direction</h2>
    <picture>
        <source media="(max-width: 600px)" srcset="images/mobile-portrait.jpg">
        <source media="(max-width: 1200px)" srcset="images/tablet-landscape.jpg">
        <img src="images/desktop.jpg" alt="Responsive image with different crops for different devices">
    </picture>
    
    <h2>Responsive hero image</h2>
    <picture>
        <source media="(max-width: 768px)" srcset="images/hero-mobile.jpg">
        <source media="(min-width: 769px)" srcset="images/hero-desktop.jpg">
        <img src="images/hero-desktop.jpg" alt="Hero banner image">
    </picture>
    
    <p>These images automatically adjust based on screen size and device capabilities.</p>
</body>
</html>
```

---

### Exercise 9: Create a Page with iframe Embeds (15 minutes)

**Task:**
Create a page with various iframe embeds:
- Embed a YouTube video
- Embed a Google Map
- Create a simple webpage iframe
- Add proper attributes for security and accessibility

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>iframe Embeds</title>
</head>
<body>
    <h1>Embedded Content</h1>
    
    <h2>YouTube Video</h2>
    <iframe 
        width="560" 
        height="315" 
        src="https://www.youtube.com/embed/dQw4w9WgXcQ"
        title="YouTube video player"
        frameborder="0" 
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
        allowfullscreen>
    </iframe>
    
    <h2>Google Maps</h2>
    <iframe 
        width="600" 
        height="450" 
        style="border:0" 
        loading="lazy"
        allowfullscreen
        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3151.835434509374!2d144.9537353153167!3d-37.8173239797517!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x6ad642af0f11fd81%3A0xf577d6a32f7f1f81!2sFederation%20Square!5e0!3m2!1sen!2sau!4v1631234567890!5m2!1sen!2sau">
    </iframe>
    
    <h2>External Website</h2>
    <iframe 
        src="https://example.com" 
        width="100%" 
        height="400"
        title="Example website"
        sandbox="allow-same-origin allow-scripts">
    </iframe>
    
    <p>These iframes demonstrate how to embed external content in your HTML pages.</p>
</body>
</html>
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Review Questions (15 minutes)

**Question 1:** What does HTML stand for?
**Answer:** HyperText Markup Language

**Question 2:** Is HTML a programming language?
**Answer:** No, it's a markup language

**Question 3:** What is the difference between block and inline elements?
**Answer:** Block elements start on a new line and take full width. Inline elements stay on the same line and take only necessary width.

**Question 4:** Why should we use semantic HTML?
**Answer:** It improves accessibility, SEO, and code readability.

**Question 5:** What is the purpose of the alt attribute on images?
**Answer:** It provides alternative text for screen readers and when images fail to load.

**Question 6:** How do you create a link that opens in a new tab?
**Answer:** Add `target="_blank"` to the anchor tag.

**Question 7:** What is the difference between `<b>` and `<strong>`?
**Answer:** `<b>` is visual only, `<strong>` has semantic meaning (important text).

**Question 8:** How many H1 tags should you have on a page?
**Answer:** Only one H1 per page.

**Question 9:** What are data-* attributes used for?
**Answer:** To store custom data on HTML elements that can be accessed via JavaScript.

**Question 10:** What is the purpose of Open Graph meta tags?
**Answer:** They control how your website appears when shared on social media platforms.

**Question 11:** What is the difference between srcset and the picture element?
**Answer:** srcset provides different image sizes for the same image, while picture allows different images for different conditions (art direction).

**Question 12:** What is an iframe used for?
**Answer:** To embed external content like YouTube videos, Google Maps, or other websites.

---

### Common Problems & Solutions (10 minutes)

**Problem 1:** Page not displaying correctly
**Solution:** Check if you saved the file with `.html` extension and opened it in a browser.

**Problem 2:** Images not showing
**Solution:** Check the file path in the `src` attribute. Make sure the image file exists in the correct location.

**Problem 3:** Links not working
**Solution:** Check the `href` attribute. For external links, include `https://`. For internal links, check the file name and extension.

**Problem 4:** Code not formatting correctly
**Solution:** Make sure you're using proper tag nesting and closing all tags.

**Problem 5:** Page title not showing
**Solution:** Make sure the `<title>` tag is inside the `<head>` section.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a page with a nested list (list inside a list).

**Challenge 2:** Create a page that links to itself using anchor links.

**Challenge 3:** Use all text formatting tags in a single paragraph.

---

### Homework Assignment

**Task:** Create a complete personal website with at least 3 pages:
1. Home page (index.html)
2. About page (about.html)
3. Contact page (contact.html)

**Requirements:**
- Proper HTML5 structure on all pages
- Navigation links between all pages
- At least one image on each page (consider using responsive images)
- Use of headings, paragraphs, lists, and links
- Proper use of semantic elements
- All images must have alt text
- SEO meta tags (description, keywords, Open Graph tags)
- Favicon
- At least one example of data-* attributes
- One iframe embed (YouTube video or Google Map)

**Bonus:**
- Use srcset for responsive images
- Create an FAQ section with details/summary

**Due Date:** Next lesson

---

## End of Lesson 1

**Next Lesson:** HTML Advanced Topics (Tables, Forms, Semantic Elements, Audio/Video, Accessibility)