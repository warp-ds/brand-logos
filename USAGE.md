# Brand Logo Usage

Use this repository as the source of truth for Warp brand logos. The SVG files in `logos/` are the approved master assets, and the published Eik CDN package is the delivery channel for web applications.

Do not redraw, recolor, crop, trace, or download these logos from third-party sources. If a logo needs to change, update this repository first and publish a new package version.

## Available Assets

The current package is `@warp-ds/brand-logos` version `1.0.0`.

| Brand | Asset | CDN URL |
| --- | --- | --- |
| Blocket.se | `blocket.svg` | `https://assets.finn.no/pkg/@warp-ds/brand-logos/1.0.0/blocket.svg` |
| Blocket.se | `blocket-condensed.svg` | `https://assets.finn.no/pkg/@warp-ds/brand-logos/1.0.0/blocket-condensed.svg` |
| DBA.dk | `dba.svg` | `https://assets.finn.no/pkg/@warp-ds/brand-logos/1.0.0/dba.svg` |
| FINN.no | `finn.svg` | `https://assets.finn.no/pkg/@warp-ds/brand-logos/1.0.0/finn.svg` |
| FINN.no | `finn-condensed.svg` | `https://assets.finn.no/pkg/@warp-ds/brand-logos/1.0.0/finn-condensed.svg` |
| Tori.fi | `tori.svg` | `https://assets.finn.no/pkg/@warp-ds/brand-logos/1.0.0/tori.svg` |

The CDN URL format is:

```text
https://assets.finn.no/pkg/@warp-ds/brand-logos/{version}/{filename}
```

Use versioned CDN URLs in production. Published versions are immutable and can be cached aggressively by browsers and CDN edges.

## Figma

Designers should use the approved logo components from the design system Figma library.

When adding or updating a logo component:

1. Import the SVG from this repository.
2. Create or update the matching Figma component.
3. Preserve the original proportions, paths, and colors.
4. Name the component after the domain-based brand name and variant, for example `FINN.no / Default` or `FINN.no / Condensed`.
5. Publish the updated Figma library.

Do not paste logos from screenshots, marketing pages, search results, or manually edited SVG exports. If the Figma component and repository disagree, the repository is the source of truth.

## Web

Web applications should use the versioned CDN SVG with an `img` element by default.

```html
<img
  src="https://assets.finn.no/pkg/@warp-ds/brand-logos/1.0.0/finn.svg"
  alt="FINN.no"
  width="92"
  height="32"
/>
```

Use `img` because these logos are static brand assets. It keeps page markup small, lets the browser and CDN cache one shared file, avoids accidental inline edits to brand artwork, and gives straightforward accessibility through the `alt` attribute.

If the logo is the only content inside a home link, put the accessible name on the link and leave the image decorative:

```html
<a href="/" aria-label="FINN.no home">
  <img
    src="https://assets.finn.no/pkg/@warp-ds/brand-logos/1.0.0/finn.svg"
    alt=""
    width="92"
    height="32"
  />
</a>
```

Only inline the SVG when the application intentionally needs to control the SVG internals, such as animating paths, styling individual shapes, or building a maintained logo component that owns the markup.

Do not use CSS background images for meaningful logos. Background images do not provide the same accessible-name path as `img`.

## iOS

iOS applications should bundle logo assets with the app. Do not load production logos from the CDN at runtime.

Use the SVG files in this repository as the source for the app asset pipeline:

1. Import the logo into the Xcode asset catalog using the app team's standard vector workflow.
2. Use SVG directly if the app's Xcode and deployment target support it.
3. Otherwise convert from the repository SVG to a vector PDF and enable preserved vector rendering where the project requires it.
4. Commit the generated asset catalog files to the iOS codebase.
5. Verify the rendered logo in light mode, dark mode, and the smallest supported device size.

Do not apply template rendering, tint, symbol effects, or automatic color changes to these logos unless a brand-specific asset explicitly supports that use.

For accessibility, expose the domain-based brand name when the logo communicates identity. If the logo is part of a navigation control, describe the action or destination, for example `FINN.no home`.

## Android

Android applications should bundle logo assets with the app. Do not load production logos from the CDN at runtime.

Use the SVG files in this repository as the source for Android resources:

1. Import the SVG as a Vector Asset via Android Studio.
2. Store the generated vector drawable in the app's `res/drawable/` resources.
3. Keep the logo multicolor. Do not tint it as a Material icon.
4. Verify the converted drawable against the source SVG, because Android VectorDrawable supports only a subset of SVG features.
5. Test the rendered logo on representative screen densities and themes.

In Jetpack Compose, use `Image` for these multicolor logos rather than `Icon`, because `Icon` is intended for tintable iconography.

```kotlin
Image(
    painter = painterResource(id = R.drawable.logo_finn),
    contentDescription = "FINN.no",
)
```

If the logo is inside a button or navigation element, put the accessibility label on the control and make the image decorative where appropriate.

## Accessibility

When a logo communicates brand identity, the accessible name should match the domain-based brand name in the SVG title:

| Brand | Accessible name |
| --- | --- |
| Blocket.se | `Blocket.se` |
| DBA.dk | `DBA.dk` |
| FINN.no | `FINN.no` |
| Tori.fi | `Tori.fi` |

Do not describe visual styling in the accessible name, such as `blue FINN logo`. Do not add the word `logo` unless it is needed to disambiguate the surrounding content.

For linked logos, describe the destination or action instead of the image itself, for example `FINN.no home`.

## Updating Logos

To change a logo:

1. Update the SVG in `logos/`.
2. Ensure the root SVG contains a short `<title>` with the domain-based brand name.
3. Bump the `version` in `eik.json`.
4. Commit and push to `main`.
5. Let CI publish the new Eik package version.
6. Update web consumers to the new versioned CDN URL.
7. Refresh Figma, iOS, and Android assets from the updated repository source.
