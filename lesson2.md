# Lesson 2: HTML Advanced Topics

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: Tables (15 minutes)

**Table Structure:**
```html
<table>
    <caption>Student Grades</caption>
    
    <thead>
        <tr>
            <th>Name</th>
            <th>Subject</th>
            <th>Grade</th>
        </tr>
    </thead>
    
    <tbody>
        <tr>
            <td>John</td>
            <td>Math</td>
            <td>A</td>
        </tr>
        <tr>
            <td>Jane</td>
            <td>Math</td>
            <td>B</td>
        </tr>
    </tbody>
    
    <tfoot>
        <tr>
            <td colspan="3">Class Average: A-</td>
        </tr>
    </tfoot>
</table>
```

**Table Elements:**
- `<table>` - Container
- `<caption>` - Table title
- `<thead>` - Header section
- `<tbody>` - Body section
- `<tfoot>` - Footer section
- `<tr>` - Table row
- `<th>` - Header cell (bold, centered)
- `<td>` - Data cell

**Merging Cells:**
```html
<!-- Merge columns -->
<td colspan="2">Cell spans 2 columns</td>

<!-- Merge rows -->
<td rowspan="2">Cell spans 2 rows</td>
```

**Live Coding Example:**
```html
<table>
    <caption>Monthly Sales</caption>
    <thead>
        <tr>
            <th>Product</th>
            <th>January</th>
            <th>February</th>
            <th>Total</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Laptop</td>
            <td>50</td>
            <td>45</td>
            <td>95</td>
        </tr>
        <tr>
            <td>Phone</td>
            <td>100</td>
            <td>120</td>
            <td>220</td>
        </tr>
    </tbody>
</table>
```

---

### Section 2: Span, Br, Hr Elements (5 minutes)

**Span (Inline Container):**
```html
<p>This is <span style="color: red;">red text</span> in a paragraph.</p>
```

**Line Break:**
```html
<p>First line<br>Second line<br>Third line</p>
```

**Horizontal Rule:**
```html
<h1>Section 1</h1>
<p>Content here.</p>
<hr>
<h1>Section 2</h1>
<p>More content.</p>
```

---

### Section 3: Div Element (5 minutes)

**Div (Block Container):**
```html
<div class="container">
    <h2>Title</h2>
    <p>Paragraph content</p>
    <span>Inline content</span>
</div>
```

**Common Uses:**
- Grouping elements
- Applying styles
- Creating layouts
- JavaScript manipulation

---

### Section 4: HTML Entities (5 minutes)

**Common Entities:**
```html
&lt;     <!-- Less than (<) -->
&gt;     <!-- Greater than (>) -->
&amp;    <!-- Ampersand (&) -->
&quot;   <!-- Quotation mark (") -->
&apos;   <!-- Apostrophe (') -->
&copy;   <!-- Copyright symbol (©) -->
&reg;    <!-- Registered trademark (®) -->
&trade;  <!-- Trademark (™) -->
&nbsp;   <!-- Non-breaking space -->
&euro;   <!-- Euro symbol (€) -->
&pound;  <!-- Pound symbol (£) -->
&yen;    <!-- Yen symbol (¥) -->
```

**Why Use Entities:**
- Display special characters
- Avoid HTML parsing issues
- Show symbols not on keyboard

---

### Section 5: Semantic Elements (15 minutes)

**HTML5 Semantic Elements:**
```html
<header>
    <nav>
        <ul>
            <li><a href="#home">Home</a></li>
            <li><a href="#about">About</a></li>
        </ul>
    </nav>
</header>

<main>
    <section>
        <h2>About Section</h2>
        <article>
            <h3>Article Title</h3>
            <p>Article content...</p>
        </article>
    </section>
    
    <aside>
        <h3>Sidebar</h3>
        <p>Sidebar content...</p>
    </aside>
</main>

<footer>
    <p>&copy; 2024 My Website</p>
</footer>
```

