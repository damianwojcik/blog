---
title: Flexbox
date: 2018-07-12 07:53:11
tags: #flexbox #css #rwd
---
## FLEXBOX FEATURES
- Flexible and efficient layouts
- Distribute space among items
- Easy vertical and horizontal centering
- Control items alignment
- Chaning order of items
- Cross-browser
- Used by Bootstrap 4

## TERMINOLOGY
![Flexbox Terminology](terminology.jpg)
- Flex Container
- Flex Items
- Main Axis (Main start -> Main end) - Horizontal
- Cross Axis (Cross start -> Cross end) - Vertical

## FLEX CONTAINER
**Properties**:
- `display`
- `flex-direction`
- `flex-wrap`
- `flex-flow`
- `justify-content`
- `align-items`
- `align-content`

### DISPLAY
Used to create a block or inline Flex Container.
- `flex` - takes full width (acts like a block element)
- `inline-flex` - takes only width needed by its children (acts like a inline-block element)

### FLEX-DIRECTION
Sets the direction of the main axis.
- `row` - (default) left to right
- `row-reverse` - right to left
- `column` - top to bottom
- `column-reverse` - bottom to top

### FLEX-WRAP
By default, flex items will all try to fit onto one line. You can change that and allow the items to wrap as needed with this property.
- `nowrap` - (default)
- `wrap` - wrap to next row or next column
- `wrap-reverse` - wrap to previuos row or previous column

### FLEX-FLOW
Shorthand for _flex-direction_ and _flex-wrap_.
``` css
.container {
    flex-flow: <flex-direction> <flex-wrap>;
}
```

### JUSTIFY-CONTENT
Align Flex Items and distribute any extra spacing in the parent Flex Container.
The alignment is always along the **main axis**.
- `flex-start` - (default)
- `flex-end` - reverse
- `center`
- `space-around`
- `space-between`
- `space-evenly`

![Flexbox justify-content](justify-content.jpg)

### ALIGN-ITEMS
Align Flex Items along the **cross axis**.
- `stretch` - (default)
- `flex-start`
- `flex-end`
- `center`
- `baseline`

![Flexbox align-items](align-items.jpg)

### ALIGN-CONTENT
Align lines of Flex Items along the **cross axis** and distribute any extra spacing in the parent Flex Container.
Works only if there is more than one line of Flex Items.
- `stretch` - (default)
- `flex-start`
- `flex-end`
- `center`
- `space-between`
- `space-around`

![Flexbox align-content](align-content.jpg)

## FLEX ITEMS
**Properties**:
- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

### ORDER
Default: `0` (int)
Controls the order of Flex Item in the Flex Container.

### FLEX-GROW
Default: `0`
Defines what amount of abailable space inside the Flex Container the Flex Item should take.
This magnitude is relative to other Flex Itmes in the Flex Container.
If set to 1 for all items, then all items grow evenly (till reach full width of the Flex Container)

### FLEX-SHRINK
Default: `1`
Defines the shrink factor of the Flex Items, when the default size of Flex Items is larger than width of the Flex Container.
This magnitude is relative to other Flex Itmes in the Flex Container.

### FLEX-BASIS
Default: `auto`
Set the initial size of a Flex Items.
Works similiar to `width` but is more _Flexbox-like-syntax_
Takes values such as pixels, percentages or relative units.

### FLEX
Default: `0 1 auto`
Combination of **flex-grow**, **flex-shrink** and **flex-basis**.
``` css
.flex-item {
    flex: <flex-grow> <flex-shrink> <flex-basis>;
    flex: <flex-grow> <flex-basis>;
    flex: <flex-grow>;
}
```

### ALIGN-SELF
Default: `auto`
Overrides the `align-items` value of the Flex Container
Possible values: `auto | flex-start | flex-end | center | stretch`
<br>

---

**SOURCES:**

<https://flexbox.io/>
<https://css-tricks.com/snippets/css/a-guide-to-flexbox/>
<https://www.youtube.com/watch?v=z6tJ5ngiF14>