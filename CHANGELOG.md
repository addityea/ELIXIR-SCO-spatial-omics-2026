# Post-course change log

Below is a list of changes that have been made to this branch after the course:

## 2026-11-09

### Fixed

- Jupyter kernels no longer hang at startup in amd64 containers on Apple Silicon: both
  Dockerfiles now build the SIGTRAP-mask workaround (`docker/nosigtrap.c`) and set it as
  `LD_PRELOAD`. This is a no-op on native amd64/arm64 hosts.
- The kernelspec is additionally registered as `elixir_sp` and `elixir`, matching the kernel
  names that the notebooks were saved with (case-sensitive match was failing before).
- The rstudio image now ships the quarto CLI, which the practical 7 vignettes need to render.
- Practical 7 environment gained `bioconductor-biocstyle`, used inline by both vignettes.
- Practical 8 `p8_niche_domain.ipynb` plotting cells derive the three highlighted clusters from
  the computed `shape_component` keys instead of the hard-coded ids 9, 10 and 15, which the
  pinned environment does not reproduce (`KeyError: '15'`).

## 2026-11-06

### Added

- Notebook includes an option to subset the data for practical 1 to reduce Cellpose run time

## 2026-11-03

### Fixed

- All images are now visible in html-files for practical exercises

## 2026-11-02

### Fixed

- Xenium boundary cropping in p3 has been fixed to align with sainsc crop
- An option to recompute steps in p9 has been added

## 2026-10-30

### Added 

- This changelog has been added

### Fixed

- A smaller version of the Xenium dataset has been uploaded to <https://zenodo.org/records/23766246>.
    - This link replaces the original link in `docker/practical_data.tsv` 
- Practical 0 now creates a symlink to avoid downloading Practical1_cropped.zarr if the cropped dataset has already been created
- `docker/download_labs.sh` can now download from branches with `/` in the branch name

## 2026-10-29

### Added

- Functionality in `docker/download_labs.sh` to delete data from individual practicals when resetting (`reset --data`)

### Fixed

- README.md has been updated to reflect course status
- The data file for practial 6 part 1 dataset is no longer downloaded
    - The paths in the practical 6 part 1 notebook now link to the identical files in the part 2 download
    - The link for part 1 in `docker/practical_data.tsv` is now the same as part 2

