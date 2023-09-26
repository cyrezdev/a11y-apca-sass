<h1 align="center">A11y APCA Sass Library</h1>
<p align="center">
    <a href="https://github.com/Myndex/apca-w3#version-information"><img alt="Static Badge" src="https://img.shields.io/badge/APCA_W3-0.1.9_(w3)_(98G4g)_beta-blueviolet"></a>
    <img alt="Static Badge" src="https://img.shields.io/badge/WCAG_3.0-beta-darkred">
    <img alt="Static Badge" src="https://img.shields.io/badge/Dart_Sass-since_1.33-mediumvioletred">
    <a href="https://github.com/cyrezdev/a11y-apca-sass/blob/main/LICENSE"><img alt="A11y APCA Sass license" src="https://img.shields.io/badge/license-MIT-blue" /></a>
</p>

<p align="center">
    <b>Sass tools library to generate accessible colors with <a href="https://git.apcacontrast.com/">APCA™</a> contrast algorithm.</b>
</p>

# Introduction
 
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
<br>

## Glossary

| _Term_ | _Definition_ |
| :---   | :--- |
| bg     | Background |
| fg     | Foreground |
| Lc     | Lc contrast value [-108,106] |
| level  | Accessibility level as defined by APCA-W3 and WCAG 3: Bronze (minimum), Silver, Gold (higher). |
| Lnp    | Absolute (non-polar) Lc value $L^C_{np}$ |
| size   | Size of the font in px used without unit.<br> The reference fonts are Helvetica Neue, Helvetica, Fira Sans, Kanit, or Arial.<br> For other font-family, visit APCA/ARC Support Resources: [Determine Size Offset](https://readtech.org/ARC/tests/visual-readability-contrast/?tn=methods#i---size) |
| weight | Weight of the font.<br> The reference fonts are Helvetica or Arial.<br> For other font-family, visit APCA/ARC Support Resources: [Determine Weight Offset](https://readtech.org/ARC/tests/visual-readability-contrast/?tn=methods#ii---weight) |

<br>

# Getting Started

Generates an accessible color automatically.

## Default Fallback Parameters

```scss
$fallback-a11y-map: (
	"level": "silver",
	"content": "body-text",
	"size": 16px,
	"weight": 400,
	"edit": "fg",
	"adjust": 0
);
```
<br>

## Using `a11y-color()` to generate an accessible color automatically

Calculate the accessible color relative to another color, accessibility level, content use case, text font size and weight, using the APCA contrast algorithm.

_Note: The returned color uses the minimum contrast per use case and level. You can increase contrast with "adjust" parameter._

<br>

### Usage

```html
a11y-color( <foreground color>, <background color>, (<parameters map>) )
```
<br>

### Parameters
Set at least one color (foreground or background). Use a valid Sass color (eg. #f4f5f3, #234, pink, darkblue, etc.)

Set **use case parameters** when different from default $fallback-a11y-map:
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
<br>

### Examples

Simple use case with default parameters:
```scss
$color: a11y-color(#0000ff, #f9f9f9);

// Lc contrast for #0000ff on #f9f9f9 is 82.2
// a11y-color returns #0000c2 as foreground color
// with a Lc contrast of 90.2 on #f9f9f9 background color.
```

Simple use case with some specific parameters:
```scss
$color: a11y-color(#1234b0, #e9e4d0, ("content": "sub-fluent-text", "size": .875rem, "weight": 500));

// Lc contrast for #1234b0 on #e9e4d0 is 75.6
// a11y-color returns #0b1f69 as foreground color
// with a Lc contrast of 85.0 on #e9e4d0 background color.
```

Get accessible color with default parameters:
```scss
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
```scss
@use "a11y-apca-sass/a11y-apca";

header {
  --text-color: #181807;
  --size: 1.125rem;
  --weight: 600;
  font-size: var(--size);
  font-weight: var(--weight);
  color: var(--text-color);
  background-color: a11y-color(var(--text-color), "", ("size": var(--size), "weight": var(--weight), "edit": "bg"));
}
```

Get accessible color for a sub-fluent text:
```scss
@use "a11y-apca-sass/a11y-apca";

footer {
  --background-color: #111;
  --size: 14px;
  font-size: var(--size);
  font-weight: 400;
  color: a11y-color(#f3f3f3, var(--background-color), ("content": "sub-fluent-text", "size": var(--size));
  background-color: var(--background-color);
}
```

<br>

# Utility Functions

## APCA based Functions

### `a11y-lnp()`
Get Lnp (Lc in non-polar mode)

```scss
@function a11y-lnp($level: "silver", $content: "body-text", $size: 16, $weight: 400,
  $bg-color: "", $adjust-contrast: 0)
```
- Method to get the absolute Lc contrast value relatively to the standards of the selected accessibility level.

### `font-to-contrast-table()`
APCA Font to Contrast table

```scss
@function font-to-contrast-table($size, $weight)
```
- Return absolute Lc contrast (Lnp) required per Font Lookup Table.<br>
  @see https://github.com/Myndex/apca-w3/tree/master#font-lookup-table

### `apca-color()`
Calculate the color relative to another color, using the APCA contrast algorithm.

```scss
@function apca-color($fg-color, $contrast: 90, $bg-color: $fg-color, $edit: "fg")
```
- Given the Lnp contrast value (Lc non-polar, $contrast: Lnp),<br>
  returns the accessible color for the foreground (Text / Icon) on the background color<br>
  OR the accessible color for the background relative to the foreground color.

### `apca-contrast()`
APCA contrast

```scss
@function apca-contrast($fg-color, $bg-color)
```
- Determine the lightness contrast as an Lc value from Lc 0 to Lc 106 for dark text on a light background, and Lc 0 to Lc -108 for light text on a dark background, using the APCA Contrast Algorithm.

### `apca-polarity()`
APCA Polarity

```scss
@function apca-polarity($fg-color, $bg-color)
```
- Use Y (luminance) for text and BG, and soft clamp black, return 0 for very close luminances, determine
   polarity, and calculate SAPC raw contrast. Then scale for easy to remember levels.

### `apca-soft-clamp()`
APCA Soft Clamp

```scss
@function apca-soft-clamp($luminance)
```
- Soft clamps Y (luminance) for either color if it is near black.

### `apca-luminance()`
APCA Luminance

```scss
@function apca-luminance($color)
```
- Linearize r, g, or b then apply coefficients and sum then return the resulting luminance
<br>

## Helper Functions

### `set-a11y-map()`
```scss
@function set-a11y-map($map)
```
- Validate the list of specific parameters and merge with default $fallback-a11y-map.

### `validate-font-size()`
```scss
@function validate-font-size($size)
```
- Depending on the unit, recalculate a font size value into pixels if possible.

### `strip-unit()`
```scss
@function strip-unit($number)
```
- Remove the unit of a length.

### `is-light-or-dark()`
```scss
@function is-light-or-dark($color)
```
- Determine if a color brightness is "light" or "dark" by checking lightness contrast against white or black.

### `get-lnp()`
```scss
@function get-lnp($fg-color, $bg-color)
```
- Shortcut function to get Lnp, the non-polar value of Lc (lightness contrast).
<br>

# Acknowledgments and resources

- APCA homepage: https://git.apcacontrast.com/
- APCA calculator: https://www.myndex.com/APCA/
- APCA-W3: https://github.com/Myndex/apca-w3
- Implementation of APCA algorithm adapted from [sass-apca](https://github.com/gfellerph/sass-apca) by [Philipp Gfeller](https://github.com/gfellerph).
- apca-color() function inspired by [J. Hogue](https://github.com/jhogue)’s [automated-a11y-sass](https://github.com/jhogue/automated-a11y-sass), based on WCAG2
