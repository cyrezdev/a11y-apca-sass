# A11y APCA Sass Library
 
Sass tools library to generate colors with [APCA™](https://git.apcacontrast.com/) color contrast algorithm.

## About a11y-apca-sass Library
- APCA™ is the candidate contrast method for WCAG 3, and is currently in [public beta](https://github.com/Myndex/apca-w3).
- This Sass library a11y-apca-sass is in beta for the same reason:
  still in development and subject to changes prior to adoption.
- The code of the a11y-apca-sass could be simplified (some lines are not needed or redundant).
  This is wanted, to keep the current development clear, detailled and
  easier to stay up-to-date when changes are required or things should be adjusted.
  All use cases are to be displayed in Beta process, even if step or code with no effect.
  When we will reach a version 1.0.0 ready for standards in production, we will lighter the code.

## About APCA™ by Myndex Perception Research.
Link: https://git.apcacontrast.com/
- APCA™ is the Accessible Perceptual Contrast Algorithm, a new way to predict contrast for text and non-text content on self illuminated displays.
- APCA™ is the candidate contrast method for WCAG 3, and is currently in public beta.
- APCA™ is a contrast assessment method for predicting the perceived contrast between sRGB colors on a computer monitor. It has been developed as an assessment method for W3 Silver/WCAG3 accessibility standards relating to content for computer displays and mobile devices, with a focus on readability and understandability.
- WCAG 3 is still in development and subject to changes prior to adoption.

### CURRENT BASE ALGORITHM:
APCA Contrast Prediction Equation 0.0.98G-4g-base-W3<br>
https://github.com/Myndex/SAPC-APCA/blob/master/documentation/APCA-W3-LaTeX.md#latex-of-the-apca-w3-base-formula<br>

### DISCLAIMER:
>“APCA is a method for predicting text contrast on self-illuminated displays for web-based content.
>Some use-cases are prohibited by license, including the following: use in medical, clinical evaluation,
>human safety related, aerospace, transportation, automotive, military applications, are strictly
>prohibited without a specific license in writing granting such use.”

<hr>

# Default Fallback Parameters

```
$fallback-a11y-map: (
	"level": "silver",
	"content": "body-text",
	"size": 16px,
	"weight": 400,
	"edit": "fg",
	"adjust": 0
);
```

# Get Started

## `a11y-color()`
Generates an accessible color automatically.

Calculate the accessible color relative to another color, accessibility level ("bronze", "silver", "gold"), use case ("small-body-text", "body-text", "fluent-text", "large-text", "sub-fluent-text", "spot", "non-text"), text font size and weight, using the APCA contrast algorithm.

### Usage

```
a11y-color( <foreground color>, <background color>, (<parameters map>) )
```

### Parameters
Set at least one color (foreground or background). Use a valid Sass color (eg. #f4f5f3, #234, pink, darkblue, etc.)

Set use case parameters when different from default $fallback-a11y-map:
- `"level": "{string}"`:
  - **Accessibility level as defined by APCA-W3 and WCAG 3:**
    - "bronze" (minimum)
    - "silver"
    - "gold" (higher).
- `"content": "{string}"`
  - **The type of content:**
    - "small-body-text"
    - "body-text"
    - "fluent-text"
    - "large-text"
    - "sub-fluent-text"
    - "spot"
    - "non-text"
- `"edit": "{string}"`
  - **The color to be edited:**
    - "fg" if color to be edited is Text / Icon color (foreground).<br>
    - "bg" if color to be edited is background color.
- `"size": {number}`
  - **Size of the font in rem, em, px (preferred) or unitless (supposed px).**<br>
    Warning: 'rem' and 'em' suppose the parent font-size to be 16px (the browser’s default root font-size is typically 16px).
    - eg. 1rem, 1em, 16px, 16
- `"weight": {number}`
  - **Weight (or boldness) of the font.**<br>
    The weights available depend on the font-family that is currently set.
    - Numeric value [1,1000]
- `"adjust": {number}`
  - **Positive value to increase lightness contrast.**<br>
    If it exceed limits defined by the a11y level, it will be auto-adjusted.

### Examples

Simple case using default parameters:
```
a11y-color(#1234b0, #e9e4d0)
```

Simple case with some specific parameters:
```
a11y-color(#1234b0, #e9e4d0, ("content": "sub-fluent-text", "size": .875rem, "weight": 500))
```

Get accessible color with default parameters:
```
@use "a11y-apca-sass/a11y-apca";

body {
  --background-color: #f5f5f5;
  font-size: 16px;
  font-weight: 400;
  color: a11y-color(#111111, var(--background-color));
  background-color: var(--background-color);
}
```

Get accessible background color to be used with text color and background color not set (will use text color to get bg-color):
```
header {
  --text-color: #181807;
  --size: 1.125rem;
  --weight: 600;
  font-size: var(--size);
  font-weight: var(--weight);
  color: var(--text-color);
  background-color: a11y-color(var(--text-color), "", ("size": var(--size), "weight": var(--weight), "edit": "bg");
}
```

Get accessible color for a sub-fluent text:
```
footer {
  --background-color: #111;
  --size: 14px;
  font-size: var(--size);
  font-weight: 400;
  color: a11y-color(#f3f3f3, var(--background-color), ("content": "sub-fluent-text", "size": var(--size));
  background-color: var(--background-color);
}
```

<hr>

# Utility Functions

## APCA based Functions

### `a11y-lnp()`
Get Lnp (Lc in non-polar mode)

`@function a11y-lnp($level: "silver", $content: "body-text", $size: 16, $weight: 400, $bg-color: "", $adjust-contrast: 0)`
- Method to get the absolute Lc contrast value relatively to the standards of the selected accessibility level.

### `font-to-contrast-table()`
APCA Font to Contrast table

`@function font-to-contrast-table($size, $weight)`
- Return absolute Lc contrast (Lnp) required per Font Lookup Table.<br>
  @see https://github.com/Myndex/apca-w3/tree/master#font-lookup-table

### `apca-color()`
Calculate the accessible color relative to another color, using the APCA contrast algorithm.

`@function apca-color($fg-color, $contrast: 90, $bg-color: $fg-color, $edit: "fg")`
- Return the accessible color for the foreground (Text / Icon) on the background color<br>
  OR the accessible color for the background relative to the foreground color.

### `apca-contrast()`
APCA contrast

`@function apca-contrast($fg-color, $bg-color)`
- Determine the lightness contrast of a given text and background color using the APCA Contrast Algorithm

### `apca-polarity()`
APCA Polarity

`@function apca-polarity($fg-color, $bg-color)`
- Use Y (luminance) for text and BG, and soft clamp black, return 0 for very close luminances, determine
   polarity, and calculate SAPC raw contrast. Then scale for easy to remember levels.

### `apca-soft-clamp()`
APCA Soft Clamp

`@function apca-soft-clamp($luminance)`
- Soft clamps Y (luminance) for either color if it is near black.

### `apca-luminance()`
APCA Luminance

`@function apca-luminance($color)`
- Linearize r, g, or b then apply coefficients and sum then return the resulting luminance
<br>

## Helper Functions

### `set-a11y-map`
`@function set-a11y-map($map)`
- Validate the list of specific parameters and merge with default $fallback-a11y-map.

### `validate-font-size`
`@function validate-font-size($size)`
- Depending on the unit, recalculate a font size value into pixels if possible.

### `strip-unit`
`@function strip-unit($number)`
- Remove the unit of a length.

### `is-light-or-dark`
`@function is-light-or-dark($color)`
- Determine if a color brightness is "light" or "dark" by checking lightness contrast against white or black.

### `get-lnp`
`@function get-lnp($fg-color, $bg-color)`
- Shortcut function to get Lnp, the non-polar value of Lc (lightness contrast).

# Acknowledgments and resources

- APCA homepage: https://git.apcacontrast.com/
- APCA calculator: https://www.myndex.com/APCA/
- APCA-W3: https://github.com/Myndex/apca-w3
- Implementation of APCA algorithm adapted from [sass-apca](https://github.com/gfellerph/sass-apca) by [Philipp Gfeller](https://github.com/gfellerph).
- apca-color() function inspired by [J. Hogue](https://github.com/jhogue)’s [automated-a11y-sass](https://github.com/jhogue/automated-a11y-sass), based on WCAG2
