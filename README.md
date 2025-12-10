# Cameo - Modern Project Showcase Component

## Overview

**Cameo** is a sophisticated, standalone web component designed for displaying portfolio projects, case studies, or recent work in a clean, modern grid layout. Built with semantic HTML, modern CSS, and a mobile-first approach, this component provides an elegant solution for designers, developers, and agencies to showcase their work effectively.

## Live Deployment

[View Component](https://thisislefa.github.io/Cameo)

## Technical Specifications

### Core Architecture

Cameo implements a modular, component-based architecture with the following key features:

- **Pure HTML/CSS Implementation** - No JavaScript dependencies required
- **CSS Custom Properties** - Theming system using CSS variables
- **Responsive Grid Layout** - Adaptive 2-column to 1-column design
- **Semantic HTML5** - Accessible markup structure
- **Performance Optimized** - Minimal CSS footprint with efficient selectors

### Design System

#### Typography Hierarchy
- **Primary Font:** Inter (400, 500, 700, 800 weights)
- **Header Title:** 3.5rem desktop, scaling down to 2.25rem mobile
- **Project Titles:** 1.25rem with letter-spacing optimization
- **Category Tags:** 0.9rem uppercase with tracking

#### Color Palette
```css
--color-black: #000000;          /* Primary text and hover states */
--color-light-gray: #f8f8f8;     /* Reserved for potential backgrounds */
--color-text-muted: #6b6b6b;     /* Secondary text and categories */
--color-accent: #525252;         /* Subtle accent for indicators */
```

#### Spacing & Layout
- **Container Width:** 1280px maximum with 90% width on desktop
- **Grid Gaps:** 20px between cards, adaptive spacing on mobile
- **Card Aspect Ratio:** 4:3 (75% padding-top technique)
- **Border Radius:** 12px for card images, creating soft modern edges

### Component Structure

```
project-showcase/
├── showcase-header/
│   ├── header-tag (Featured projects)
│   └── header-title (Recent Works)
└── project-grid/
    ├── project-card (x4)
    │   ├── card-media/
    │   │   └── img (Unsplash placeholder)
    │   └── card-details/
    │       ├── project-title
    │       └── project-categories
```

## Integration Guide

### Basic Implementation

1. **Include Dependencies:**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="cameo.css">
```

2. **HTML Structure:**
```html
<section class="project-showcase">
    <header class="showcase-header">
        <p class="header-tag">Your Tagline</p>
        <h2 class="header-title">Section Title</h2>
    </header>
    <div class="project-grid">
        <!-- Repeat for each project -->
        <a href="#" class="project-card">
            <div class="card-media">
                <img src="project-image.jpg" alt="Project description">
            </div>
            <div class="card-details">
                <h3 class="project-title">Project Name</h3>
                <p class="project-categories">Category 1 • Category 2</p>
            </div>
        </a>
    </div>
</section>
```

### Advanced Integration Patterns

#### With Static Site Generators

**Next.js (React):**
```jsx
import ProjectCard from '@/components/ProjectCard';

export default function PortfolioPage({ projects }) {
    return (
        <section className="project-showcase">
            <header className="showcase-header">
                <p className="header-tag">Featured Work</p>
                <h2 className="header-title">Portfolio</h2>
            </header>
            <div className="project-grid">
                {projects.map((project) => (
                    <ProjectCard key={project.id} project={project} />
                ))}
            </div>
        </section>
    );
}
```

**11ty (JavaScript):**
```nunjucks
<section class="project-showcase">
    <header class="showcase-header">
        <p class="header-tag">{{ tagline }}</p>
        <h2 class="header-title">{{ title }}</h2>
    </header>
    <div class="project-grid">
        {% for project in collections.projects %}
        <a href="{{ project.url }}" class="project-card">
            <div class="card-media">
                <img src="{{ project.data.image }}" alt="{{ project.data.title }}">
            </div>
            <div class="card-details">
                <h3 class="project-title">{{ project.data.title }}</h3>
                <p class="project-categories">{{ project.data.categories | join(' • ') }}</p>
            </div>
        </a>
        {% endfor %}
    </div>
</section>
```

#### With Content Management Systems

**WordPress Integration:**
```php
<?php
$projects = get_posts(array(
    'post_type' => 'portfolio',
    'posts_per_page' => 4
));
?>
<section class="project-showcase">
    <header class="showcase-header">
        <p class="header-tag"><?php echo get_field('portfolio_tagline', 'option'); ?></p>
        <h2 class="header-title"><?php echo get_field('portfolio_title', 'option'); ?></h2>
    </header>
    <div class="project-grid">
        <?php foreach ($projects as $project): ?>
        <a href="<?php echo get_permalink($project->ID); ?>" class="project-card">
            <div class="card-media">
                <?php echo get_the_post_thumbnail($project->ID, 'large'); ?>
            </div>
            <div class="card-details">
                <h3 class="project-title"><?php echo $project->post_title; ?></h3>
                <p class="project-categories">
                    <?php echo get_the_term_list($project->ID, 'portfolio_category', '', ' • ', ''); ?>
                </p>
            </div>
        </a>
        <?php endforeach; ?>
    </div>
</section>
```

### API-Driven Implementation

**JavaScript Fetch Example:**
```javascript
async function loadProjects() {
    try {
        const response = await fetch('/api/projects');
        const projects = await response.json();
        
        const grid = document.querySelector('.project-grid');
        grid.innerHTML = projects.map(project => `
            <a href="${project.url}" class="project-card">
                <div class="card-media">
                    <img src="${project.image}" alt="${project.title}" loading="lazy">
                </div>
                <div class="card-details">
                    <h3 class="project-title">${project.title}</h3>
                    <p class="project-categories">${project.categories.join(' • ')}</p>
                </div>
            </a>
        `).join('');
    } catch (error) {
        console.error('Failed to load projects:', error);
    }
}

// Initialize on DOM ready
document.addEventListener('DOMContentLoaded', loadProjects);
```

## Customization Options

### Theming via CSS Custom Properties

Override the default theme by adding these variables to your main stylesheet:

```css
:root {
    --font-primary: 'Your-Font', sans-serif;
    --color-black: #222222;
    --color-text-muted: #777777;
    --color-accent: #3a86ff;
}

.project-showcase {
    /* Override container width */
    max-width: 1400px;
}

.project-card:hover .project-title {
    /* Custom hover effect */
    text-decoration-color: var(--color-accent);
}
```

### Extending Functionality

**Add Lazy Loading:**
```html
<img src="placeholder.jpg" data-src="real-image.jpg" loading="lazy" class="lazy-load">
```

**Implement Intersection Observer for animations:**
```javascript
const observerOptions = {
    threshold: 0.1,
    rootMargin: '0px 0px -50px 0px'
};

const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('animate-in');
        }
    });
}, observerOptions);

