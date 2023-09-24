# A11y APCA Sass Library
 
Sass tools library to generate colors with [APCA™](https://git.apcacontrast.com/) color contrast algorithm.

## About a11y-apca-sass Library
- APCA™ is the candidate contrast method for WCAG 3, and is currently in public beta.
- This Sass library a11y-apca-sass is in beta for the same reason:
  still in development and subject to changes prior to adoption.
- The code of the a11y-apca-sass could be simplified (some lines are not needed or redundant).
  This is wanted, to keep the current development clear, detailled and
  easier to stay up-to-date when changes are required or things should be adjusted.
  All use cases are to be displayed in Beta process, even if step or code with no effect.
  When we will reach a version 1.0.0 ready for standards in production, we will lighter the code.

## About APCA™ by Myndex Perception Research. https://git.apcacontrast.com/
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

# Main Functions

## `a11y-color()`
Generates an accessible color automatically.

Calculate the accessible color relative to another color, accessibility level ("bronze", "silver", "gold"), use case ("small-body-text", "body-text", "fluent-text", "large-text", "sub-fluent-text", "spot", "non-text"), text font size and weight, using the APCA contrast algorithm.

### Usage

a11y-color( <code>foreground color</code>, <code>background color</code>, <code>parameters</code> )

### Parameters
Set at least one color (foreground or background).

Set use case parameters when different from default $fallback-a11y-map:
- `"level": {string}`: **Accessibility level as defined by APCA-W3 and WCAG 3:**
  - Bronze (minimum)
  - Silver
  - Gold (higher).
- `"content": {string}`<br>
  **The type of content:**
  - small-body-text
  - body-text
  - fluent-text
  - large-texts
  - sub-fluent-text
  - spot
  - non-text
- `"edit": {string}'<br>
**The color to be edited**
  - "fg" if color to be edited is Text / Icon color (foreground).<br>
  - "bg" if color to be edited is background color.
- `"size": {number}`<br>
**Size of the font in rem, em, px (preferred) or unitless (supposed px)**
  - Warning: 'rem' and 'em' suppose the parent font-size to be 16px (the browser’s default root font-size is typically 16px).
- `"weight": {number}`<br>
**Weight (or boldness) of the font.**
  - The weights available depend on the font-family that is currently set.<br>
  - Accepts: Numeric values [1,1000]
- `"adjust": {number}`<br>
**Positive or negative value to increase or reduce lightness contrast.**
  - If it exceed limits defined by the a11y level, it will be auto-adjusted.

### Examples

```
@use "a11y-apca-sass/a11y-apca";

body {
  --background-color: #181818;
  font-size: 16px;
  font-weight: 400;
  color: a11y-color(#f3f3f3, var(--background-color), ("adjust": 15);
  background-color: var(--background-color);
}
header {
  --text-color: #181818;
  font-size: 18px;
  font-weight: 600;
  color: var(--text-color);
  background-color: a11y-color(#f3f3f3, var(--background-color), ("size": 18px, "weight": 600, "edit": "bg");
}
footer {
  --background-color: #222222;
  font-size: 14px;
  font-weight: 400;
  color: a11y-color(#f3f3f3, var(--background-color), ("content": "sub-fluent-text", "size": 14px);
  background-color: var(--background-color);
}
```

<hr>

# APCA Functions

## `a11y-lnp()`
## `font-to-contrast-table()`
## `apca-color()`
## `apca-contrast()`
## `apca-polarity()`
## `apca-soft-clamp()`
## `apca-luminance()`


<hr>

# Utility Functions

## `set-a11y-map`
## `validate-font-size`
## `strip-unit`
## `is-light-or-dark`
## `get-lnp`

# Inspiration and resources

- APCA homepage: https://git.apcacontrast.com/
- APCA calculator: https://www.myndex.com/APCA/
- APCA-W3: https://github.com/Myndex/apca-w3
- SASS-APCA: https://github.com/gfellerph/sass-apca
