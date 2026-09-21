# Font assets

Twenty freely redistributable families are bundled. Existing IDs 0–7 remain unchanged:

Roboto, Open Sans, Lato, Montserrat, Oswald, Raleway, Merriweather, Playfair Display, Nunito, Poppins, Roboto Slab, PT Sans, PT Serif, Source Sans 3, Source Serif 4, Inconsolata, Quicksand, Barlow Condensed, Rubik, and Roboto Mono.

Roboto Slab uses Apache License 2.0; the other nineteen use SIL Open Font License 1.1. Individual copyright notices and complete licenses are in `resources/fonts/licenses/`.

Sources are pinned to https://github.com/google/fonts/tree/e44c4b011a820c2cbe2fd2cfa8052037d7edb571 (`ofl/`, or `apache/robotoslab/`).

Variable families are instantiated at weight 600 and default width/optical size. Lato, PT Sans and PT Serif use static Regular; Poppins and Barlow Condensed use static SemiBold. Fonts are subset to printable ASCII with original up/down arrow glyphs added; hinting and OpenType layout tables are removed. Original letter and number outlines are unchanged. Neutral asset names f0–f19 and internal primary names Photo Frame F0–F19 identify the subsets. The watch and embedded phone preview use the same resulting TTFs.

## Reproduction

With Python 3 and `fonttools==4.65.0`, run `python scripts/prepare-fonts.py`. Verify checked-in TTF/license/preview hashes offline with `python scripts/prepare-fonts.py --check`.

Using a Python environment with `freetype-py` (the local Pebble SDK Python includes it), run in order:

```sh
python scripts/prepare-time-fit.py
python scripts/prepare-solar-metrics.py
python scripts/prepare-font-packs.py
python scripts/test-font-packs.py
pebble clean
pebble build
```

Time tables reserve enough width for every four-digit time and side AM/PM. Font packs render the same FreeType monochrome pixels and bearings as the SDK font generator, retaining all seven detail sizes plus regular and adaptive time sizes. Zlib compression stores all twenty families within the 262,144-byte resource budget. Only the selected family is decoded. During photo replacement its cache is released and SDK text is used temporarily; the selected font is restored after commit, cancellation or timeout. Invalid commits retain the transfer for retry until timeout.

`prepare-date-fonts.py` is a legacy SDK-PBF comparison utility and is no longer part of the build workflow. The packed-font test checks all family glyph pixels/metrics, renderer alignment and malformed input with address/undefined-behavior sanitizers.

The inflater is Joergen Ibsen's tinf 1.2.1 under the zlib license. See `src/c/vendor/tinf/LICENSE` and its README for the pinned revision and local Pebble adaptations.
