# Vision Context — Handling Instructions

> [!NOTE]
> **Status:** Vision Context is currently **inactive / disabled** in the pipeline. No visual context processing is active as of now.

## What is Vision Context?

In the Figma extraction `.txt` files, you will encounter a special property:

```
Vision Context: assets/images/<filename>.svg
```

This property appears on **elements that use `position: absolute`** but whose **exact placement coordinates are not explicitly written** in the extraction text. Instead, a **reference image** (screenshot / cropped Figma frame) is provided at the given path.

The image visually shows **where the element is placed** relative to its parent container. The model must **analyze this image** and derive the correct CSS positioning properties (`top`, `right`, `bottom`, `left`, `transform`) from what it sees.

---

## Why Vision Context Exists

Figma extraction text can describe *what* an element is (content, typography, colors) but sometimes cannot numerically describe *where* it sits inside a relative container — especially for:

- Floating badges (e.g. "Sale" tags over card images)
- Overlay icon groups (e.g. like/basket/views icons on images)
- Play buttons centered over media
- Decorative elements with non-trivial offsets

In these cases, a visual reference image is the **single source of truth** for placement.

---

## Pipeline Integration

This step fits into the existing `base-system-prompt.xml` pipeline as a sub-step within **Step 1 (Figma Extraction Compilation)** and **Step 3 (One-Shot Code Execution)**.

### During Step 1 — Compilation

When parsing an extraction `.txt` file and encountering a `Vision Context:` line:

1. **Record** the Vision Context image path in the compiled `.md` payload under that element's specification.
2. **Flag** the element as requiring visual analysis for positioning.
3. **Do NOT** guess or invent positioning values at this stage — only document that Vision Context resolution is needed.

### During Step 3 — Code Execution

When implementing CSS for a Vision Context element:

1. **Open and analyze** the referenced image file.
2. **Derive** the positioning properties from the image (see Analysis Rules below).
3. **Write** the derived values into `css/style.css` as part of the element's ruleset.

---

## Analysis Rules

When you receive a Vision Context image, follow this systematic analysis:

### Rule 1 — Identify the Parent Boundary

Determine the **bounding box of the parent container** (the `position: relative` ancestor). This is typically the full image card, the full image area, or the container frame identified one level above the Vision Context element.

### Rule 2 — Locate the Element

Identify the target element within the image. It will match the `Content` and visual appearance described in the extraction text (e.g., a red "Sale" badge, a group of circular icons, a play button).

### Rule 3 — Determine Quadrant Placement

Analyze which **quadrant or edge** the element occupies relative to its parent:

```
┌─────────────────────────────────┐
│  top-left       top-right       │
│                                 │
│                                 │
│         center (50%, 50%)       │
│                                 │
│                                 │
│  bottom-left    bottom-right    │
└─────────────────────────────────┘
```

Map the element's visual position to one of these placement zones:

| Visual Position       | CSS Properties to Use                                  |
|-----------------------|--------------------------------------------------------|
| Top-left corner       | `top: <offset>; left: <offset>;`                       |
| Top-right corner      | `top: <offset>; right: <offset>;`                      |
| Bottom-left corner    | `bottom: <offset>; left: <offset>;`                    |
| Bottom-right corner   | `bottom: <offset>; right: <offset>;`                   |
| Center                | `top: 50%; left: 50%; transform: translate(-50%, -50%);` |
| Center-left edge      | `top: 50%; left: <offset>; transform: translateY(-50%);` |
| Center-right edge     | `top: 50%; right: <offset>; transform: translateY(-50%);` |
| Top center            | `top: <offset>; left: 50%; transform: translateX(-50%);` |
| Bottom center         | `bottom: <offset>; left: 50%; transform: translateX(-50%);` |

### Rule 4 — Estimate Offset Values

Offsets are estimated by analyzing the **visual gap** between the element's edge and the parent's edge in the image:

- **Small gap** (element nearly touching the edge): `5px` – `10px`
- **Standard gap** (clear breathing room): `15px` – `20px`
- **Large gap** (significant spacing): `25px` – `40px`
- **No gap** (flush against edge): `0`

When the element appears **proportionally positioned** (e.g., roughly 1/3 from the top), prefer **percentage values** (`top: 33%`) over fixed pixel values.

### Rule 5 — Validate Against Context Clues

Cross-check derived positions with any **partial layout hints** from the extraction text:

- If the extraction says `Layout: position-absolute` with no further coordinates → Vision Context is the sole positioning source.
- If the extraction provides *some* values (e.g., `top-50% left-50%`) alongside a Vision Context → use the text values as primary and Vision Context only to confirm or fill gaps.
- If the extraction provides a `Layout:` with `d-flex gap-10px position-absolute` → the element is a flex group that is absolutely positioned; Vision Context tells you *where* the group sits, not the internal flex layout.

