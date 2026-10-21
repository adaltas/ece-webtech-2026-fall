---
duration: 1h
---

# Introduction to Web Technologies

Course objectives:

- Understand how web pages load.
- Learn about the history of web technologies.
- Explore key concepts and tools for web development.

## How does a web page load?

When a URL is entered in the browser, a series of steps occurs to fetch and display the web page. Here’s a high-level overview of the sequence:

1. Enter an URL in the browser

    - **User Action**: the user types <https://example.com> in the browser’s address bar and presses Enter.
    - **Browser Pre-Checks**: The browser checks if the domain is already cached locally (in the browser or OS cache). If it is, it can skip the DNS lookup.

2. DNS Lookup

    - **DNS Request**: If the IP address isn’t cached, the browser (or the OS) sends a request to a DNS resolver to find the IP address of example.com.
    - **DNS Response**: The DNS resolver replies with the IP address of the server hosting the site.

3. TCP/HTTPS Handshake

    - **TCP Connection**: The browser initiates a TCP connection with the server using the IP address obtained from the DNS lookup.
    - **TLS/SSL Handshake**: For secure sites (HTTPS), the browser and the server perform a TLS/SSL handshake to create a secure connection. This process includes negotiating encryption algorithms, exchanging keys, and confirming the connection.

4. HTTP Request

    - **HTTP Request**: The browser sends an HTTP request to the server to retrieve the web page. The request includes the type of request (GET, POST, etc.), the URL, and other headers.

      Example of a GET request:

      ```http
      GET / HTTP/1.1
      Host: example.com
      ```

5. Server Processing

    - **Server Receives Request**: The web server (e.g., Apache, Nginx, Node.js-based, etc.) receives the HTTP request and processes it.

      - If static content (HTML file) is requested, the server retrieves it from the file system.
      - If dynamic content is requested, the server might run a script (CGI, PHP, Node.js, etc.) or query a database (MySQL, MongoDB, etc.).

6. HTTP Response

    - **HTTP Response**: The server sends an HTTP response to the browser. The response contains the requested web page, along with the status code (e.g., 200 for success, 404 for “Not Found”, etc.) and headers.

      Example of an HTTP response:

      ```http
      HTTP/1.1 200 OK
      Content-Type: text/html; charset=UTF-8

      <!DOCTYPE html>
      <html>
      <head>
        <title>Example Domain</title>
      </head>
      <body>
        <h1>Hello, World!</h1>
      </body>
      </html>
      ```

7. Browser Rendering

    - **Browser Parses HTML**: The browser parses the HTML to build the Document Object Model (DOM) tree.
    - **Additional Requests**: The browser might discover additional resources (stylesheets, images, scripts, etc.) while parsing the HTML. It sends additional requests for these resources.
    - **Rendering**: The browser renders the web page on the screen according to the DOM tree and CSS styles.
    - **JavaScript Execution**: If the web page includes JavaScript, the browser executes the scripts.

8. Interaction

    - **User Interaction**: Clicking links, submitting forms, or triggering JavaScript events may cause more HTTP requests or DOM updates.

This simple sequence underpins all modern web requests. Even when using advanced frameworks (like Next.js) or complex infrastructures (load balancers, CDNs), the basic flow—DNS → Server → Response → Browser Render—remains at the core.

## Brief History of Web Technologies

### 1989: Invention of the Web

- Tim Berners-Lee, a British computer scientist, invented the World Wide Web in 1989 while working at CERN.
- The first web browser (WorldWideWeb) and web server (httpd) were created in 1990.
- The web was initially text-based, with hyperlinks connecting documents (HTML files) on different servers.
- Key technologies:
  - HTML (Hypertext Markup Language): Defines the structure of web pages.
  - HTTP (Hypertext Transfer Protocol): Defines how web servers and browsers communicate.
  - URL (Uniform Resource Locator): Identifies web resources.
- The web was initially used for sharing information and research.

### Early 1990s: Mosaic and the Browser Wars

- Mosaic, developed in 1993 at the National Center for Supercomputing Applications (NCSA), was the first popular graphical web browser.
- Mosaic’s success led to the “Browser Wars” between Netscape Navigator and Microsoft Internet Explorer.
- Key technologies:
  - GIF (Graphics Interchange Format): Supported by Mosaic, allowed images in web pages.
  - JavaScript: Developed by Netscape in 1995, added interactivity to web pages.
  - Cookies: Introduced by Netscape in 1994, used for session management.
- The web became more visual and interactive, with the rise of images, animations, and forms.

### Early to mid-1990s: CGI

- "Common Gateway Interface"
- Landscape: Most pages were static (HTML files). To add interactivity—like processing a form—developers needed a way to run programs on the server and return dynamic results.
- CGI is a simple protocol that allows a web server (e.g., Apache) to execute external programs (often written in Perl, C, or shell scripts) and send their output back to the client’s browser.
- Used for simple tasks like form processing, guestbooks, and counters.
- Advantages: First real dynamic content, language flexibility.
- Disadvantages: Performance overhead, security risks, scaling challenges.

