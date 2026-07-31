# tusk

Source for the Roberts Lab course set — **<https://robertslab.github.io/tusk/>**

The intent is to offer core, basic content. What that exactly might be could be complicated, but let's start very basic / foundational and work our way up.

The site has three kinds of content:

- **Modules** — hands-on technical training, ordered so that each one builds on the ones before it, but written so any single module also stands alone.
- **Organism Biology** — fundamental biology of the animals we work on, for readers with no background in invertebrate zoology. Sits between the technical modules and the conceptual framing, both of which assume it.
- **Lab Science** — a beginner bridge from gene expression to DNA methylation, followed by the lab's conceptual grounding for environmental memory and resilience and the foundational papers behind it.

## Repository layout

```
_quarto.yml            site config — navbar, sidebar, theme
index.qmd              landing page
about.qmd              about / contributing pointer
styles.css             site-wide CSS overrides
modules/               numbered training modules (00–05)
  04-data/             inputs + outputs for the BLAST module
biology/               Organism Biology pages
  _template.qmd        starting point for a new taxon page (not rendered)
framework/             Lab Science pages
  papers/              openly licensed PDFs served from the site
docs/                  RENDERED SITE — committed, served by GitHub Pages
_site/                 stale output from an older config; not used
```

`docs/` is the published site. GitHub Pages serves it directly from `main`, which means **rendered output has to be committed along with your source changes** or the live site will not update. The `.nojekyll` files stop Pages from trying to run Jekyll over the Quarto output.

## Building locally

You need:

- [Quarto](https://quarto.org/docs/get-started/) 1.7 or newer
- R, plus the `knitr` package — the modules use `{r}` code chunks, so Quarto renders through knitr rather than treating them as plain markdown

Then, from the repo root:

```bash
quarto preview
```

That serves the site locally and live-reloads as you edit. When you're ready to publish:

```bash
quarto render
```

This writes to `docs/`. Commit the source *and* the `docs/` changes together.

> **Heads up on `modules/04-blast.qmd`:** several of its chunks are still `eval=TRUE`, so rendering it downloads reference data and actually runs `makeblastdb` / `blastx` into `modules/04-data/`. You need NCBI BLAST+ on your `PATH`, and a full render takes a few minutes. If you only touched an unrelated page, render just that file — `quarto render index.qmd` — instead of the whole site.

Note also that rendering with a newer Quarto than the last contributor used will rewrite everything under `docs/site_libs/`. That churn is expected and harmless; it just makes the diff noisy.

## Contributing

Issues, corrections, and new content are all welcome. Every page has **Edit this page** and **Report an issue** links in the right margin for small fixes.

For anything larger, branch off `main`, make the change, render, and open a pull request.

### Adding a module

1. Create `modules/NN-shortname.qmd`, continuing the number sequence. The number sets reading order.
2. Give it front matter with a short, human-readable title — this is the text that shows up in the sidebar:

   ```yaml
   ---
   title: "NCBI Blast"
   subtitle: "Taking a set of unknown sequence files and annotating them"
   ---
   ```

3. Register it in `_quarto.yml` under the `Modules` section of `sidebar: contents:`. **A file that isn't listed there renders but is unreachable from the site navigation** — this is the most common thing to forget.
4. Render and commit.

### Adding a biology page

1. Copy `biology/_template.qmd` to `biology/NN-shortname.qmd`. Files beginning with an underscore are not rendered, so the template itself never reaches the site.
2. Keep the eight sections in the order the template gives them. Consistency across taxon pages is deliberate — a reader who learns the shape on the bivalve page should be able to skim the coral page.
3. Register it in `_quarto.yml` under the `Organism Biology` section of `sidebar: contents:`, same as for a module.
4. Render and commit.

Write these for someone who has never taken invertebrate zoology. If a term needs defining, define it inline *and* add it to `biology/glossary.qmd`.

### Figures and licensing

Everything under `docs/` is publicly served, so the same licensing care that applies to `framework/papers/` applies to images.

- **Prefer Mermaid** (` ```{mermaid} ` blocks) for life cycles, phylogenies, and process flows. Quarto renders them natively — no licensing exposure, they diff cleanly in review, and they adapt to the light and dark themes.
- **For anatomy**, use lab-made figures or verified CC BY / public-domain sources only. NOAA and USFWS material is generally public domain and is the safest external source.
- Record the source and license in an HTML comment beside the figure *and* in the alt text.

### Writing guidelines

- Assume the reader is new to the tool. Explain the *why* before the *how*.
- Prefer showing real commands with real output over describing them in prose.
- Keep code chunks reproducible. If a chunk depends on data or software a reader won't have, set `eval=FALSE` and show the expected output as a static block instead.
- Don't commit large data files. `data/` is git-ignored; use it for scratch inputs and outputs.

### A note on the papers

`framework/papers/` only holds PDFs that carry an open license (CC BY / CC BY-NC), because everything in `docs/` is publicly served. Paywalled papers are cited by DOI only. Please keep it that way when adding references — check the paper's license before committing a PDF.

## Questions

Open an issue at <https://github.com/RobertsLab/tusk/issues>.