**details and summary Elements:**
```html
<!-- Collapsible content without JavaScript -->
<details>
    <summary>Click to see more</summary>
    <p>This content is hidden by default and shown when clicked.</p>
</details>

<!-- FAQ Example -->
<details>
    <summary>What is HTML?</summary>
    <p>HTML stands for HyperText Markup Language. It is used to structure web pages.</p>
</details>

<details>
    <summary>How do I create a link?</summary>
    <p>Use the anchor tag: <code>&lt;a href="url"&gt;Link text&lt;/a&gt;</code></p>
</details>

<details open>
    <summary>This is open by default</summary>
    <p>Add the <code>open</code> attribute to show content initially.</p>
</details>
```

**Why details/summary:**
- Built-in collapsible functionality
- No JavaScript required
- Perfect for FAQs, accordions
- Accessible by default
- Semantic and lightweight

**Semantic vs Non-Semantic:**
```html
<!-- Old Way (Non-Semantic) -->
<div class="header">Header</div>
<div class="nav">Navigation</div>
<div class="content">Content</div>
<div class="footer">Footer</div>

<!-- Modern Way (Semantic) -->
<header>Header</header>
<nav>Navigation</nav>
<main>Content</main>
<footer>Footer</footer>
```

**Benefits of Semantic HTML:**
- Better for SEO
- Improved accessibility
- Easier to read and maintain
- Better browser support

---

### Section 6: Page Layout Comparison (10 minutes)

**Old Layout (Div Soup):**
```html
<div class="header">
    <div class="logo">Logo</div>
    <div class="menu">Menu</div>
</div>
<div class="content">
    <div class="sidebar">Sidebar</div>
    <div class="main">Main Content</div>
</div>
<div class="footer">Footer</div>
```

**Modern Semantic Layout:**
```html
<header>
    <div class="logo">Logo</div>
    <nav>Menu</nav>
</header>
<main>
    <aside>Sidebar</aside>
    <section>Main Content</section>
</main>
<footer>Footer</footer>
```

**Why Semantic is Better:**
- Screen readers understand the structure
- Search engines rank semantic pages higher
- Code is more readable
- Easier to maintain

---

### Section 7: Audio Element (10 minutes)

**Basic Audio:**
```html
<audio controls>
    <source src="song.mp3" type="audio/mpeg">
    <source src="song.ogg" type="audio/ogg">
    Your browser does not support audio.
</audio>
```

**Audio Attributes:**
```html
<audio controls autoplay muted loop>
    <source src="background.mp3" type="audio/mpeg">
</audio>
```

**Attributes Explained:**
- `controls` - Show play/pause buttons
- `autoplay` - Play automatically (not recommended)
- `muted` - Start muted
- `loop` - Repeat when finished

**Best Practice:**
- Always provide multiple formats (mp3, ogg)
- Don't autoplay (user experience)
- Provide fallback text

---

### Section 8: Video Element (10 minutes)

**Basic Video:**
```html
<video controls width="500">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    Your browser does not support video.
</video>
```

**Video with Attributes:**
```html
<video 
    controls 
    width="640" 
    height="360"
    poster="poster.jpg"
    preload="metadata">
    <source src="video.mp4" type="video/mp4">
    <track 
        src="subtitles.vtt" 
        kind="subtitles" 
        srclang="en" 
        label="English">
</video>
```

**Video Attributes:**
- `controls` - Show video controls
- `autoplay` - Auto play (not recommended)
- `muted` - Start muted
- `loop` - Repeat video
- `poster` - Image before video plays
- `width/height` - Video dimensions
- `preload` - How to load video

**Subtitles/Captions:**
```html
<track 
    src="captions.vtt" 
    kind="captions" 
    srclang="en" 
    label="English Captions">
```

---

### Section 9: Forms Introduction (15 minutes)

**Basic Form Structure:**
```html
<form action="/submit" method="POST">
    <label for="name">Name:</label>
    <input 
        type="text" 
        id="name" 
        name="name" 
        required>
    
    <button type="submit">Submit</button>
</form>
```

**Form Attributes:**
- `action` - Where to send data
- `method` - How to send (GET or POST)
- `enctype` - Encoding type (for file uploads)

**GET vs POST:**
- GET - Data in URL (visible, limited size)
- POST - Data in body (hidden, larger size)

---

