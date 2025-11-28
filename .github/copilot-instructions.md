# Presentation Generation Instructions

When asked to transform notes into a presentation:

1. Use Marp markdown format
2. Structure as:
   - `# Main Title` for presentation title
   - `---` between slides
   - `## Slide Title` for slide headings
   - Bullet points for content
   - Use `![]()` for images if needed

3. Styling directives:
   - Add `<!-- _class: lead -->` before title slide for emphasis
   - Use `<!-- footer: Your Name -->` for footers
   - Add `<!-- paginate: true -->` for slide numbers

4. Output to slides.md preserving this format exactly ; input is in notes.md

5. Keep slides under 6-8 items per slide max

Think deeply about structure to make it best practice.

Marp output must be in: French ! La présentation est en français, ne l'oublie pas même si on travaille en anglais. Si besoin, les anglissismes sont autorisés.

Your role is to make slides for the presentation so don't charge them too much: not all the notes must fit. Most of it I will say orally. We can set notes (comments) in the markdown if needed.

Example format:
---
# Title Slide
## Subtitle

---
## Content Slide
- Key point 1
- Key point 2
- Key point 3
---

Don't hesitate if you think at some point, it would be better to use an image instead of text. Then you let a placeholder with explanation and i would handle it.

The goal is to create slides for a 25-30 minute presentation.