### Late 1990s to mid-2000s: LAMP Stack Era

- "Linux, Apache, MySQL, PHP/Perl/Python"
- Landscape: The internet was expanding rapidly; more sites needed database-driven content (e.g., forums, blogs, e-commerce).
- The LAMP stack became a de facto standard for hosting dynamic websites because it was open source, relatively easy to set up, and cost-effective. PHP emerged as the dominant server-side language, with MySQL as the database.
- Popular platforms that emerged during this time (e.g., WordPress, Drupal).
- Advantages: cost-effective, open source, integration, scalability.
- Disadvantages: monolithic architecture, security & maintenance, limited front-end interactivity.

### Late 2000s into the 2010s: Rise of SPAs

- "Single Page Applications"
- Landscape: Web applications were becoming more interactive, but the traditional request-response model of the web was limiting. JavaScript rose in prominence thanks to libraries like jQuery, then more robust frameworks like AngularJS (2010), React (2013), and Vue.js (2014).
- SPAs load a single HTML page and dynamically update content through JavaScript, often using AJAX or fetch/axios calls to a REST API. Instead of refreshing the whole page, the web app updates only the needed parts of the DOM, resulting in a smoother, more app-like experience.
- AJAX was popularized by Google (Gmail, Google Maps) and led to the rise of single-page applications (SPAs).
- Advantages: interactivity, speed, decoupled front-end and back-end, user experience.
- Disadvantages: SEO challenges, initial load time, browser history and navigation complexity.

### Mid-2010s and forward: SSR in Modern Frameworks

- "Server-Side Rendering"
- Landscape: As SPAs became popular, new frameworks (e.g., Next.js for React, Nuxt.js for Vue, SvelteKit for Svelte) emerged to address shortcomings, especially around SEO, performance, and initial load.
- With SSR, the server pre-renders the initial HTML content and sends a fully formed page to the client. The page loads quickly and is SEO-friendly because the HTML markup is already available for crawlers. The client-side JavaScript then takes over to enable interactivity (“hydration”), making the page dynamic.
- Advantages: SEO-friendly, performance, initial load time, user and developer experience.
- Disadvantages: complexity, server-side overhead, build setup

The next chapters of this history - component libraries, meta-frameworks, rendering strategies - are the subject of modules 4 to 6.

## Key Concepts and Tools

Following are some key concepts essential to any web developer and to the rest of this course:

### UNIX-like Operating Systems

- UNIX: Developed in the 1970s at AT&T Bell Labs.
- UNIX-like operating systems share similar design principles and commands.
- Best suited for development and server environments:
  - Command-line tools and scripting capabilities.
  - Package managers for easy software installation.
  - Strong security features.
  - Stability and performance.
- Linux: Developed in the 1990s by Linus Torvalds.
- Most web servers run on Linux.
- macOS: Based on UNIX (BSD).
- Windows: Not UNIX-like, but runs a real Linux through WSL 2 (Windows Subsystem for Linux).

### Git

- Version control system for tracking changes in source code.
- Developed by Linus Torvalds in 2005 to manage the Linux kernel.
- Must-know for developers to collaborate and manage code.
- Key concepts:
  - Repository: A collection of files and their history.
  - Commit: A snapshot of changes to the repository.
  - Branch: A parallel version of the repository.
  - Merge: Combining branches.
  - Pull Request: Propose changes and discuss before merging.
- Distributed: Every developer has a local copy of the repository.
- Platforms for hosting repositories:
  - GitHub: Most popular, acquired by Microsoft in 2018.
  - GitLab: Open-core, offers self-hosted and cloud-based solutions.
  - Bitbucket: By Atlassian, integrated with Jira.

#### Conventional Commits

- A specification for structuring commit messages.
- Format: `<type>[optional scope]: <description>`
- Types:
  - `feat`: A new feature.
  - `fix`: A bug fix.
  - `docs`: Documentation changes.
  - `style`: Code style changes (e.g., formatting).
  - `refactor`: Code changes that neither fix a bug nor add a feature.
  - `test`: Adding or modifying tests.
  - `chore`: Maintenance tasks, build changes, etc.
- Benefits:
  - Standardizes commit messages.
  - Enables automated release notes and versioning.
  - Improves readability and searchability.
- Full spec: <https://www.conventionalcommits.org>

### Markdown

- Lightweight markup language with plain-text formatting syntax.
- Easy to read and write.
- Commonly used for documentation, README files, and online forums.
- Features:
  - Headings: `# Heading 1`, `## Heading 2`, etc.
  - Lists: `* Item 1`, `1. Item 2`, etc.
  - Links: `[Text](URL)`.
  - Images: `![Alt text](URL)`.
  - Code blocks: `` `Code` ``, ```` `Multiline code` ````.
  - Tables: `| Header 1 | Header 2 |`, `| --- | --- |`, `| Row 1 | Row 2 |`.
  - Emphasis: `*Italic*`, `**Bold**`.
- GitHub, GitLab, and Bitbucket render Markdown files in repositories.

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
