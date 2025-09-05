# Overview

This is a mobile-first wishlist application built with Vue.js 3 and Vite. The app allows users to manage their wishlist items across different categories with budget visualization capabilities using AmCharts5. It features a modern, responsive design optimized for mobile devices with drag-and-drop functionality for organizing wishlist items.

# User Preferences

Preferred communication style: Simple, everyday language.

# System Architecture

## Frontend Framework
- **Vue.js 3**: Chosen for its composition API and reactive data binding, making it ideal for dynamic wishlist management
- **Vite**: Used as the build tool for fast development and optimized production builds
- **Single Page Application (SPA)**: Provides smooth mobile experience without page reloads

## UI/UX Design
- **Mobile-first approach**: CSS designed with mobile breakpoints as priority, ensuring optimal mobile experience
- **Component-based architecture**: Vue components for reusable UI elements like category buttons and wishlist items
- **Responsive design**: Adapts to different screen sizes while maintaining mobile optimization

## Interactive Features
- **SortableJS**: Enables drag-and-drop functionality for reordering wishlist items
- **Category-based organization**: Items can be organized into different categories with horizontal scrolling menu
- **Touch-friendly interface**: Large touch targets and mobile-optimized interactions

## Data Visualization
- **AmCharts5**: Advanced charting library for budget visualization and wishlist analytics
- **D3.js integration**: AmCharts5 includes D3 for complex data manipulations and custom visualizations

## Development Configuration
- **Host configuration**: Server configured to run on all network interfaces (0.0.0.0) for easy mobile testing
- **Port 5000**: Consistent port configuration for development and preview modes

# External Dependencies

## Core Framework Dependencies
- **Vue.js 3.5.21**: Main frontend framework
- **@vitejs/plugin-vue 6.0.1**: Vite plugin for Vue single-file component support
- **Vite 7.1.4**: Build tool and development server

## Visualization and Charts
- **@amcharts/amcharts5 5.13.6**: Professional charting library with extensive chart types and mobile optimization
- **D3.js ecosystem**: Included via AmCharts5 for data processing and custom visualizations

## User Interface Enhancements
- **SortableJS 1.15.6**: Drag-and-drop functionality for reordering wishlist items
- **Flatpickr**: Date picker functionality (included via AmCharts5)

## Development Tools
- **TypeScript definitions**: Comprehensive type definitions for D3, GeoJSON, and various chart types
- **PDF generation**: pdfmake support for exporting wishlist data (via AmCharts5)

# Current Implementation

## Features Completed
- **Category Navigation**: Four categories (Groceries, Business, Clothes, Software) with active state highlighting
- **Item Display**: Left panel shows selected item details with name and price in PHP currency
- **Reorderable List**: Right panel shows category items with drag-and-drop reordering using SortableJS
- **Budget Visualization**: Items fill with green gradient based on budget allocation (100% fill for affordable items, partial fill for items within remaining budget)
- **Budget Input**: Real-time budget entry with PHP currency formatting
- **Add Items**: Simple dialog-based item addition with name and price input
- **Mobile Responsive**: Optimized for mobile devices with touch-friendly interface

## Recent Design Updates (Sept 2025)
- **High-Tech Finance Dashboard Transformation**: Complete UI redesign from modern SaaS app to professional financial dashboard
- **Professional Typography**: Implemented Gambetta (serif) for headings/labels and JetBrains Mono (monospace) for all monetary values and data
- **Solarized Light Color Scheme**: Applied sophisticated color palette with warm cream backgrounds (#fdf6e3), professional blue-gray text (#657b83), and trustworthy blue data values (#268bd2)
- **Sharp Professional Styling**: Reduced border-radius from 12px to 2-4px, replaced card shadows with thin 1px borders
- **Condensed Layout**: Reduced whitespace and padding for more data-dense, dashboard-like appearance
- **Subtle Interactions**: Replaced animated hover effects with professional border color changes

## Data Structure
The app uses a client-side data structure with predefined items in each category:
- Groceries: Rice, Cooking Oil, Vegetables, Meat Package
- Business: Office Chair, Standing Desk, Monitor  
- Clothes: Business Shirt, Formal Pants, Leather Shoes
- Software: Adobe Creative Suite, Microsoft Office, Design Software

## Budget Algorithm
Items are filled sequentially based on their position in the list:
1. Affordable items get 100% fill (green background)
2. Partially affordable items get proportional fill based on remaining budget
3. Unaffordable items get 0% fill

Note: The application operates as a client-side only app with no backend or database integration. All data is stored in component state and resets on page refresh.