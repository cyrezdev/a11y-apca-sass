<h1 align="center">A11y APCA Sass Library</h1>

<p align="center">
    <b>Sass tools library to generate accessible colors with <a href="https://git.apcacontrast.com/">APCA™</a> contrast algorithm.</b>
</p>

<p align="center">
	<a href="https://github.com/Myndex/apca-w3#version-information"><img alt="APCA-W3 Version" src="https://img.shields.io/badge/APCA_W3-0.1.9_(w3)_(98G4g)_beta-blueviolet?style=for-the-badge"></a>
	<img alt="WCAG 3.0 Status" src="https://img.shields.io/badge/WCAG_3.0-candidate-darkred?style=for-the-badge">
	<img alt="Dart Sass minimum version" src="https://img.shields.io/badge/Dart_Sass-since_1.33-mediumvioletred?style=for-the-badge"><br>
	<img alt="A11y APCA Sass Library version" src="https://img.shields.io/badge/version-0.1.0 beta-darkgreen?style=for-the-badge">
	<a href="https://github.com/cyrezdev/a11y-apca-sass/blob/main/LICENSE"><img alt="A11y APCA Sass license" src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" /></a>
	<!--img alt="Downloads" src="https://img.shields.io/github/downloads/cyrezdev/a11y-apca-sass/total"-->
</p>

<details>
<summary>Table of Contents</summary>
<br />