### Section 10: Input Types (15 minutes)

**Text Input:**
```html
<input type="text" name="username" placeholder="Enter username">
```

**Password Input:**
```html
<input type="password" name="password" placeholder="Enter password">
```

**Email Input:**
```html
<input type="email" name="email" required>
```

**Number Input:**
```html
<input type="number" name="age" min="18" max="100">
```

**Date Input:**
```html
<input type="date" name="birthdate">
```

**Color Input:**
```html
<input type="color" name="favorite-color">
```

**Range Input:**
```html
<input type="range" name="volume" min="0" max="100">
```

**Checkbox:**
```html
<input type="checkbox" name="subscribe" id="subscribe">
<label for="subscribe">Subscribe to newsletter</label>
```

**Radio Buttons:**
```html
<input type="radio" name="gender" value="male" id="male">
<label for="male">Male</label>

<input type="radio" name="gender" value="female" id="female">
<label for="female">Female</label>
```

**File Input:**
```html
<input type="file" name="avatar" accept="image/*">
```

**Hidden Input:**
```html
<input type="hidden" name="user-id" value="12345">
```

---

### Section 11: Input Attributes (10 minutes)

**Common Attributes:**
```html
<input 
    type="text" 
    name="username"
    placeholder="Enter username"
    required
    minlength="3"
    maxlength="20"
    readonly
    disabled
    autofocus
    value="default">
```

**Attributes Explained:**
- `placeholder` - Hint text
- `required` - Field must be filled
- `minlength/maxlength` - Length limits
- `readonly` - Can't be modified
- `disabled` - Can't be used
- `autofocus` - Focus on load
- `value` - Default value
- `pattern` - Regex pattern
- `step` - Number increment

---

### Section 12: Other Form Elements (10 minutes)

**Select Dropdown:**
```html
<label for="country">Country:</label>
<select name="country" id="country">
    <option value="">Select country</option>
    <option value="us">United States</option>
    <option value="uk">United Kingdom</option>
    <option value="ca">Canada</option>
</select>
```

**Select with Multiple:**
```html
<select name="languages" multiple>
    <option value="html">HTML</option>
    <option value="css">CSS</option>
    <option value="js">JavaScript</option>
</select>
```

**Textarea:**
```html
<label for="message">Message:</label>
<textarea 
    name="message" 
    id="message" 
    rows="5" 
    cols="30"
    placeholder="Write your message...">
</textarea>
```

**Datalist (Autocomplete):**
```html
<input list="languages" name="language">
<datalist id="languages">
    <option value="HTML">
    <option value="CSS">
    <option value="JavaScript">
    <option value="Python">
    <option value="Java">
</datalist>
```

**Buttons:**
```html
<button type="submit">Submit</button>
<button type="reset">Reset</button>
<button type="button">Click Me</button>
```

---

### Section 13: Accessibility Basics (10 minutes)

**What is Accessibility:**
- Making websites usable for everyone
- Including people with disabilities
- Legal requirement in many countries

**Key Principles:**
- Use semantic elements
- Add alt text to images
- Connect labels with inputs
- Use proper heading hierarchy
- Ensure keyboard navigation
- Provide sufficient color contrast

**Accessible Form:**
```html
<form>
    <label for="email">Email:</label>
    <input 
        type="email" 
        id="email" 
        name="email"
        aria-required="true"
        required>
    
    <button type="submit">Submit</button>
</form>
```

**Accessible Image:**
```html
<img src="chart.png" alt="Bar chart showing sales increased by 50% in 2024">
```

**Why Accessibility Matters:**
- 15% of world population has disabilities
- Better SEO
- Legal compliance
- Better user experience for everyone

---

### Section 14: ARIA Introduction (5 minutes)

**What is ARIA:**
- Accessible Rich Internet Applications
- Extends HTML for accessibility
- Used when native HTML isn't enough

**ARIA Labels:**
```html
<button aria-label="Close menu">X</button>
<div aria-hidden="true">Hidden content</div>
<button aria-expanded="false">Menu</button>
```

**ARIA Roles:**
```html
<div role="navigation">Navigation</div>
<div role="main">Main content</div>
<div role="complementary">Sidebar</div>
```

