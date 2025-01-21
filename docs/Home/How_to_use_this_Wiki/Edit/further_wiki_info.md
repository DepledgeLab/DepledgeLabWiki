---
tags:
    - Wiki
    - Guide
---

# Further Information of this Wiki

!!! warning "Work in progress"

    This site is currently under construction, many parts still have an unfinished look or are yet to be written.<br>
    ~Erik


## Further information

TODO: more detailed

### Files systems / naming convention
- Important info on navigation.
- Structure not determined by directories, but by `#!yaml nav:` in `mkdocs.yml`. Paths still need to be correct.
- Current attempt: same naming scheme/directory structure between filenames/pagenames/pageheaders for better overview. (But wouldn't be obligatory.)
- Site URLs are generated from directory and file names.

### The mkdocs.yml file
- Main configuration file
- Includes general configurations, Wiki structure, cosmetics (e.g. themes and colors), extensions and plugins.


### The .github/workflows/ci.yml file
- gives settings for the GitHub Actions deployement of the site.

### The .cache folder
- contains cached data, mainly (only?) git committers data. Greatly reduces load time.
- `.cache` is listed in `.gitignore` to prevent merge unnecessary conflicts.

### The stylesheets/extra.css file
- contains additional text formatting settings.

### Adding tags
- Add Tag Name and reference name in YAML file (bottom part).
- Add icon (using reference name) in YAML file (top part).
- Add Tag Name to the head of an .md file.
- Currently (automaticcaly) lists all tags at tags.md.

### Adding abbreviations (the includes/abbreviations.md file)
- Full text automatically pops up as a tooltip (e.g.: hover over the term "HTML" on any page).
- just add an abbreviation in includes/abbreviations.md in the format `#yaml [TIAE]: This Es An Example`.
- The rest is done by the configurations, tooltips work globally.


### Currently activated plugins
- see `mkdocs.yml`
- check the Materials for MkDocs documentation for more information.

### The site folder and offline builds
- use `#!python mdocs build` to manually generate a static build (--> site).

---
