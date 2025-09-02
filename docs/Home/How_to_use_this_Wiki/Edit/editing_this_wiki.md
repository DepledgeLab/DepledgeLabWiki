---
tags:
    - Wiki
    - Guide
---


# Editing this Wiki

---

!!! warning "Work in progress"

    This site is currently under construction, many parts still have an unfinished look or are yet to be written.<br>
    For a full view of currently available pages check out the [tag directory](../../../tags.md).<br>
    ~Erik



## Introduction
Hello there, fellow researcher!  
This page is written for all current and future members of AG Depledge and acts as a guiding rope in how to edit this wiki. As the long-term goal of this Wiki is to become an extensive documentation hub, a uniform coding and organising style is essential.
Because of this, please read through this page before making any changes and make yourself familiar with the mentioned topics.<br>
Throughout this page, different text formatting has been used deliberately. That way, this page also serves as a reference if you want to include more sophisticated formatting elements. However, don't be alarmed by the length of this page or some sections that look complicated, most of this page is optional.

Should you have any questions, feel free to ask Erik for more information.
<sub> (That is, if I'm still around by the time you are reading this.) </sub>

### General Information
This Wiki is based on the [_MkDocs_](https://www.mkdocs.org/) framework, which is a common static site generator built for project documentation. To be more precise, it uses an extension for MkDocs, [_Materials for MkDocs_](https://squidfunk.github.io/mkdocs-material/).
??? info "Where _Materials for MkDocs_ comes from"
    Sometime _Materials for MkDocs_ is also referred to as a "theme" of MkDocs. MkDocs allows so called "themes" to change its appearance (and behavior). It contains only few themes from the get-go, but there are many independent ones.
    _Materials for MkDocs_ started out as such a third party theme and since evolved into a full framework itself. The installation of MkDocs is still necessary though.
This Wiki (which is essentially just the collection of all its files and folders) is hosted on [GitHub](https://github.com/) and deployed through [GitHub Pages](https://pages.github.com/) (using GitHubs automatic [Action](https://github.com/features/actions) workflows)(1).
The actual repository link of this Wiki is therefore "[https://github.com/DepledgeLab/DepledgeLabWiki](https://github.com/DepledgeLab/DepledgeLabWiki)" (name might change). From here, it hosts the actual Wiki site you can currently see, "[https://depledgelab.github.io/DepledgeLabWiki/](https://depledgelab.github.io/DepledgeLabWiki/)".
{ .annotate }

1. ...which in turn, is based on [Jekyll](https://github.com/jekyll/jekyll), which _in turn_ is written in [Ruby](https://www.ruby-lang.org/en/).

### Where to find out more
!!! tip "Recommendation"
    The documentation for _Materials for MkDocs_ is very extensive and well written and also the most important reference for this Wiki. It is highly recommended to give it a visit.

| Documentation resource           | Link                                                                                         |
| -------------------------------- | -------------------------------------------------------------------------------------------- |
| MkDocs                           | [Click me](https://www.mkdocs.org/)
| ^^**Materials for MkDocs**^^         | [--> ^^**Click me**^^ <--](https://squidfunk.github.io/mkdocs-material/)
| GitHub Pages                     | [Click me](https://docs.github.com/en/pages)
| Markdown                         | [Click me](https://www.markdownguide.org/)
| HTML                             | [Click me](https://www.w3schools.com/html/default.asp)
| CSS                              | [Click me](https://www.w3schools.com/cssref/index.php)
| Git                              | [Click me](https://git-scm.com/doc)


---

## Working with the repository
The actual content of this Wiki lies at "https://github.com/DepledgeLab/DepledgeLabWiki". From the files available there, all sites are rendered to this Wiki. This means that if you want to change, add or remove any content you will have to edit, add, or remove the appropriate files in the mentioned GitHub repository. In most cases you will only want to change one of these files:

`The .md (markdown) files`
:   The markdown files are the files that contain the actual content of the sites. One markdown file equals to one site on the Wiki. For example, you can find the content of this page you are currently reading at "docs/Home/How_to_use_this_Wiki/Edit/editing_this_wiki.md" (in the GitHub repository)(name might change). If you were to change anything in that file the changes would reflect in the rendered Wiki.

`The mkdocs.yml file`
:   This is the main configuration file of this Wiki and could be considered the heart. It contains information on the structure of this Wiki, which themes and plugins are enabled etc. More info on that file can be found at [Further Information on this Wiki](./further_wiki_info.md).

This leads us to the most important question in this guide:
> How do I actually edit the Wiki?

There are many different options. Because of this you can find a collection of possible solutions below. You are free to use whatever method suits you best, just keep the listed (dis-)advantages in mind. <br>
If you already have some coding experiences, Erik **strongly** recommends the [Local - Git + IDE (VSCode)](#option-4-local-git-ide-vscode-recommended) solution. <br>
Otherwise, it is recommended to stick to [solution 1](#option-1-remote-standard-github-editor-through-github-or-the-wiki-itself). It has a slimmer selection of features (i.e. you can't look at the changes you have made live), but it's less convoluted. If you choose this option, just ignore the other.

<br>

#### First steps
Either way, you will have to do the following steps to be able to edit the Wiki:

<div class="annotate" markdown>
1. Get yourself a [GitHub Account](https://github.com/signup) (if you do not already have one).
2. Familiarise yourself with the [basics of GitHub](https://docs.github.com/en/get-started/start-your-journey/about-github-and-git).<br>
(If you are opting to use a more extensive solution (Local repo), you should also learn the basics of git. Failure to do so can easily break the Wiki.)
3. Ask Dan for access to the Wiki repository.(1)
</div>

1. Note to Dan: In the [repositories settings](https://github.com/DepledgeLab/DepledgeLabWiki/settings) just click on Collaborators (left) and add people.

<br>

---

### Option 1: Remote - Standard GitHub editor (through GitHub or the Wiki itself)

| pros                           | cons                           |
|--------------------------------|--------------------------------|
| <span style="color:#00c853">:material-plus-circle:</span> Super quick and easy changes | <span style="color:#ff1744">:material-minus-circle:</span> No live preview |
| <span style="color:#00c853">:material-plus-circle:</span> No local installations needed | <span style="color:#ff1744">:material-minus-circle:</span> Messy history |
| <span style="color:#00c853">:material-plus-circle:</span> No advanced knowledge needed | <span style="color:#ff1744">:material-minus-circle:</span> Limited editing (e.g. no deletion of files) |
|  | <span style="color:#ff1744">:material-minus-circle:</span> Longer reloading times (Wiki reboots with every save) |
|  | <span style="color:#ff1744">:material-minus-circle:</span> Many changes in quick succession might hit API limits and restrict changes temporarily |

1. Login into your GitHub account in your web browser.
2. Navigate to the file you want to edit on [^^GitHub^^](https://github.com/DepledgeLab/DepledgeLabWiki) and click on _Edit this file_ on the right.<br>**OR**<br>Navigate to the site you want to edit in the [^^Wiki^^](https://depledgelab.github.io/DepledgeLabWiki) and click on _Edit this page_ in the top right.
3. Edit and save the file.

---

### Option 2: Remote - GitHubs integrated VSCode

| pros                           | cons                           |
|--------------------------------|--------------------------------|
| <span style="color:#00c853">:material-plus-circle:</span> Quick changes | <span style="color:#ff1744">:material-minus-circle:</span> No live preview |
| <span style="color:#00c853">:material-plus-circle:</span> No local installations needed |  |

1. Login into your GitHub account in your web browser.
2. Navigate to the file you want to edit on [^^GitHub^^](https://github.com/DepledgeLab/DepledgeLabWiki) and press ++period++ (on your keyboard) to open the integrated VSCode editor.
3. Edit and save your files (Tip: Local files can be added with drag-and-drop).
4. Commit and push them (left sidebar --> Source Control).

---

### Option 3: Local - Git only

| pros                           | cons                           |
|--------------------------------|--------------------------------|
| <span style="color:#00c853">:material-plus-circle:</span> Allows changes (and saves) to a local instance before remote pushes | <span style="color:#ff1744">:material-minus-circle:</span> Requires (slightly longer) initial setup |
| <span style="color:#00c853">:material-plus-circle:</span> Cleaner history | <span style="color:#ff1744">:material-minus-circle:</span> No live preview |
| <span style="color:#00c853">:material-plus-circle:</span> No IDE installation necessary | <span style="color:#ff1744">:material-minus-circle:</span> Minor Git and Linux knowledge necessary |

=== "Initial (one-time) setup"

    If you have already done parts of the following steps you can just skip them.

    1. Install Git on your local machine ([Download](https://git-scm.com/downloads); changing settings not necessary).
    2. Create a local repository (this will be your local instance of the Wiki).
        1. Open the Git bash and navigate to a folder you want to be your local repository to be in.
        2. Initiate the repository and add the Wiki repository as its remote origin (upstream).
        3. (Recommended: rename your branch to _main_ instead of _master_, as this is the name the Wiki repository uses.)
    3. (Check if you already have a ssh key.)
        1. Type `#!bash cd ~/.ssh` and `#!bash ls`.
        2. Check for a file named "id_dsa(.pub)" or "id_rsa(.pub)". If these files do not exist, or the command above does not work, you do not have a ssh key yet.
    4. Create a personal ssh-key if you do not already have one (this is necessary to authorize your local machine to GitHub).
        1. In the Git bash, type `#!bash ssh-keygen`.
        2. Press ++return++ thrice.
        3. Go to ~/.ssh (yourUser/.ssh) and copy the **^^public^^** key (content of the .pub file).
    5. Add your ssh key to GitHub.
        1. Open the web browser of your choice and login to GitHub.
        2. Go to your profile settings.
        3. Go to Access --> SSH and GPG keys.
        4. Click on _new SSH key_.
        5. Paste and add your public ssh key you just copied.
    6. Synchronise with the remote repository.
        1. In the Git bash, set your username and email (corresponding to your GitHub account) (`#!bash git config user.name XXX`, `#!bash git config user.email XXX`).
        2. Go to your local repository in the Git bash and pull the Wiki (`#!bash git pull origin main`).

=== "The work routine afterwards"

    1. Pull from remote if necessary.
    2. Locally edit files (with an editor of your choice).
    3. Commit changes and push them to remote. Merge if necessary.

---

### Option 4: Local - Git + IDE (VSCode) [Recommended]

| pros                           | cons                           |
|--------------------------------|--------------------------------|
| <span style="color:#00c853">:material-plus-circle:</span> Once set up very quick to use  | <span style="color:#ff1744">:material-minus-circle:</span> Requires (slightly longer) initial setup |
| <span style="color:#00c853">:material-plus-circle:</span> Allows changes (and saves) to a local instance before remote pushes | <span style="color:#ff1744">:material-minus-circle:</span> Small changes in YAML file necessary whenever pull/push (difference in configuration for remote/local) |
| <span style="color:#00c853">:material-plus-circle:</span> Allows (almost) instant local live previews | <span style="color:#ff1744">:material-minus-circle:</span> Minor Git and Linux knowledge necessary |
| <span style="color:#00c853">:material-plus-circle:</span> Cleaner history | |
| <span style="color:#00c853">:material-plus-circle:</span> Good IDE with many interfaces | |

=== "Initial (one-time) setup"

    This guide will use VSCode as its IDE. If you have already done parts of the following steps you can just skip them.

    1. Install Git on your local machine ([Download](https://git-scm.com/downloads); changing settings not necessary).
    2. Create a local repository (this will be your local instance of the Wiki).
        1. Open the Git bash and navigate to a folder you want to be your local repository to be in.<br>
        (Optional: You might want to create a subfolder for your local repository, that way you can add your python environment without it interfering with git (see directory structure below).)
        2. Initiate the repository and add the Wiki repository as its remote origin.
        3. (Recommended: rename your branch to _main_ instead of _master_, as this is the name the Wiki repository uses.)
    3. Install Python on your local machine ([Download](https://www.python.org/downloads/)).
    4. Install VSCode on your local machine ([Download](https://code.visualstudio.com/)).
        1. Add extensions (Python, GitHub Pull Requests) (and many more that are useful (optional)).
        2. In VScode, go to the parent folder of your local repository (built-in terminal) and create a new python environment (++ctrl+shift+p++ --> "Create Environment" --> Choose a python version). (You can also use conda, but this example will use a standard python environment.)
        3. Open the python terminal and install necessary packages:
        ```
        pip install mkdocs-material
        pip install mkdocs-git-revision-date-localized-plugin
        pip install mkdocs-git-committers-plugin-2
        ```
        4. Login to your GitHub account on VSCode (left sidebar)
    5. Setup
        1. Pull from remote (left sidebar --> Source control --> Pull)

    Unless you used conda or created your python environment differently, your files should now be arranged like this (including hidden files):
    ``` { .text .no-copy }
    .
    ├─ .venv
    │  └─ ...
    └─ wiki_local
       ├─ .cache
       ├─ .github
       ├─ docs
       ├─ includes
       ├─ site
       ├─ LICENSE
       ├─ mkdocs.yml
       ├─ README.md
       └─ test.py
    ```

=== "The work routine afterwards"

    1. Pull from remote if necessary.
    2. After pulling from remote:
        1. Turn _off_ the committers and revision plugins. (These plugin add the creation and last-updated time at the bottom of a site. Using this locally technically still works but increases the live preview time **_massively_**.)
            1. Open the `mkdocs.yml` file and scroll to `#!yaml # GitHub: commented out; local: not commented out`.
            2. _Delete_ the `#` in front of `enabled:`. These two lines should be active locally. Don't forget to save the changes (++ctrl+s++).
        2. Change the `site_url` to local
            1. Open the `mkdocs.yml` file and scroll to the very top
            2. Un-comment (remove the `#` in front) `#!yaml #site_url: http://127.0.0.1:8000/` and comment (add a `#` in front) `#!yaml site_url: https://depledgelab.github.io/DepledgeLabWiki/`
    3. In the VSCode Terminal: type `#!python mkdocs serve` (make sure you are in the folder that contains the Wiki).
        1. The _git-committers_ plugin should be DISABLED (++ctrl+c++ to abort).
        2. Paste the local address into your web browser.
        3. Edits you do are now reflected live (++ctrl+s++ to save a file and update the Wiki).
        4. Use ++ctrl+c++ in the VSCode terminal to shut down.
    4. Edit files locally (via VSCode).
    5. Before pushing to the remote again:
        1. Turn _on_ the committers and revision plugins.
            1. Open the `mkdocs.yml` file and scroll to `#!yaml # GitHub: commented out; local: not commented out`.
            2. _Add_ a `#` in front of `enabled:`. These two lines should be deactivated in the remote repository. Don't forget to save the changes.
        2. Change the `site_url` to remote
            1. Open the `mkdocs.yml` file and scroll to the very top
            2. Un-comment `#!yaml #site_url: https://depledgelab.github.io/DepledgeLabWiki/` and comment `#!yaml site_url: http://127.0.0.1:8000/`
    6. Commit changes and push them to remote. Merge if necessary.

    !!! note
        Bigger changes to `mkdocs.yml` sometimes require to restart the Wiki (++ctrl+c++ + `#!python mkdocs serve`).

---

## Adding a new page

### Obligatory changes

In order to add a new page to the Wiki, you have to do just **two** things:

<div class="annotate" markdown>
1. Create a new .md file in the docs folder. Create subfolders if necessary. Best practise is, to mirror the layout of the wiki to the folder strucutre (e.g.: The monday lab meeting schedule is located at Home -> Schedules and Events -> Monday Lab meetings. Conversely, the corresponding .md file is located at /docs/Home/Schedules_and_Events/monday_meeting.md.)(1)
2. Determine where and with which name the new page should be visible in the Wiki. For this, open the mkdocs.yml file. Under '### Structure of the Wiki', the layout of the Wiki is specified. Each bullet point is one entry in the Wiki.(2) Here, you enter your new page with the format 'Name of the page: pathtothefile/file.md'
</div>

1. The way the Wiki is currently set up, they don't **have to** match, as the Wiki itself doesn't really care where the files are; but it's more organised this way.
2. Bullet points with no .md files attached to them are chapters (see the content table on the left), they don't have their own page. Chapters must contain at least one page underneath them.

Example: <br>
Say this is how the docs folder looks like (a section of it):

```
docs
├─ Getting_started
│  └─ ...
├─ Home
│   ├─ How_to_use_this_Wiki
│   │   ├─ Edit
│   │   │   ├─ editing_this_wiki.md
│   │   │   └─ further_wiki_info.md
│   │   └─ read_search.md
│   ├─ Schedules_and_Events
│   │  └─ ...
│   └─ news.md
├─ How-to_Dry-lab
│   └─ ...
...
```

And we wanted to add a new chapter under 'How_to_use_this_Wiki' called 'Changing the layout' and add a page to that called 'Why darkmode is superior'. We would first create the needed folders and .md file, so our new structure looks like this:

``` hl_lines="10 11"
docs
├─ Getting_started
│  └─ ...
├─ Home
│   ├─ How_to_use_this_Wiki
│   │   ├─ Edit
│   │   │   ├─ editing_this_wiki.md
│   │   │   └─ further_wiki_info.md
│   │   ├─ read_search.md
│   │   └─ Change_layout
│   │       └─ darkmode.md
│   ├─ Schedules_and_Events
│   │  └─ ...
│   └─ news.md
├─ How-to_Dry-lab
│   └─ ...
...
```

Once we have written something into the file (and saved it) we add it to our table in the mkdocs.yml file.

Previously, the respective session may have looked something like this:

``` yaml
- Home:
    - Welcome: index.md
    - News: Home/news.md
    - Schedules and Events:
      - ...
    - How to use this Wiki:
      - Read, Search and Tags:
        - Manoeuver through the Wiki: Home/How_to_use_this_Wiki/read_search.md
        - List of Tags: tags.md
      - Manage:
        - Editing this Wiki: Home/How_to_use_this_Wiki/Edit/editing_this_wiki.md
        - Further Information on this Wiki: Home/How_to_use_this_Wiki/Edit/further_wiki_info.md
  - Getting Started:
    - ...
...
```

After our edit, it might look like this:

``` yaml hl_lines="13 14"
- Home:
    - Welcome: index.md
    - News: Home/news.md
    - Schedules and Events:
      - ...
    - How to use this Wiki:
      - Read, Search and Tags:
        - Manoeuver through the Wiki: Home/How_to_use_this_Wiki/read_search.md
        - List of Tags: tags.md
      - Manage:
        - Editing this Wiki: Home/How_to_use_this_Wiki/Edit/editing_this_wiki.md
        - Further Information on this Wiki: Home/How_to_use_this_Wiki/Edit/further_wiki_info.md
      - Changing the layout:
        - Why darkmode is superior: Home/How_to_use_this_Wiki/Change_layout/darkmode.md
  - Getting Started:
    - ...
...
```

Once both has been done, saved and pushed to the repository, it should be updated after a few minutes.

### Additional (optional) things to add (for better QoL):

You can also add tags to a page (see at top at this page). These help categorising the Wiki. The Wiki automatically creates an overview of all pages associated with a certain tag at the [tags page](../../../tags.md). Adding tags is technically not obligatory for the Wiki to work, but makes it significantly easier to find something.<br>
[How to add a tag](./further_wiki_info.md#adding-tags)

If your text contains common abbreviations, you can also add them to the Wiki-wide abbreviation index. That way,  whenever that abbreviation is used anywhere in the Wiki, it's explanation is also automatically rendered (example: Using your mouse, hover over this abbreviation: QoL).<br>
[How to add abbreviations](./further_wiki_info.md#adding-abbreviations-the-includesabbreviationsmd-file)


## Writing content

### Plain text
The content of this Wiki is written in Markdown. This means that plain text can be written _as is_ and be lightly modified via the [Markdown syntax](https://www.markdownguide.org/basic-syntax/) (Check this link to see a table of options). For example, this includes **bold text**, _italic text_ or block quotes:

> In the pursuit of great, we failed to do good.

For more nuanced ways to format your text, you can use [HTML](https://www.w3schools.com/html/).
If even more specific modifications are required (e.g.
<span class="rainbow-letters">
  <span>c</span><span>o</span><span>l</span><span>o</span><span>r</span><span>e</span><span>d</span>
</span> text), you can use [CSS](https://www.w3schools.com/css/).
??? note "Endless possibilities"
    Theoretically, <span class="rainbow-gradient"><i><b>you could go really crazy with this if you wanted to</b></i></nobr></span>.  
    That doesn't mean you have to, or even that you should (in fact, keeping it simple is often a good approach). But it _is_ possible.

!!! info
    _Materials for MkDcos_ also offers various [markdown-like inline text modifications](https://squidfunk.github.io/mkdocs-material/reference/formatting/) (^^underlining^^, {==highlighting==}, ~sub-~ and ^superscripts^, etc.) that can be very useful.


### Everything beyond plain text
For an extensive list of all additional annotation features (admonitions, content tabs, tooltips etc.) visit the [Materials for MkDcos Reference](https://squidfunk.github.io/mkdocs-material/reference/) page. Especially the admonitions are very nice!

!!! Info
    I am an admonition!

Because _Materials for MkDocs_ already offers many formatting options it is recommended to check its documentation before trying to implement something manually with HTML or CSS.

This also includes more complex structures including these (but not limited to):

=== "Code block example"

    An exemplary code block made with _Materials for MkDocs_ including line numbers, line highlighting and correct syntax highlighting:
    ``` r linenums="1" title="Dummy_script.R" hl_lines="5 6"
    for (i in (1:length(mRNA_IDs))) {
    k <- 1
    for (j in (1:nrow(exon_IDs))) {
        if (str_detect(exon_parents[j], mRNA_IDs[i])) {
        df_all_mRNA_w_exons[k, i, 1] <- exon_IDs[j, 1]
        df_all_mRNA_w_exons[k, i, 2] <- exon_IDs[j, 2]
        k <- k + 1
        }
      }
    }
    ```

=== "Diagram example"

    An exemplary mermaid diagram made with _Materials for MkDocs_:
    ``` mermaid
    graph LR
    A[Hungry] --> B{Eat chocolate?};
    B -->|No| C[Disappointment];
    C --> D{Are you happy?};
    D -->|No| B;
    B ---->|Yes| E[Yummy!];
    ```

=== "Formula example"

    An exemplary Mathjax formula

    $$
    \cos x=\sum_{k=0}^{\infty}\frac{(-1)^k}{(2k)!}x^{2k}
    $$

---
