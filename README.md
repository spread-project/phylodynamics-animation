# Phylodynamics, animated

A Claude generated, interactive, animated explainer of **phylodynamics**: how the family tree of pathogen genomes reveals when an epidemic started, how fast it spread and where it went.

**Live version:** [https://spread-project.github.io/phylodynamics-animation/](https://spread-project.github.io/phylodynamics-animation/)

The whole thing is a single self-contained `index.html`, with no build step and no dependencies apart from two Google Fonts. It falls back to system fonts if they can't load.

## What it covers

The page walks through nine animated steps:

1. **Epidemics write themselves into genomes.** An outbreak spreads across a population, and new mutations found new lineages.
2. **A transmission chain becomes a tree.** A simulated epidemic is sampled and pruned into the phylogeny we can actually reconstruct.
3. **The molecular clock.** A root-to-tip regression estimates the substitution rate and the date of the common ancestor (TMRCA).
4. **Coalescent models.** Lineages merge backward in time at a rate k(k−1)/2 ÷ Ne(t), shown for a constant-size population and for a growing epidemic.
5. **Birth–death models (interactive).** Sliders set R = λ/δ and the sampling proportion, and an optional intervention lowers R partway through. The unobserved transmission tree is drawn next to the sampled phylogeny.
6. **Structured models.** Multi-type birth–death and structured coalescent models estimate migration between regions (phylogeography).
7. **Bayesian inference.** The posterior factorises into a likelihood, a tree prior and parameter priors; MCMC sampling then builds the posterior (BEAST 2).
8. **What comes out, and what can go wrong.** An R(t) skyline with credible intervals, followed by the main pitfalls: sampling bias, identifiability, low diversity, and recombination or selection.
9. **Maximum likelihood.** The step-by-step pipeline: tree search (IQ-TREE, RAxML-NG, FastTree), dating (LSD2, TreeTime), ancestral states (PastML, TreeTime mugration), and parameter estimation on a fixed tree (TreeTime skyline, PyBDEI, PhyloDeep).

You can navigate with the Back/Next buttons, the numbered step bar, or the ← → arrow keys. The page adapts to light and dark mode and works on phones. If the system asks for reduced motion, each step jumps straight to its final frame.

The simulations are illustrative. They are stochastic birth–death and coalescent processes with fixed random seeds, not fits to real data.

## Running locally

Open `index.html` in any modern browser. That's it.

## Publishing with GitHub Pages

In the repo, go to **Settings → Pages**. Set the source to *Deploy from a branch*, choose `main` and `/ (root)`, and save. After about a minute the page is live at `https://<you>.github.io/<repo>/`.

## References

- Grenfell et al. (2004). Unifying the epidemiological and evolutionary dynamics of pathogens. *Science* 303:327–332.
- Volz, Koelle & Bedford (2013). Viral phylodynamics. *PLoS Comput Biol* 9:e1002947.
- Stadler et al. (2013). Birth–death skyline plot reveals temporal changes of epidemic spread in HIV and hepatitis C virus (HCV). *PNAS* 110:228–233.
- To et al. (2016). Fast dating using least-squares criteria and algorithms. *Syst Biol* 65:82–97 (LSD).
- Sagulenko, Puller & Neher (2018). TreeTime: maximum-likelihood phylodynamic analysis. *Virus Evol* 4:vex042.
- Ishikawa et al. (2019). A fast likelihood method to reconstruct and visualize ancestral scenarios. *Mol Biol Evol* 36:2069–2085 (PastML).
- Voznica et al. (2022). Deep learning from phylogenies to uncover the epidemiological dynamics of outbreaks. *Nat Commun* 13:3896 (PhyloDeep).
- Zhukova, Hecht, Maday & Gascuel (2023). Fast and accurate maximum-likelihood estimation of multi-type birth–death epidemiological models from phylogenetic trees. *Syst Biol* 72:1387–1402. [doi:10.1093/sysbio/syad059](https://doi.org/10.1093/sysbio/syad059) (PyBDEI).

## License

This project uses two licenses:

- **Code** (the HTML, CSS and JavaScript in `index.html`): [MIT](LICENSE).
- **Content** (step titles, captions, labels, explanatory text, and the animations and figures as presented): [CC BY-NC 4.0](LICENSE-CONTENT.md).

To reuse the content, credit it as: *"Phylodynamics, animated" by &lt;Your Name&gt;, https://github.com/&lt;you&gt;/&lt;repo&gt;, CC BY-NC 4.0*. Commercial reuse of the content needs separate permission.