document.querySelectorAll('.project-card').forEach(card => {
    observer.observe(card);
});
```

## Performance Considerations

### Optimizations Included
- **CSS containment:** Each card is a self-contained layout unit
- **Efficient image rendering:** `object-fit: cover` with fixed aspect ratios
- **Minimal reflows:** CSS Grid provides stable layout performance
- **Mobile-first breakpoints:** Reduced complexity on smaller devices

### Additional Optimization Recommendations
1. **Image Optimization:**
   - Implement `srcset` for responsive images
   - Use WebP format with JPEG fallbacks
   - Consider using an image CDN for automatic optimization

2. **Critical CSS:**
   - Inline critical CSS for above-the-fold content
   - Load remaining styles asynchronously

3. **Font Loading Strategy:**
```css
/* Font loading optimization */
@font-face {
    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-display: swap;
    src: url('/fonts/inter.woff2') format('woff2');
}
```

## Browser Compatibility

| Feature | Chrome | Firefox | Safari | Edge |
|---------|---------|----------|---------|-------|
| CSS Grid | 57+ | 52+ | 10.1+ | 16+ |
| CSS Variables | 49+ | 31+ | 9.1+ | 15+ |
| object-fit | 31+ | 36+ | 10+ | 16+ |
| Flexbox | 29+ | 28+ | 9+ | 12+ |

**Polyfills Available:** For IE11 support, include:
- `css-vars-ponyfill` for CSS custom properties
- Custom Grid polyfill or fallback to Flexbox

## Accessibility Features

### Built-in Accessibility
- Semantic HTML structure (`<section>`, `<header>`, `<h2>` hierarchy)
- ARIA labels can be easily added to project cards
- Keyboard navigation support via anchor tags
- Sufficient color contrast ratios (AAA compliant for text)

### Enhancing Accessibility
```html
<a href="/project" class="project-card" aria-labelledby="project-title-1 project-categories-1">
    <div class="card-media">
        <img src="project.jpg" alt="Description of project visual">
    </div>
    <div class="card-details">
        <h3 class="project-title" id="project-title-1">Project Name</h3>
        <p class="project-categories" id="project-categories-1">Category 1 • Category 2</p>
    </div>
</a>
```

## Development Workflow

### Prerequisites
- Modern text editor (VS Code, Sublime Text, etc.)
- Git for version control
- Node.js (optional, for build tools)
- Local development server (Live Server, http-server)

### Setup Instructions

1. **Clone or Download:**
```bash
git clone https://github.com/thisislefa/Cameo.git
cd cameo-component
```

2. **Local Development:**
```bash
# Using Python
python -m http.server 8000

# Using Node.js
npx serve .
```

3. **Build Integration:**
```bash
# Example with npm scripts
npm install
npm run build:css  # Process and minify CSS
npm run watch      # Development with hot reload
```

## Contribution Guidelines

Cameo is an open-source component welcoming contributions from the community.

### How to Contribute

1. **Fork the Repository**
2. **Create a Feature Branch:**
```bash
git checkout -b feature/enhancement-name
```

3. **Development Standards:**
   - Follow existing code style and structure
   - Add comments for complex CSS rules
   - Update documentation for new features
   - Ensure responsive behavior is maintained

4. **Testing Requirements:**
   - Test in multiple browsers (Chrome, Firefox, Safari)
   - Verify responsive behavior (mobile, tablet, desktop)
   - Check accessibility with screen readers
   - Validate HTML structure

5. **Submit Pull Request:**
   - Provide clear description of changes
   - Include before/after screenshots for visual changes
   - Reference any related issues

### Areas for Contribution

1. **New Features:**
   - Filtering capabilities
   - Pagination or load more functionality
   - Sorting options (date, alphabetical, custom)
   - Animation variants (fade, slide, scale)

2. **Integration Examples:**
   - Framework-specific implementations (React, Vue, Angular)
   - CMS integration guides (WordPress, Drupal, Shopify)
   - E-commerce adaptations

3. **Accessibility Improvements:**
   - Enhanced keyboard navigation
   - Screen reader optimizations
   - Reduced motion alternatives

4. **Performance Enhancements:**
   - Lazy loading implementations
   - Image optimization strategies
   - Reduced JavaScript dependencies

## License

Cameo is released under the MIT License, allowing for free use, modification, and distribution in both personal and commercial projects.

## Support

For issues, questions, or suggestions:
- **GitHub Issues:** Report bugs or request features
- **Documentation:** Check the project wiki for advanced usage
- **Community:** Share implementations and adaptations

---

*Cameo represents a production-ready web component following modern development practices. Its modular design and clean separation of concerns make it suitable for integration into any web project, from simple static sites to complex web applications.*