---

## Output Format

When writing CSS for a Vision Context element, document the derivation as a comment:

```css
/* Vision Context: assets/images/course-vision-context.svg
   Analysis: "Sale" badge positioned at top-left of image container
   Derived: top: 20px, left: 20px */
.course-sale-badge {
    position: absolute;
    top: 20px;
    left: 20px;
}

/* Vision Context: assets/images/course-vision-context.svg
   Analysis: Action icons group positioned at bottom-right of image container
   Derived: bottom: 20px, right: 20px */
.course-img-actions {
    position: absolute;
    bottom: 20px;
    right: 20px;
}
```

---

## Common Patterns & Examples

### Pattern A — Badge Over Image (Top-Left)

**Extraction text:**
```
Frame: H6 (Level 6)
Layout: position-absolute
Vision Context: assets/images/course-vision-context.svg
Content: "Sale"
```

**What the model sees in the image:** A small colored tag sitting near the top-left corner of a card image, with ~20px offset from both top and left edges.

**Derived CSS:**
```css
.sale-badge {
    position: absolute;
    top: 20px;
    left: 20px;
    z-index: 2;
}
```

### Pattern B — Icon Group Over Image (Bottom-Right)

**Extraction text:**
```
Frame: Image Group (Level 6)
Layout: d-flex gap-10px position-absolute
Vision Context: assets/images/course-vision-context.svg
```

**What the model sees in the image:** A row of circular icons sitting near the bottom-right corner of the card image.

**Derived CSS:**
```css
.image-actions {
    position: absolute;
    bottom: 20px;
    right: 20px;
    display: flex;
    gap: 10px;
    z-index: 2;
}
```

### Pattern C — Play Button Centered Over Media

**Extraction text:**
```
Frame: Icon (Level 5)
Content: icon(fa-play)
Layout: position-absolute top-50% left-50% translate(-50%,-50%)
```

**Note:** This example has explicit coordinates in the extraction text itself, so Vision Context is NOT needed. But if only `position-absolute` was given with a Vision Context image showing a play button dead-center, derive:

```css
.play-btn {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

### Pattern D — Floating Label (Top-Right)

**Extraction text:**
```
Frame: Badge (Level 5)
Layout: position-absolute
Vision Context: assets/images/some-context.svg
Content: "New"
```

**What the model sees:** A badge pinned to the top-right of its container.

**Derived CSS:**
```css
.new-badge {
    position: absolute;
    top: 15px;
    right: 15px;
}
```

---

## Strict Rules

1. **NEVER guess positions without analyzing the Vision Context image.** If the image file is missing or unreadable, flag it as unresolved and ask for human input — do not fabricate values.

2. **NEVER hardcode positions from memory or common patterns** if a Vision Context image is provided. Always derive from the actual image.

3. **The Vision Context image is authoritative** for placement. If text extraction says `position-absolute` and the Vision Context image shows the element at bottom-left, the CSS must use `bottom` + `left` — not `top` + `right`.

4. **Maintain the parent's `position: relative`** in CSS. Every Vision Context element requires its parent container to have `position: relative` to establish the positioning context.

5. **Document every derivation.** Every Vision Context–derived CSS rule must include a comment stating:
   - The source Vision Context image path
   - What was visually observed
   - The derived property values

6. **Use `z-index` intentionally.** Absolutely positioned overlay elements should receive an appropriate `z-index` (typically `2` for overlays over images, `1` for backgrounds) to maintain correct visual stacking.

---

## Integration with base-system-prompt.xml

Add the following to the `<context>` block:

```xml
<vision_context_path>assets/vision-context/</vision_context_path>
```

Add the following to `<guidelines_and_constraints>`:

```xml
<guideline priority="strict">When a Figma extraction element includes a "Vision Context:" property, 
the model must analyze the referenced image to derive accurate CSS positioning values 
(top, right, bottom, left, transform). Never fabricate positioning values without 
visual analysis. Document all derived positions with CSS comments tracing back to the 
Vision Context source image.</guideline>
```

---

## File Organization

```
assets/
├── images/                    ← Production assets used in HTML
│   ├── landing-background.svg
│   ├── school.svg
│   ├── course-vision-context.svg   ← Referenced by extraction
│   └── ...
└── vision-context/            ← This folder: instructions & documentation
    └── VISION-CONTEXT-INSTRUCTIONS.md   ← This file
```

Vision Context reference images live in `assets/images/` alongside other assets (since the extraction file paths point there). This `assets/vision-context/` folder contains only the **handling instructions** for the model.
