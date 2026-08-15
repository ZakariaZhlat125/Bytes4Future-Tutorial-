# Markdown (MD) Basics
### A Beginner's Guide

---

# Table of Contents

1. [What is Markdown?](#1-what-is-markdown)
2. [Why Use Markdown?](#2-why-use-markdown)
3. [Markdown Files](#3-markdown-files)
4. [Headings](#4-headings)
5. [Paragraphs](#5-paragraphs)
6. [Text Formatting](#6-text-formatting)
7. [Lists](#7-lists)
8. [Links](#8-links)
9. [Images](#9-images)
10. [Code](#10-code)
11. [Blockquotes](#11-blockquotes)
12. [Horizontal Rules](#12-horizontal-rules)
13. [Tables](#13-tables)
14. [Task Lists](#14-task-lists)
15. [Escaping Characters](#15-escaping-characters)
16. [Best Practices](#16-best-practices)

---

# 1. What is Markdown?

**Markdown (MD)** is a lightweight markup language used to create formatted text using simple, readable syntax.

Markdown files usually have the extension:

```
.md
```

Examples:

```
README.md
Notes.md
Tutorial.md
Course.md
```

---

# 2. Why Use Markdown?

Markdown is widely used because it is:

- Easy to learn
- Easy to read
- Easy to write
- Platform independent
- Supported by GitHub, GitLab, VS Code, and many other tools

Common uses:

- Project documentation
- README files
- Notes
- Blogs
- Technical documentation
- Course materials

---

# 3. Markdown Files

Create a new file with the `.md` extension.

Example:

```
README.md
```

You can edit Markdown using:

- Visual Studio Code
- Cursor
- Obsidian
- Typora
- GitHub Editor

---

# 4. Headings

Use `#` symbols to create headings.

```md
# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6
```

---

# 5. Paragraphs

Simply write text on separate lines.

```md
This is the first paragraph.

This is the second paragraph.
```

---

# 6. Text Formatting

## Bold

```md
**Bold Text**
```

Output:

**Bold Text**

---

## Italic

```md
*Italic Text*
```

Output:

*Italic Text*

---

## Bold and Italic

```md
***Bold and Italic***
```

Output:

***Bold and Italic***

---

## Strikethrough

```md
~~Deleted Text~~
```

Output:

~~Deleted Text~~

---

# 7. Lists

## Unordered List

```md
- HTML
- CSS
- JavaScript
```

Output:

- HTML
- CSS
- JavaScript

---

## Ordered List

```md
1. Learn HTML
2. Learn CSS
3. Learn JavaScript
```

Output:

1. Learn HTML
2. Learn CSS
3. Learn JavaScript

---

# 8. Links

```md
[Google](https://www.google.com)
```

Output:

[Google](https://www.google.com)

---

# 9. Images

```md
![Logo](images/logo.png)
```

Syntax:

```md
![Alt Text](image-path)
```

---

# 10. Code

## Inline Code

```md
Use `git status` to check changes.
```

Output:

Use `git status` to check changes.

---

## Code Block

````md
```html
<h1>Hello World</h1>
```
````

Output:

```html
<h1>Hello World</h1>
```

---

# 11. Blockquotes

```md
> This is a quote.
```

Output:

> This is a quote.

---

# 12. Horizontal Rules

Use three dashes, asterisks, or underscores.

```md
---
```

Output:

---

# 13. Tables

```md
| Name | Age |
|------|----:|
| Ali  | 22  |
| Sara | 25  |
```

Output:

| Name | Age |
|------|----:|
| Ali  | 22  |
| Sara | 25  |

---

# 14. Task Lists

```md
- [x] Learn HTML
- [x] Learn CSS
- [ ] Learn JavaScript
```

Output:

- [x] Learn HTML
- [x] Learn CSS
- [ ] Learn JavaScript

---

# 15. Escaping Characters

To display Markdown characters as plain text, use a backslash (`\`).

Example:

```md
\# This is not a heading
```

Output:

\# This is not a heading

---

# 16. Best Practices

✅ Use meaningful headings.

✅ Keep formatting simple.

✅ Use lists for readability.

✅ Add code blocks for code examples.

✅ Write descriptive link text.

✅ Use relative paths for local images.

✅ Preview your Markdown before sharing.

---

# Example README.md

~~~~md
# My Project

A simple project built with HTML, CSS, and JavaScript.

## Features

- Responsive Design
- Fast Performance
- Easy to Customize

## Installation

```bash
git clone https://github.com/username/project.git
```

## Author

John Doe
~~~~

---

# Summary

You now know the basic Markdown syntax:

- Headings
- Paragraphs
- Bold & Italic
- Lists
- Links
- Images
- Code Blocks
- Blockquotes
- Horizontal Rules
- Tables
- Task Lists

Markdown is simple, clean, and one of the most useful skills for writing documentation and README files.