**Best Practice:**
- Use native HTML first
- Use ARIA only when necessary
- Test with screen readers

---

### Section 15: Live Server (5 minutes)

**What is Live Server:**
- VS Code extension
- Auto-refreshes browser on save
- Simulates real web server

**Benefits:**
- Faster development
- No manual refresh
- Better workflow
- Catches errors early

**How to Use:**
1. Install Live Server extension
2. Right-click on HTML file
3. Select "Open with Live Server"
4. Edit and save - browser auto-refreshes

---

### Section 16: Lighthouse (5 minutes)

**What is Lighthouse:**
- Chrome DevTools feature
- Website quality audit
- Performance and accessibility testing

**What it Checks:**
- Performance
- Accessibility
- Best Practices
- SEO
- Progressive Web App

**How to Use:**
1. Open Chrome DevTools (F12)
2. Go to Lighthouse tab
3. Select categories
4. Click "Analyze page load"
5. Review results

---

### Section 17: HTML Best Practices (10 minutes)

**Do's:**
✅ Use HTML5 doctype
✅ Use semantic elements
✅ Indent your code properly
✅ Write meaningful class names
✅ Always use UTF-8 encoding
✅ Always include alt on images
✅ Use only one H1 per page
✅ Connect labels with form inputs
✅ Validate your HTML
✅ Organize files into folders

**Don'ts:**
❌ Don't use inline styles
❌ Don't skip heading levels
❌ Don't use div for everything
❌ Don't forget closing tags
❌ Don't use deprecated tags
❌ Don't ignore accessibility
❌ Don't use tables for layout
❌ Don't forget meta tags

**File Organization:**
```
project/
├── index.html
├── about.html
├── contact.html
├── css/
│   └── style.css
├── js/
│   └── script.js
├── images/
├── videos/
└── audio/
```

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Create a Data Table (20 minutes)

**Task:**
Create a table showing student grades with:
- Caption: "Student Grades Fall 2024"
- Headers: Name, Math, Science, English, Average
- At least 3 students
- Calculate averages (you can hardcode)
- Use thead, tbody, tfoot
- Merge cells in footer for class average

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Student Grades</title>
</head>
<body>
    <h1>Student Grades</h1>
    
    <table>
        <caption>Student Grades Fall 2024</caption>
        
        <thead>
            <tr>
                <th>Name</th>
                <th>Math</th>
                <th>Science</th>
                <th>English</th>
                <th>Average</th>
            </tr>
        </thead>
        
        <tbody>
            <tr>
                <td>John Smith</td>
                <td>95</td>
                <td>88</td>
                <td>92</td>
                <td>91.6</td>
            </tr>
            <tr>
                <td>Jane Doe</td>
                <td>87</td>
                <td>94</td>
                <td>89</td>
                <td>90.0</td>
            </tr>
            <tr>
                <td>Bob Johnson</td>
                <td>92</td>
                <td>85</td>
                <td>95</td>
                <td>90.6</td>
            </tr>
        </tbody>
        
        <tfoot>
            <tr>
                <td colspan="4">Class Average:</td>
                <td>90.7</td>
            </tr>
        </tfoot>
    </table>
</body>
</html>
```

---

### Exercise 2: Create a Semantic Blog Layout (25 minutes)

**Task:**
Create a blog page using semantic elements:
- Header with logo and navigation
- Main content area with:
  - Hero section
  - Article section with at least 2 articles
  - Sidebar with related links
- Footer with copyright
- Use proper heading hierarchy
- Include some sample content

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Blog</title>
</head>
<body>
    <header>
        <div class="logo">MyBlog</div>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#articles">Articles</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>
    
    <main>
        <section class="hero">
            <h1>Welcome to My Blog</h1>
            <p>Learn about web development, design, and technology.</p>
        </section>
        
        <section class="articles">
            <h2>Latest Articles</h2>
            
            <article>
                <h3>Getting Started with HTML</h3>
                <p>HTML is the foundation of web development. Learn the basics...</p>
                <a href="#">Read more</a>
            </article>
            
            <article>
                <h3>CSS Styling Tips</h3>
                <p>Discover how to make your websites beautiful with CSS...</p>
                <a href="#">Read more</a>
            </article>
        </section>
        
        <aside>
            <h3>Categories</h3>
            <ul>
                <li><a href="#">Web Development</a></li>
                <li><a href="#">Design</a></li>
                <li><a href="#">Technology</a></li>
            </ul>
            
            <h3>Popular Posts</h3>
            <ul>
                <li><a href="#">10 HTML Tips</a></li>
                <li><a href="#">CSS Grid Guide</a></li>
                <li><a href="#">JavaScript Basics</a></li>
            </ul>
        </aside>
    </main>
    
    <footer>
        <p>&copy; 2024 MyBlog. All rights reserved.</p>
        <nav>
            <a href="#privacy">Privacy Policy</a>
            <a href="#terms">Terms of Service</a>
        </nav>
    </footer>
</body>
</html>
```

