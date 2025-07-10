# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static HTML documentation website for a Japanese bakery's operational manual system. The site documents an Excel+VBA-based management system that handles production planning, order management, and inventory for a manufacturing center that supplies approximately 40 retail stores.

## Architecture

### File Structure
- `index.html` - Main landing page with navigation to all manual sections
- `pages/` - Individual manual pages for different business functions:
  - `daily-operations.html` - Daily staff operations manual
  - `system-overview.html` - Technical system architecture documentation
  - `product-management.html` - Product database management
  - `order-management.html` - Ordering and procurement workflows
  - `production-management.html` - Manufacturing planning and scheduling
  - `reports.html` - Various report generation procedures
  - `troubleshooting.html` - Problem resolution guide
- `assets/scss/style.scss` - SCSS source file with comprehensive design system
- `assets/css/style.css` - Compiled CSS output
- Markdown documentation files containing business process details

### Design System
The SCSS architecture uses a structured approach with:
- Color variables for consistent theming (primary: #e8a87c, secondary: #f4c2a1)
- Responsive breakpoints (mobile: 768px, tablet: 1024px, large: 1200px)
- Mixins for card styles, hover effects, and gradients
- Component-based styling for navigation, cards, steps, badges, and buttons

### Navigation Structure
All pages must maintain consistent navigation with 8 core sections:
1. ホーム (Home)
2. 日常操作 (Daily Operations)
3. システム構成 (System Overview)
4. 商品管理 (Product Management)
5. 発注管理 (Order Management)
6. 製造数管理 (Production Management)
7. 各種帳票作成 (Reports)
8. トラブル対応 (Troubleshooting)

Each page includes mobile-responsive hamburger menu with JavaScript toggle functionality.

## Development Commands

### SCSS Compilation
```bash
# Compile SCSS to CSS with source maps
npx sass assets/scss/style.scss assets/css/style.css --style=expanded --source-map

# Watch for changes during development
npx sass --watch assets/scss/style.scss assets/css/style.css --style=expanded --source-map
```

### Dependencies
```bash
# Install Sass compiler
npm install sass --save-dev
```

## Key Implementation Details

### Mobile Navigation
All HTML pages must include the hamburger menu button and JavaScript for toggle functionality:
```html
<button class="nav__toggle" onclick="toggleNav()">
    <span></span>
    <span></span>
    <span></span>
</button>
```

### Page Template Structure
Each page follows this structure:
1. Navigation header with mobile toggle
2. Container div wrapping main content
3. Header section with title and subtitle
4. Main content sections using card grids and step containers
5. Footer with copyright and home link
6. JavaScript for mobile navigation

### Business Context
The documentation covers a complex Excel+VBA system managing:
- Product database with 20+ attributes per item
- Daily, weekly, and monthly operational workflows
- Multi-company order processing (internal "CB" and external "クリエ")
- Production scheduling based on different bread types and lead times
- Automated report generation and printing workflows

When modifying content, ensure accuracy with the business processes described in the markdown documentation files, particularly regarding VBA automation, production timing, and multi-store logistics.