# coming-soon

ИСКРИ is coming soon.

## Font

The ИСКРИ wordmark uses Jost ExtraLight (200), subset to the letters И,
С, К and Р. The subset is inlined in `index.html` as a base64 data URI,
so the page loads no font file. While the font loads,
`font-display: block` briefly hides the text rather than showing a
fallback font. The `h1` has a fixed width in em, so it does not move
when the font is ready. Jost uses the SIL Open Font License 1.1. See
`OFL.txt`.

To change the wordmark, put the new text in `<h1>`. Then rebuild the
subset from the variable Jost font, with the new text in `--text`:

```sh
curl -fLo 'Jost[wght].ttf' \
  'https://raw.githubusercontent.com/google/fonts/main/ofl/jost/Jost%5Bwght%5D.ttf'
uvx --from 'fonttools[woff]' fonttools varLib.instancer 'Jost[wght].ttf' \
  wght=200 -o Jost-200.ttf
uvx --from 'fonttools[woff]' pyftsubset Jost-200.ttf \
  --text=ИСКРИ --flavor=woff2 --layout-features=kern \
  --output-file=iskri-jost200.woff2
base64 < iskri-jost200.woff2 | tr -d '\n'
```

Replace the base64 string in `@font-face` with the output. Then open the
page and run this in the browser console:

```js
await document.fonts.ready;
const h1 = document.querySelector('h1');
const r = document.createRange();
r.selectNodeContents(h1);
// minus the letter-spacing after the last letter
r.getBoundingClientRect().width / parseFloat(getComputedStyle(h1).fontSize) - 0.08
```

Set `--wordmark-width` to the result, so the wordmark takes up to 70% of
the screen width.