---

### Exercise 3: Create a Contact Form (25 minutes)

**Task:**
Create a comprehensive contact form with:
- Name input (text, required)
- Email input (email, required)
- Phone input (tel)
- Subject select dropdown
- Message textarea (required)
- Newsletter checkbox
- Submit button
- Proper labels connected to inputs
- Use appropriate input types and attributes

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Contact Us</title>
</head>
<body>
    <h1>Contact Us</h1>
    
    <form action="/submit-contact" method="POST">
        <div>
            <label for="name">Name:</label>
            <input 
                type="text" 
                id="name" 
                name="name" 
                required
                minlength="2"
                placeholder="Enter your full name">
        </div>
        
        <div>
            <label for="email">Email:</label>
            <input 
                type="email" 
                id="email" 
                name="email" 
                required
                placeholder="your@email.com">
        </div>
        
        <div>
            <label for="phone">Phone:</label>
            <input 
                type="tel" 
                id="phone" 
                name="phone"
                placeholder="(123) 456-7890">
        </div>
        
        <div>
            <label for="subject">Subject:</label>
            <select id="subject" name="subject" required>
                <option value="">Select a subject</option>
                <option value="general">General Inquiry</option>
                <option value="support">Technical Support</option>
                <option value="sales">Sales Question</option>
                <option value="feedback">Feedback</option>
            </select>
        </div>
        
        <div>
            <label for="message">Message:</label>
            <textarea 
                id="message" 
                name="message" 
                rows="5" 
                cols="30"
                required
                minlength="10"
                placeholder="Write your message here...">
            </textarea>
        </div>
        
        <div>
            <input type="checkbox" id="newsletter" name="newsletter">
            <label for="newsletter">Subscribe to our newsletter</label>
        </div>
        
        <button type="submit">Send Message</button>
    </form>
</body>
</html>
```

---

### Exercise 4: Create a Media Gallery (20 minutes)

**Task:**
Create a media gallery with:
- H1: "My Media Gallery"
- At least 2 audio files with controls
- At least 2 video files with controls
- Use poster images for videos
- Add captions/descriptions
- Make it accessible

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Media Gallery</title>
</head>
<body>
    <h1>My Media Gallery</h1>
    
    <section>
        <h2>Audio Collection</h2>
        
        <h3>Background Music</h3>
        <audio controls>
            <source src="audio/background.mp3" type="audio/mpeg">
            <source src="audio/background.ogg" type="audio/ogg">
            Your browser does not support audio.
        </audio>
        <p>Relaxing background music for focus and study.</p>
        
        <h3>Podcast Episode</h3>
        <audio controls>
            <source src="audio/podcast.mp3" type="audio/mpeg">
            Your browser does not support audio.
        </audio>
        <p>Web development podcast episode 1.</p>
    </section>
    
    <section>
        <h2>Video Collection</h2>
        
        <h3>Tutorial Video</h3>
        <video controls width="640" poster="images/tutorial-poster.jpg">
            <source src="videos/tutorial.mp4" type="video/mp4">
            <source src="videos/tutorial.webm" type="video/webm">
            <track 
                src="subtitles/tutorial.vtt" 
                kind="captions" 
                srclang="en" 
                label="English">
            Your browser does not support video.
        </video>
        <p>HTML tutorial for beginners.</p>
        
        <h3>Demo Video</h3>
        <video controls width="640" poster="images/demo-poster.jpg">
            <source src="videos/demo.mp4" type="video/mp4">
            Your browser does not support video.
        </video>
        <p>Project demonstration video.</p>
    </section>
</body>
</html>
```

