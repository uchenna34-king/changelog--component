# Changelog Component - Layering & Positioning Guide

## Overview
This changelog component uses CSS Grid, Flexbox, and absolute positioning techniques to create a professional vertical timeline layout with proper layering.

---

## Key Positioning & Layering Techniques

### 1. **Vertical Timeline Line (Pseudo-element)**
```css
.timeline::before {
    position: absolute;
    left: 18px;
    top: 0;
    bottom: 0;
    width: 2px;
    background: #e0e0e0;
    z-index: 0; /* Behind everything */
}
```
- Uses `::before` pseudo-element to create the vertical line without additional HTML elements
- **Absolute positioning** makes it extend from top to bottom
- **z-index: 0** ensures it stays behind all timeline items

### 2. **Timeline Item Grid Layout**
```css
.timeline-item {
    display: grid;
    grid-template-columns: 60px 1fr;
    grid-gap: 20px;
    position: relative;
    z-index: 2; /* Above the line, below dots */
}
```
- **CSS Grid** creates a two-column layout: marker area | content area
- **Relative positioning** establishes a stacking context
- **z-index: 2** layers it above the vertical line but below dots

### 3. **Timeline Marker (Dot Container)**
```css
.timeline-marker {
    position: relative;
    z-index: 3; /* Above content */
    display: flex;
    justify-content: center;
}
```
- **Relative positioning** allows absolute children to position within it
- **z-index: 3** ensures dots appear above content text

### 4. **Timeline Dot (Absolute Positioning)**
```css
.timeline-dot {
    position: absolute;
    left: 50%;
    top: 0;
    transform: translateX(-50%);
    z-index: 4; /* On top of everything */
}
```
- **Absolute positioning** places it over the vertical line
- **transform: translateX(-50%)** centers it horizontally (better than `margin: 0 auto`)
- **z-index: 4** highest layer - appears on top of the vertical line
- Positioned at same coordinates as the line (left: 18px) for alignment

### 5. **Content Area (Flex Alignment)**
```css
.timeline-content {
    position: relative;
    z-index: 2;
}
```
- Contains date and description text
- Positioned to the right of the marker in the grid

---

## Z-Index Stacking Order (Bottom to Top)

| Z-Index | Element | Purpose |
|---------|---------|---------|
| 0 | `.timeline::before` | Vertical line - background |
| 1 | (none) | Reserved space |
| 2 | `.timeline-item` + `.timeline-content` | Timeline items and text |
| 3 | `.timeline-marker` | Marker container |
| 4 | `.timeline-dot` | Dot circles - foreground |
| 10 | `.changelog-header`, `.changelog-footer` | Header and footer sections |

---

## Positioning Techniques Used

### Absolute Positioning
- **Dot circles**: Positioned absolutely to overlap the vertical line
- Used with `left: 50%` + `transform: translateX(-50%)` for perfect centering

### Relative Positioning
- **Timeline items**: Creates stacking context without removing from document flow
- **Timeline marker**: Acts as positioned ancestor for absolute children

### CSS Grid
- **Two-column layout**: 60px for marker area + flexible space for content
- Clean alignment without unnecessary wrappers

### Flexbox
- **Body**: Centers the entire container
- **Timeline marker**: Aligns dot vertically

---

## Responsive Design Strategy

The component uses mobile-first responsive breakpoints:

```css
/* Tablets and below */
@media (max-width: 600px) {
    .timeline-item {
        grid-template-columns: 50px 1fr; /* Narrower marker area */
    }
}

/* Small phones */
@media (max-width: 400px) {
    .timeline-item {
        grid-gap: 12px; /* Tighter spacing */
    }
}
```

---

## Layer Composition Diagram

```
┌─────────────────────────────────────────┐
│ z-index: 10  [Header]                   │
│                                         │
├─────────────────────────────────────────┤
│ z-index: 4   [Dot] ●                    │
│ z-index: 2   [Content] Changelog entry  │
│ z-index: 0   [Line] ─────────────────── │
│                                         │
│ z-index: 4   [Dot] ●                    │
│ z-index: 2   [Content] Another entry    │
└─────────────────────────────────────────┘
│ z-index: 10  [Button]                   │
└─────────────────────────────────────────┘
```

---

## How It Works Step-by-Step

1. **Timeline Container** creates the context for absolute positioning
2. **::before pseudo-element** renders the vertical line (z-index: 0)
3. **Timeline Items** are laid out with Grid, positioned relatively (z-index: 2)
4. **Timeline Marker** divs act as containers for dots (z-index: 3)
5. **Timeline Dots** positioned absolutely centered on the line (z-index: 4)
6. **Content** displays alongside markers, appearing between the line and dots

---

## Key CSS Properties Explained

| Property | Purpose |
|----------|---------|
| `position: absolute` | Remove from flow, position relative to positioned ancestor |
| `position: relative` | Keep in flow, establish stacking context, allow absolute children |
| `z-index` | Control stacking order (higher = on top) |
| `left: 50%` | Move element 50% from the left edge |
| `transform: translateX(-50%)` | Move back 50% of own width for centering |
| `grid-template-columns` | Define column widths for grid layout |
| `::before`/`::after` | Pseudo-elements for decorative lines without extra HTML |

---

## Customization Tips

### Change Timeline Color
```css
.timeline::before {
    background: #your-color;
}

.timeline-dot {
    border-color: #your-color;
}

.timeline-dot:hover {
    background: #your-color;
}
```

### Adjust Timeline Width/Alignment
```css
.timeline-item {
    grid-template-columns: 80px 1fr; /* Change marker area width */
}

.timeline::before {
    left: 27px; /* Adjust line position to match new marker width */
}
```

### Add Icons Instead of Dots
Replace `.timeline-dot` with an icon element and adjust positioning accordingly.

---

## Performance Considerations

- ✅ Uses CSS pseudo-elements instead of extra HTML elements
- ✅ Minimal layout shifts with proper sizing
- ✅ GPU-accelerated transforms for smooth hover effects
- ✅ No JavaScript required
- ✅ Mobile-optimized with appropriate touch targets (36px minimum dot size)

