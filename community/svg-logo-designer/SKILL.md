---
name: "SVG Logo Designer"
description: "Create professional SVG logos from descriptions and design specifications. Generates multiple logo variations with different layouts, styles, and concepts. Produces scalable vector graphics that can be used directly or exported to PNG. Use this skill when users ask to create logos, brand identities, icons, or visual marks for their designs."
---

# SVG Logo Designer

This skill creates professional, scalable vector graphic (SVG) logos from design specifications, offering multiple variations and layout options.

## When to Use This Skill

Activate this skill when the user requests:
- Create a logo from a description or specification
- Design a brand identity or visual mark
- Generate logo variations and concepts
- Create icons or symbols
- Design wordmarks or lettermarks
- Produce scalable graphics for branding
- Export logos in different layouts and styles

## Core Workflow

### Phase 1: Requirements Gathering

When a user requests a logo, gather comprehensive design requirements:

1. **Brand Information**
   - Company/product name
   - Industry and market
   - Target audience
   - Brand personality (modern, classic, playful, serious, etc.)
   - Brand values and messaging
   - Competitors (for differentiation)

2. **Design Preferences**
   - Logo type:
     - **Wordmark**: Text-based logo (Google, Coca-Cola style)
     - **Lettermark**: Initials/abbreviation (IBM, HBO style)
     - **Pictorial Mark**: Icon/symbol (Apple, Twitter style)
     - **Abstract Mark**: Abstract geometric form (Pepsi, Adidas style)
     - **Mascot**: Character-based (KFC Colonel, Michelin Man style)
     - **Combination Mark**: Icon + text (Burger King, Lacoste style)
     - **Emblem**: Text inside symbol (Starbucks, Harley-Davidson style)

3. **Style Guidelines**
   - Color palette (specific colors or let AI choose)
   - Color psychology considerations
   - Font preferences (if text-based)
   - Visual style:
     - Minimalist
     - Geometric
     - Organic/flowing
     - Bold/strong
     - Elegant/refined
     - Playful/friendly
     - Tech/modern
     - Vintage/retro

4. **Technical Requirements**
   - Size constraints (will it be used small? large?)
   - Application contexts (website, print, merchandise, etc.)
   - Color vs monochrome versions needed
   - Background usage (light, dark, transparent)
   - Scalability requirements

5. **Number of Variations**
   - How many different concepts? (Recommend 3-5)
   - How many layouts per concept? (Horizontal, vertical, square, circular)
   - Color variations needed?

### Phase 2: Design Concept Development

Create multiple logo concepts based on requirements:

#### Concept 1: Primary Direction

Develop the main design direction:

**Design Thinking:**
- Research visual metaphors related to brand
- Consider negative space opportunities
- Ensure memorability and uniqueness
- Balance simplicity with distinctiveness
- Consider cultural appropriateness

**SVG Structure:**
```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200" width="200" height="200">
  <defs>
    <linearGradient id="gradient1" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#4F46E5;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#7C3AED;stop-opacity:1" />
    </linearGradient>
  </defs>
  <g id="logo-symbol">
  </g>
  <g id="logo-text">
  </g>
</svg>
```

#### Concept 2-5: Alternative Directions

Create variations exploring different visual approaches:
- Different visual metaphors
- Different style treatments
- Different layouts and compositions
- Different color applications

### Phase 3: Layout Variations

For each concept, create multiple layout options:

#### Layout A: Horizontal Lockup
- Icon on left, text on right
- Best for website headers, business cards
- Wider aspect ratio

#### Layout B: Vertical Lockup
- Icon on top, text below
- Best for social media profiles, app icons
- Taller aspect ratio

#### Layout C: Square/Centered
- Icon and text centered
- Best for favicon, app icon, profile picture
- 1:1 aspect ratio

#### Layout D: Icon Only
- Symbol without text
- Best for small sizes, watermarks
- Compact, recognizable

#### Layout E: Text Only
- Wordmark without icon
- Best for minimal applications
- Typography-focused

### Phase 4: SVG Generation

Create professional, optimized SVG code:

**Best Practices:**

1. **Clean, Semantic Code**
```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 60">
  <g id="icon">
  </g>
  <g id="wordmark">
  </g>
</svg>
```

2. **Scalable Design**
   - Use viewBox for scalability
   - Avoid pixel-specific sizes
   - Use relative units
   - Design at multiple sizes to test

3. **Color Management**
```xml
<defs>
  <style>
    .primary { fill: #4F46E5; }
    .secondary { fill: #10B981; }
    .text { fill: #1F2937; }
  </style>
</defs>
<rect class="primary" x="0" y="0" width="100" height="100" />
```

4. **Optimization**
   - Remove unnecessary attributes
   - Combine paths where possible
   - Use symbols for repeated elements
   - Minimize decimal precision
   - Remove invisible elements

5. **Accessibility**
```xml
<svg role="img" aria-labelledby="logo-title logo-desc">
  <title id="logo-title">Company Name Logo</title>
  <desc id="logo-desc">A blue circular icon with the company name</desc>
</svg>
```

### Phase 5: Presentation

Present logos in an organized, professional manner with design rationale, all layout options, and color variations clearly documented.

### Phase 6: File Generation

Save SVG files with consistent naming conventions using the Write tool.

### Phase 7: Usage Guidelines

Provide comprehensive documentation covering file formats, export instructions, clear space requirements, minimum sizes, and color usage rules.

## Design Patterns

### Color Psychology Guide

- **Blue** (#0066CC): Trust, professionalism, stability
- **Green** (#10B981): Growth, health, eco-friendly
- **Red** (#DC2626): Energy, passion, urgency
- **Purple** (#7C3AED): Creativity, luxury, spirituality
- **Orange** (#F97316): Friendly, energetic, affordable
- **Teal** (#0D9488): Balance, clarity, sophistication
- **Black/Gray** (#1F2937): Sophisticated, modern, classic

## Key Principles

Great logos are: **simple, memorable, timeless, versatile, and appropriate.**
Focus on creating designs that work across all applications and stand the test of time.