---

### Exercise 5: Create a Registration Form (20 minutes)

**Task:**
Create a user registration form with:
- Personal information section
- Account details section
- Preferences section
- Use fieldsets and legends
- Various input types
- Password confirmation
- Terms and conditions checkbox
- Submit and reset buttons

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Register</title>
</head>
<body>
    <h1>Create Account</h1>
    
    <form action="/register" method="POST">
        <fieldset>
            <legend>Personal Information</legend>
            
            <div>
                <label for="fullname">Full Name:</label>
                <input 
                    type="text" 
                    id="fullname" 
                    name="fullname" 
                    required>
            </div>
            
            <div>
                <label for="email">Email:</label>
                <input 
                    type="email" 
                    id="email" 
                    name="email" 
                    required>
            </div>
            
            <div>
                <label for="birthdate">Birth Date:</label>
                <input 
                    type="date" 
                    id="birthdate" 
                    name="birthdate">
            </div>
            
            <div>
                <label for="country">Country:</label>
                <select id="country" name="country">
                    <option value="us">United States</option>
                    <option value="uk">United Kingdom</option>
                    <option value="ca">Canada</option>
                    <option value="au">Australia</option>
                </select>
            </div>
        </fieldset>
        
        <fieldset>
            <legend>Account Details</legend>
            
            <div>
                <label for="username">Username:</label>
                <input 
                    type="text" 
                    id="username" 
                    name="username" 
                    required
                    minlength="3"
                    maxlength="20">
            </div>
            
            <div>
                <label for="password">Password:</label>
                <input 
                    type="password" 
                    id="password" 
                    name="password" 
                    required
                    minlength="8">
            </div>
            
            <div>
                <label for="confirm-password">Confirm Password:</label>
                <input 
                    type="password" 
                    id="confirm-password" 
                    name="confirm-password" 
                    required>
            </div>
        </fieldset>
        
        <fieldset>
            <legend>Preferences</legend>
            
            <div>
                <label for="theme">Theme:</label>
                <select id="theme" name="theme">
                    <option value="light">Light</option>
                    <option value="dark">Dark</option>
                    <option value="auto">Auto</option>
                </select>
            </div>
            
            <div>
                <label for="language">Language:</label>
                <input list="languages" id="language" name="language">
                <datalist id="languages">
                    <option value="English">
                    <option value="Spanish">
                    <option value="French">
                    <option value="German">
                    <option value="Arabic">
                </datalist>
            </div>
            
            <div>
                <input type="checkbox" id="newsletter" name="newsletter">
                <label for="newsletter">Subscribe to newsletter</label>
            </div>
            
            <div>
                <input type="checkbox" id="terms" name="terms" required>
                <label for="terms">I agree to the terms and conditions</label>
            </div>
        </fieldset>
        
        <button type="submit">Create Account</button>
        <button type="reset">Reset Form</button>
    </form>
