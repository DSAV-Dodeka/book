# Content

The content can be found primarily in `./src/content`. There you can find many JSON files. JSON (JavaScript Object Notation) is a format that can easily be read by a machine. In the actual pages, we import these files and then read them, putting the text actually on the website. 

The images can be found in `./src/images`. Import images directly from the relevant page/component, often through the configured aliases such as `$images`.

## Images

Avoid uploading very large images. They make the repository slower to clone/update and they make the website slower, because images are usually the largest browser downloads.

Before committing new images:

- Resize photos to the size the page actually needs.
- Prefer `.webp` for photos.
- Run `npm run build` in `dodekafrontend`; Vite lists generated asset sizes, which makes the largest images easy to spot.
- If one image dominates the asset list, optimize or replace it before committing.