- [Introduction](#introduction)
  - [About a11y-apca-sass Library](#about-a11y-apca-sass-library)
  - [About APCA™ by Myndex Perception Research](#about-apca-by-myndex-perception-research)
    - [CURRENT BASE ALGORITHM](#current-base-algorithm)
    - [DISCLAIMER](#disclaimer)
  - [Glossary](#glossary)
  - [Conformance Levels](#conformance-levels)
    - [Bronze](#bronze)
    - [Silver](#silver)
    - [Gold](#gold)
- [Getting started](#getting-started)
  - [Default Fallback Parameters](#default-fallback-parameters)
  - [Using `a11y-color()` to generate an accessible color automatically](#using-a11y-color-to-generate-an-accessible-color-automatically)
    - [Usage](#usage)
    - [Color value](#color-value)
    - [Parameters](#parameters)
    - [Warning messages](#warning-messages)
    - [Examples](#examples)
- [Utility Functions](#utility-functions)
  - [APCA based Functions](#apca-based-functions)
    - [`get-minimum-absolute-lc()`](#get-minimum-absolute-lc)
    - [`font-to-contrast-table()`](#font-to-contrast-table)
    - [`apca-color()`](#apca-color)
    - [`apca-contrast()`](#apca-contrast)
    - [`apca-polarity()`](#apca-polarity)
    - [`apca-soft-clamp()`](#apca-soft-clamp)
    - [`apca-luminance()`](#apca-luminance)
  - [Helper Functions](#helper-functions)
    - [`set-a11y-map()`](#set-a11y-map)
    - [`validate-font-size()`](#validate-font-size)
    - [`strip-unit()`](#strip-unit)
    - [`is-light-or-dark()`](#is-light-or-dark)
    - [`absolute-lc()`](#absolute-lc)
- [Acknowledgments and resources](#acknowledgments-and-resources)
- [License](#license)

</details>

<br>

## Introduction
 
### About a11y-apca-sass Library
- A11y-APCA-Sass is an APCA™ based library to generate accessible colors for the web.
- This Sass library is still in development and subject to changes prior to adoption.
- The code of the a11y-apca-sass could be simplified (some lines are not needed or redundant).
  This is wanted, to keep the current development clear, detailled and
  easier to stay up-to-date when changes are required or things should be adjusted.
  All use cases are to be displayed in Beta process, even if step or code with no effect.
  When we will reach a version 1.0.0 ready for standards in production, we will lighter the code.

### About APCA™ by Myndex Perception Research
GitHub: https://github.com/Myndex<br>
Link: https://git.apcacontrast.com/
- APCA™ is the Accessible Perceptual Contrast Algorithm, a new way to predict contrast for text and non-text content on self illuminated displays.
- APCA™ is the candidate contrast method for WCAG 3, and is currently in [public beta](https://github.com/Myndex/apca-w3).
- APCA™ is a contrast assessment method for predicting the perceived contrast between sRGB colors on a computer monitor. It has been developed as an assessment method for W3 Silver/WCAG3 accessibility standards relating to content for computer displays and mobile devices, with a focus on readability and understandability.
- WCAG 3 is still in development and subject to changes prior to adoption.

#### CURRENT BASE ALGORITHM:
APCA Contrast Prediction Equation 0.0.98G-4g-base-W3<br>
https://github.com/Myndex/SAPC-APCA/blob/master/documentation/APCA-W3-LaTeX.md#latex-of-the-apca-w3-base-formula<br>

#### DISCLAIMER:
>“APCA is a method for predicting text contrast on self-illuminated displays for web-based content.
>Some use-cases are prohibited by license, including the following: use in medical, clinical evaluation,
>human safety related, aerospace, transportation, automotive, military applications, are strictly
>prohibited without a specific license in writing granting such use.”
<br>

### Glossary

| _Term_ | _Definition_ |
| :---   | :--- |
| absLc  | Absolute Lc value, abs(Lc) |
| bg     | Background |
| fg     | Foreground |
| Lc     | Lc contrast value [-108,106] |
| level  | Accessibility level as defined by APC-RC: Bronze (minimum), Silver, Gold (higher). |
| size   | Size of the font in rem, em, px (preferred) or unitless (supposed px).<br> Warning: 'rem' and 'em' suppose the root or parent font-size to be 16px (typically the browser's default font size).<br> The reference fonts are Helvetica Neue, Helvetica, Fira Sans, Kanit, or Arial.<br> For other font-family, visit APCA/ARC Support Resources: [Determine Size Offset](https://readtech.org/ARC/tests/visual-readability-contrast/?tn=methods#i---size) |
| weight | Weight of the font.<br> The reference fonts are Helvetica or Arial.<br> For other font-family, visit APCA/ARC Support Resources: [Determine Weight Offset](https://readtech.org/ARC/tests/visual-readability-contrast/?tn=methods#ii---weight) |

<br>

### Conformance levels

#### Bronze
**Covers primary content text only.**

Bronze is the minimum conformance level. Content that does not meet the requirements of the bronze level fails to conform to ARC.<br>
[Read more about Bronze level conformance](https://readtech.org/ARC/tests/visual-readability-contrast/?tn=criterion#bronze-level-conformance)

#### Silver
**Covers all text content.**

Silver is a higher conformance level, featuring improved readability and accommodation of user preferences.<br>
[Read more about Silver level conformance](https://readtech.org/ARC/tests/visual-readability-contrast/?tn=criterion#silver-level-conformance)

#### Gold
**Covers all text content, enhanced.**

Gold is the highest conformance level, featuring the improvements from Silver, but adding tests to ensure consistent presentation, such as regulating font x-height and font weight relative to defined reference fonts.<br>
[Read more about Gold level conformance](https://readtech.org/ARC/tests/visual-readability-contrast/?tn=criterion#gold-level-conformance)


<br>

## Getting Started

Generates an accessible color automatically.

### Default Fallback Parameters

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

### Using `a11y-color()` to generate an accessible color automatically

Calculate the accessible color relative to another color, accessibility level, content use case, text font size and weight, using the APCA contrast algorithm.

If `a11y-color()` can not generate an accessible color, it returns the best possible color and prints a [warning message](#warning-messages) for the user, along with a stack trace indicating how the current function was called.

_Note: The returned color uses the minimum contrast per use case and level. You can increase contrast with "adjust" parameter._

<br>

#### Usage

```html
a11y-color( <foreground color>, <background color>, (<parameters map>) )
```
<br>

#### Color value

A string containing a valid CSS color string, including hexadecimal or functional representations.<br>
For example the color MidnightBlue can be specified as:
- `MidnightBlue`
- `#191970`
- `rgb(25, 25, 112)`
- `hsl(240, 64%, 27%)`

_Note: a11y-apca-sass does not support opacity yet._

<br>

#### Parameters
Set at least one color (foreground or background).

Set **use case parameters** when different from default $fallback-a11y-map:
- `"level": "{string}"`:
  - **Accessibility level as defined by APCA-W3 and WCAG 3:**
    - "bronze" (minimum)
    - "silver"
    - "gold" (higher)
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

#### Warning messages

A11y APCA Sass provides warning messages when an accessible color can't be generated per use case.

For example, you want to get the text color for a background color Crimson `#dc143c` (dark color).

As it is your body background color, and you want the same hue for body-text color, you use the short a11y-color version:

```scss
$color: a11y-color(#dc143c);
```

With only one parameter, it uses the color as background, and search for the foreground color with the minimum contrast required (using [default fallback parameters](#default-fallback-parameters), which are level Silver, body-text of font-size 16px and weight 400 (normal)).

In this case, a minimum contrast of Lc 90 (in absolute value) is expected for "Silver" level ([APCA Font to contrast table](https://github.com/Myndex/apca-w3#font-lookup-table)).

When watching Sass, it prints a `@warn` message for you:

```console
Warning:
Background color #dc143c is not dark enough. white is returned as color for the foreground (text / icon), but it does not conform to the desired Lc contrast value.
```

The best contrast possible with a background color Crimson `#dc143c` is `Lc -77.6` with `white` as foreground color. It fails to conform.

You need then to adjust the background color, or increase the font-size and/or weight.

Increasing a bit the font-size to 18 px is fixing it:
```scss
$color: a11y-color(#dc143c, #dc143c, ("size": 18px));

// a11y-color returns #fffafb as foreground color
// with a Lc contrast of -75.1 on #dc143c background color, for a body text with a font-size of 18px.
```
<br>

#### Examples

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

## Utility Functions

### APCA based Functions

#### `get-minimum-absolute-lc()`
Get the minimum absolute Lc (absLc) as defined by APC-RC.

```scss
@function get-minimum-absolute-lc($level: "silver", $content: "body-text", $size: 16, $weight: 400,
  $bg-color: "", $adjust-contrast: 0)
```
- Method to get the absolute Lc contrast value relatively to the standards of the selected accessibility level.

#### `font-to-contrast-table()`
APCA Font to Contrast table

```scss
@function font-to-contrast-table($size, $weight)
```
- Return absolute Lc contrast (absLc) required per Font Lookup Table.<br>
  @see https://github.com/Myndex/apca-w3/tree/master#font-lookup-table

#### `apca-color()`
Calculate the color relative to another color, using the APCA contrast algorithm.

```scss
@function apca-color($fg-color, $contrast: 90, $bg-color: $fg-color, $edit: "fg")
```
- Given the absLc contrast value (absolute Lc, $contrast: absLc),<br>
  returns the accessible color for the foreground (Text / Icon) on the background color<br>
  OR the accessible color for the background relative to the foreground color.

#### `apca-contrast()`
APCA contrast

```scss
@function apca-contrast($fg-color, $bg-color)
```
- Determine the lightness contrast as an Lc value from Lc 0 to Lc 106 for dark text on a light background, and Lc 0 to Lc -108 for light text on a dark background, using the APCA Contrast Algorithm.

#### `apca-polarity()`
APCA Polarity

```scss
@function apca-polarity($fg-color, $bg-color)
```
- Use Y (luminance) for text and BG, and soft clamp black, return 0 for very close luminances, determine
   polarity, and calculate SAPC raw contrast. Then scale for easy to remember levels.

#### `apca-soft-clamp()`
APCA Soft Clamp

```scss
@function apca-soft-clamp($luminance)
```
- Soft clamps Y (luminance) for either color if it is near black.

#### `apca-luminance()`
APCA Luminance

```scss
@function apca-luminance($color)
```
- Linearize r, g, or b then apply coefficients and sum then return the resulting luminance
<br>

### Helper Functions

#### `set-a11y-map()`
```scss
@function set-a11y-map($map)
```
- Validate the list of specific parameters and merge with default $fallback-a11y-map.

#### `validate-font-size()`
```scss
@function validate-font-size($size)
```
- Depending on the unit, recalculate a font size value into pixels if possible.

#### `strip-unit()`
```scss
@function strip-unit($number)
```
- Remove the unit of a length.

#### `is-light-or-dark()`
```scss
@function is-light-or-dark($color)
```
- Determine if a color brightness is "light" or "dark" by checking lightness contrast against white or black.

#### `absolute-lc()`
```scss
@function absolute-lc($fg-color, $bg-color)
```
- Shortcut function to get absLc, the absolute value of Lc (lightness contrast).
<br>

## Acknowledgments and resources

- APCA homepage: https://git.apcacontrast.com/
- APCA calculator: https://www.myndex.com/APCA/
- APCA-W3: https://github.com/Myndex/apca-w3
- Implementation of APCA algorithm adapted from [sass-apca](https://github.com/gfellerph/sass-apca) by [Philipp Gfeller](https://github.com/gfellerph).
- apca-color() function inspired by [J. Hogue](https://github.com/jhogue)’s [automated-a11y-sass](https://github.com/jhogue/automated-a11y-sass), based on WCAG2

## License

Code copyright 2023 [Cyril Reze](https://cyrez.dev). Code released under the [MIT License](https://github.com/cyrezdev/a11y-apca-sass/blob/main/LICENSE).