</body>
</html>
```

---

### Exercise 6: Create an FAQ Page with details/summary (15 minutes)

**Task:**
Create an FAQ page using details and summary elements:
- Create at least 5 FAQ items
- Use proper heading hierarchy
- One FAQ should be open by default
- Add descriptive summary text
- Include helpful answers

**Solution:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>FAQ - Frequently Asked Questions</title>
</head>
<body>
    <h1>Frequently Asked Questions</h1>
    
    <h2>General Questions</h2>
    
    <details>
        <summary>What is HTML?</summary>
        <p>HTML stands for HyperText Markup Language. It is the standard markup language for creating web pages and describing the structure of a web page.</p>
    </details>
    
    <details>
        <summary>Do I need to learn HTML before CSS?</summary>
        <p>Yes, it's highly recommended to learn HTML first. HTML provides the structure and content of a web page, while CSS is used to style and layout that content.</p>
    </details>
    
    <details open>
        <summary>How long does it take to learn HTML?</summary>
        <p>You can learn the basics of HTML in a few days or weeks. However, mastering HTML and understanding best practices may take several months of practice and building real projects.</p>
    </details>
    
    <h2>Technical Questions</h2>
    
    <details>
        <summary>What is the difference between HTML and HTML5?</summary>
        <p>HTML5 is the latest version of HTML. It introduced new semantic elements, better form handling, audio/video support, and improved APIs while maintaining backward compatibility with older HTML versions.</p>
    </details>
    
    <details>
        <summary>Can I use HTML for mobile apps?</summary>
        <p>Yes, HTML can be used in hybrid mobile app development using frameworks like React Native, Ionic, or Apache Cordova. These frameworks allow you to use web technologies to build mobile applications.</p>
    </details>
    
    <details>
        <summary>What tools do I need to write HTML?</summary>
        <p>You need a text editor (like VS Code, Sublime Text, or Notepad++) and a web browser (like Chrome, Firefox, or Edge) to view your HTML files. No special software is required.</p>
    </details>
    
    <p><em>Click on any question above to reveal the answer!</em></p>
</body>
</html>
```

---

### Exercise 7: Create an Accessible Product Page (20 minutes)

**Task:**
Create an accessible product page with:
- Semantic HTML structure
- Proper heading hierarchy
- Accessible images with descriptive alt text
- Accessible form with connected labels
- ARIA attributes where needed
- Sufficient color contrast (mention in comments)
- Keyboard navigable elements

**Solution:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Product Page - Wireless Headphones</title>
</head>
<body>
    <header>
        <nav aria-label="Main navigation">
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#products">Products</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>
    
    <main>
        <article>
            <h1>Wireless Headphones Pro</h1>
            
            <figure>
                <img 
                    src="images/headphones.jpg" 
                    alt="Wireless headphones in black color with noise cancelling feature"
                    width="400"
                    height="300">
                <figcaption>Premium wireless headphones with active noise cancellation</figcaption>
            </figure>
            
            <section aria-labelledby="price-heading">
                <h2 id="price-heading">Price</h2>
                <p><strong>$199.99</strong></p>
            </section>
            
            <section aria-labelledby="features-heading">
                <h2 id="features-heading">Features</h2>
                <ul>
                    <li>Active noise cancellation</li>
                    <li>30-hour battery life</li>
                    <li>Bluetooth 5.0 connectivity</li>
                    <li>Comfortable over-ear design</li>
                    <li>Built-in microphone</li>
                </ul>
            </section>
            
            <section aria-labelledby="purchase-heading">
                <h2 id="purchase-heading">Purchase</h2>
                <form action="/add-to-cart" method="POST">
                    <div>
                        <label for="quantity">Quantity:</label>
                        <input 
                            type="number" 
                            id="quantity" 
                            name="quantity" 
                            min="1" 
                            max="10" 
                            value="1"
                            aria-required="true">
                    </div>
                    
                    <div>
                        <label for="color">Color:</label>
                        <select id="color" name="color" aria-required="true">
                            <option value="black">Black</option>
                            <option value="white">White</option>
                            <option value="blue">Blue</option>
                        </select>
                    </div>
                    
                    <button type="submit" aria-label="Add wireless headphones to cart">
                        Add to Cart
                    </button>
                </form>
            </section>
            
            <section aria-labelledby="reviews-heading">
                <h2 id="reviews-heading">Customer Reviews</h2>
                <article>
                    <h3>Great headphones!</h3>
                    <p><strong>Rating: 5/5</strong></p>
                    <p>Amazing sound quality and very comfortable for long sessions.</p>
                    <p><cite>— John D., Verified Purchase</cite></p>
                </article>
            </section>
        </article>
    </main>
    
    <footer>
        <p>&copy; 2024 TechStore. All rights reserved.</p>
        <nav aria-label="Footer navigation">
            <a href="#shipping">Shipping Info</a>
            <a href="#returns">Returns</a>
            <a href="#support">Support</a>
        </nav>
    </footer>
