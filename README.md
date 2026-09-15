# CReFaDet — Customer Reviews for Failure Detection

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-red.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22015257.svg)](https://doi.org/10.5281/zenodo.22015257)
![Dataset: NLP](https://img.shields.io/badge/Dataset-NLP-blue.svg)

CReFaDet is an annotated dataset for detecting product failures in Amazon customer reviews of tablet computers. It was created to support text-based reliability analysis and was first used by Meunier-Pion, Zeng, and Liu (2021). It was subsequently used in the preprint [*Assessing Product Reliability from Customer Reviews Through Natural Language Processing and Machine Learning*](https://doi.org/10.2139/ssrn.5262702) by Meunier-Pion, Liu, Zeng, and Barros (2025).

## Dataset overview

- **File:** [`CReFaDet-release-v1.xlsx`](CReFaDet-release-v1.xlsx)
- **Release:** 1.0.0
- **Size:** 762,128 bytes (approximately 744 KiB)
- **Records:** 2,415 customer reviews covering 11 tablet models
- **Failure labels:** 417 intolerable failures (`IF`), 405 tolerable failures (`TF`), and 1,593 reviews with no failure label
- **Format:** Microsoft Excel (`.xlsx`), with two released data sheets

The workbook contains the following sheets:

| Sheet | Scope | Purpose |
| --- | ---: | --- |
| `Customer Review Data` | 2,415 reviews | Review text, model, star rating, failure class, review date, and failure-free usage time |
| `Component labels` | 356 reviews | Component-level annotations and time-to-degradation/failure labels for failure reviews of the Asus C302 model |
See [`DATA_DICTIONARY.md`](DATA_DICTIONARY.md) for column definitions, annotation scopes, missing-value conventions, and duration rules.

## Access and integrity

Download the workbook from the [versioned Zenodo record](https://doi.org/10.5281/zenodo.22015257), this repository, or a versioned GitHub release. For the current branch, use the [raw workbook download](https://github.com/jmpion/CReFaDet/raw/refs/heads/master/CReFaDet-release-v1.xlsx).

Verify the downloaded file with the SHA-256 value in [`SHA256SUMS`](SHA256SUMS):

```bash
sha256sum -c SHA256SUMS
```

## Supported research tasks

The annotations directly support:

- **Failure detection:** classify reviews as failure (`IF` or `TF`) versus no failure label.
- **Failure severity classification:** distinguish intolerable (`IF`) from tolerable (`TF`) failures.
- **Component-level failure analysis:** identify affected components in the annotated Asus C302 failure subset.
- **Reliability information extraction:** extract failure-free usage time, time of initial degradation (`tid`), and time of final failure (`tff`) where explicitly stated.

## Duration normalization

Human annotations preserve the units stated by the reviewer. For experiments that require a numeric value in days, use this versioned conversion policy:

| Unit | Days |
| --- | ---: |
| day (`D`) | 1 |
| week (`W`) | 7 |
| month (`M`) | 30.4375 |
| year (`Y`) | 365.25 |
| hour (`H`) | 1/24 |
| minute (`m`) | 1/1440 |

Thus `1Y` and `12M` are equivalent because `12 × 30.4375 = 365.25`. Composite labels are additive: `1M5D` becomes `35.4375` days. Do not replace the original human-readable label with its normalized value.

An immediate or built-in failure is labeled `0D`. In `usage_time`, `X` means that no explicit failure-free usage duration was stated; a blank normally means the row is outside that annotation's scope. In `tid` and `tff`, missing annotations remain blank.

## Validation performed for release 1.0.0

The release workbook was checked for unique review identifiers, valid categorical labels, valid duration syntax, component-to-review referential integrity, date formatting, formula errors, and embedded private file paths. The values and formulas in both retained data sheets were preserved during the release export.

## Limitations and responsible use

- Reviews were collected in 2020 and cover tablet products only; results should not be assumed to generalize to other periods, platforms, languages, or product categories.
- The dataset is observational. A review is a customer's report, not a verified engineering failure record.
- Missing values are scope-dependent. Consult the data dictionary before treating blanks as negative labels.
- Review text may remain subject to rights held by the original reviewers or platform. See [`LICENSE.md`](LICENSE.md).

## License

The repository-authored annotations, documentation, and metadata are released under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International license](https://creativecommons.org/licenses/by-nc-sa/4.0/) to the extent that the authors hold the relevant rights. Third-party review text may be subject to additional rights and platform terms. See [`LICENSE.md`](LICENSE.md) for the full notice.

## Citation

If you use CReFaDet, please cite the versioned dataset release ([doi:10.5281/zenodo.22015257](https://doi.org/10.5281/zenodo.22015257)) and the relevant publication. GitHub can generate the dataset citation from [`CITATION.cff`](CITATION.cff).

### Original conference paper

```bibtex
@inproceedings{meunierpion2021big,
  title     = {Big Data Analytics for Reputational Reliability Assessment Using Customer Review Data},
  author    = {Meunier-Pion, Jean and Zeng, Zhiguo and Liu, Jie},
  booktitle = {Proceedings of the 31st European Safety and Reliability Conference (ESREL 2021)},
  publisher = {Research Publishing Services},
  year      = {2021},
  pages     = {2336--2343},
  doi       = {10.3850/978-981-18-2016-8_434-cd}
}
```

### Subsequent preprint

```bibtex
@misc{meunierpion2025assessing,
  title  = {Assessing Product Reliability from Customer Reviews Through Natural Language Processing and Machine Learning},
  author = {Meunier-Pion, Jean and Liu, Jie and Zeng, Zhiguo and Barros, Anne},
  year   = {2025},
  note   = {SSRN preprint},
  doi    = {10.2139/ssrn.5262702}
}
```

## Contact

Questions and correction reports can be sent to Jean Meunier-Pion at `jean.meunier-pion@centralesupelec.fr`.