</body>
</html>
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Review Questions (15 minutes)

**Question 1:** What is the difference between `<thead>`, `<tbody>`, and `<tfoot>`?
**Answer:** `<thead>` contains table headers, `<tbody>` contains the main data, and `<tfoot>` contains summary/footer information.

**Question 2:** Why should we use semantic HTML elements instead of divs?
**Answer:** Semantic elements improve accessibility, SEO, code readability, and provide meaning to the content structure.

**Question 3:** What is the purpose of the alt attribute on images?
**Answer:** It provides alternative text for screen readers and when images fail to load, making content accessible.

**Question 4:** What is the difference between GET and POST form methods?
**Answer:** GET sends data in the URL (visible, limited size), POST sends data in the body (hidden, larger size, more secure).

**Question 5:** Why should we connect labels with form inputs?
**Answer:** It improves accessibility, allows users to click the label to select the input, and provides better user experience.

**Question 6:** What is ARIA and when should we use it?
**Answer:** ARIA (Accessible Rich Internet Applications) extends HTML for accessibility. Use it only when native HTML cannot solve the accessibility need.

**Question 7:** What are the benefits of using Live Server?
**Answer:** Auto-refreshes browser on save, faster development workflow, simulates real web server behavior.

**Question 8:** What does Lighthouse audit check for?
**Answer:** Performance, accessibility, best practices, SEO, and progressive web app features.

**Question 9:** What is the purpose of the details and summary elements?
**Answer:** To create collapsible content without JavaScript, perfect for FAQs and accordions.

**Question 10:** Why should you add a favicon to your website?
**Answer:** It improves branding, helps users identify your website in browser tabs, and provides a professional appearance.

---

### Common Problems & Solutions (10 minutes)

**Problem 1:** Table not displaying correctly
**Solution:** Check that you have proper table structure with `<thead>`, `<tbody>`, and that all `<tr>` have correct number of cells.

**Problem 2:** Form not submitting data
**Solution:** Check the `action` and `method` attributes. Ensure input elements have `name` attributes.

**Problem 3:** Video/audio not playing
**Solution:** Check file paths, ensure you have multiple source formats, and verify the files exist.

**Problem 4:** Select dropdown not showing options
**Solution:** Make sure `<option>` elements are inside `<select>` and have proper `value` attributes.

**Problem 5:** Accessibility issues with screen readers
**Solution:** Add proper alt text, connect labels with inputs, use semantic elements, and add ARIA labels when needed.

**Problem 6:** Checkbox/radio not working with label
**Solution:** Ensure the `for` attribute on label matches the `id` attribute on the input.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a table with merged rows and columns combined.

**Challenge 2:** Create a form that uses all available input types.

**Challenge 3:** Build a page that passes a basic accessibility audit.

---

### Final Project Assignment

**Task:** Create a complete, accessible website for a small business with at least 4 pages:
1. Home page (index.html)
2. About page (about.html)
3. Products/Services page (products.html)
4. Contact page (contact.html)

**Requirements:**
- Semantic HTML5 structure on all pages
- Consistent navigation between all pages
- At least one table with proper structure
- Contact form with various input types
- Media elements (audio or video)
- Accessible images with descriptive alt text
- Proper heading hierarchy
- Mobile-friendly viewport meta tag
- Clean, organized code structure
- External CSS file linked
- SEO meta tags on all pages
- Favicon
- FAQ page with details/summary elements
- Responsive images using srcset
- Data attributes for dynamic elements

**Bonus:**
- Add ARIA attributes where needed
- Use datalist for autocomplete
- Implement form validation
- Add semantic sections (header, nav, main, footer)
- Embed YouTube video or Google Map using iframe
- Use picture element for art direction

**Due Date:** 1 week from today

---

## End of Lesson 2

**Congratulations!** You have completed the HTML Complete Course.

**Next Steps:**
- Practice building real websites
- Learn CSS for styling
- Learn JavaScript for interactivity
- Explore frameworks like React or Vue
- Keep building and improving your skills

**Remember:** The best way to learn is by building projects. Start small, practice daily, and don't be afraid to make mistakes!

Happy Coding! 🚀