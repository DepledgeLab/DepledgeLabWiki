---
tags:
    - Wiki
    - Guide
---


# How to edit this Wiki

---

!!! warning "Work in progress"

    This site is currently under construction, many parts still have an unfinished look or are yet to be written.<br>
    ~Erik

TODO:

- add images
- annotate the YAML file

## Introduction
Hello there, fellow researcher!  
This page is written for all current and future members of AG Depledge and acts as a guiding rope in how to edit this wiki. As the long-term goal of this Wiki is to become an extensive documentation hub, a uniform coding and organising style is essential.
Because of this, please read through this page before making any changes and make yourself familar with the mentioned topics.

Should you have any questions, feel free to ask Erik for more information.
<sub> (That is, if I'm still around by the time you are reading this.) </sub>

### General Information
This Wiki is based on the [_MkDocs_](https://www.mkdocs.org/) framework, which is a common static site generator built for project documentation. To be more precise, it uses an extension for MkDocs, [_Materials for MkDocs_](https://squidfunk.github.io/mkdocs-material/).
??? info "Where _Materials for MkDocs_ comes from"
    Sometime _Materials for MkDocs_ is also refered to as a "theme" of MkDocs. MkDocs allows so called "themes" to change its appearance (and behaviour). It contains only few themes from the get-go, but there are many independent ones.
    _Materials for MkDocs_ started out as such a third party theme and since evolved into a full framework itself. The installation of MkDocs is still neccessary though.
This Wiki (which is essentially the collection of all its files and folders) is hosted on [GitHub](https://github.com/) and deployed through [GitHub Pages](https://pages.github.com/) (using GitHubs automatic [Action](https://github.com/features/actions) workflows).
The actual repository Link of this Wiki is therefore "[https://github.com/DepledgeLab/DepledgeLabWiki](https://github.com/DepledgeLab/DepledgeLabWiki)" (name might change). From here, it hosts the actual Wiki site you can currently see, "[https://depledgelab.github.io/DepledgeLabWiki/](https://depledgelab.github.io/DepledgeLabWiki/)".

### Where to find out more
!!! tip "Recommendation"
    The documentation for _Materials for MkDocs_ is very extensive and well written and also the most important reference for this Wiki. It is highly recommended to give it a visit.

MkDocs Link
Materials for MkDocs Link
GutHub Pages Link

---

## Working with the repository
The actual content of this Wiki lies at https://github.com/DepledgeLab/DepledgeLabWiki. From the files available there, all sites are rendered to this Wiki. This means that if you want to change, add or remove any content you will have to edit, add, or remove the approraite files in mentioned GitHub repository. In most cases you will only want to change one of these files:

`A Mardown file`
:   It are the markdown files that contain the actual content of the sites. One markdown files equals to one site on the Wiki. For example, you can find the content of this page at docs/Home/How_to_ise_this_Wiki/Edit/howto_edit_1.md (in the GitHub repository)(name might change). If you were to change anything in that file the changes would reflect in the rendered Wiki.

`The mkdocs.yml file`
:   This is the main configuration file of this Wiki and might be considered the heart. It contains information on the strucuture of this Wiki, which themes and plugins are enabled etc. More info on that file can be found below (Link).

This leads us to the most important question in this guide:
> How do I properly edit the Wiki the easiest way, and without breaking anything?

Of course, there are many ways to skin a cat. Because of this you can find a collection of possible solutions below. You are free to use whatever method suits you best, just keep in mind the listed pros and especially the cons.  
Erik **strongly** recommends the Local VSCode solution (Link).

<br>

#### First steps
Either way, you will have to do the following steps to be able to edit the Wiki:

1. Get yourself a [GitHub Account](https://github.com/signup) (if you do not already have one).
2. Familiarise yourself with the basics of Git. Failure to do so can easily break the Wiki.
3. Ask Dan for access to the Wiki repository.

### Option 1: Local VSCode

| pros                           | cons                           |
|--------------------------------|--------------------------------|
| <span style="color:#00c853">:material-plus-circle:</span> Once set up very quick to use  | <span style="color:#ff1744">:material-minus-circle:</span> Requires (slightly longer) inital setup |
| <span style="color:#00c853">:material-plus-circle:</span> Allows changes (and saves) to a local instance before remote pushes | <span style="color:#ff1744">:material-minus-circle:</span> One small change in YAML file necessary whenever pull/push |
| <span style="color:#00c853">:material-plus-circle:</span> Allows (almost) instant local live previews | <span style="color:#ff1744">:material-minus-circle:</span> Minor Git and Linux knowledge necessary |
| <span style="color:#00c853">:material-plus-circle:</span> Cleaner remote history | |
| <span style="color:#00c853">:material-plus-circle:</span> Good IDE with many interfaces | |

<br>

##### One-time setup
If you have already done parts of the following steps you can just skip them.

1. Install Git on your local machine ([Download](https://git-scm.com/downloads); changing settings not necessary).
2. Create a local repository (this will be your local instance of the Wiki).
    1. Open the Git bash and navigate to a folder you want to be your local repository to be in.
    (Optional: You might want to create a subfolder for your local repository, that way you can add your python environment without it interfering with git (see directory structure below).)
    2. Initiate the repository and add the Wiki repository as its remote origin (upstream).
    3. (Recommended: rename your branch to _main_ instead of _master_, as this is the name the Wiki repository uses.)
3. (Check if you already have a ssh key.)
    1. Type `cd ~/.ssh` and `ls`.
    2. Check for a file named "id_dsa(.pub)" or "id_rsa(.pub)". If these files do not exist, or the command above does not work, you do not have a ssh key yet.
4. Create a personal SSH-key if you do not already have one (this is necessary to authorize your local mashine to GitHub).
    1. Type `ssh-keygen`.
    2. Press ++return++ thrice.
    3. Go to ~/.ssh (yourUser/.ssh) and copy the **^^public^^** key (content of the .pub file).
5. Add your SSH-key to GitHub.
    1. Open the web browser of your choice and login to GitHub.
    2. Go to your profile settings.
    3. Go to Access -> SSH and GPG keys.
    4. Klick on new SSH key.
    5. Paste and add your public ssh key you just copied.
6. Synchronise with the remote repository.
    1. Go to your local repository in the Git bash and pull the Wiki (`git pull origin main`).
7. Install Python on your local machine ([Download](https://www.python.org/downloads/)).
8. Install VSCode on your local machine ([Download](https://code.visualstudio.com/)).
    1. Add extensions (Python, GitHub Actions) (optional: many more that are useful).
    2. In VScode, go to the parent folder of your local repository (built-in Terminal) and create a new python environment (++ctrl+shift+p++ -> "Create Environment" -> Use a python version). (You can also use conda, but this example will use a standard python environment.)
    3. Open the python terminal and install necessary packages.
    ```
    pip install mkdocs-material
    pip install mkdocs-git-revision-date-localized-plugin
    pip install mkdocs-git-committers-plugin-2
    ```
9. Setup

Unless you used conda or created your python environment differently, your files should now be arranged like this (with hidden files visible):
``` { .text .no-copy }
.
├─ .venv
│  └─ ...
└─ wiki_local
   ├─ .cache
   ├─ .git
   ├─ .github
   ├─ docs
   ├─ site
   ├─ LICENSE
   ├─ mkdocs.yml
   ├─ README.md
   └─ test.py
```

<br>

##### The work routine afterwards

1. Pull from remote if necessary.
2. After pulling from remote: Turn _off_ the committers and revision plugins (these plugin add the creation and last-updated time at the bottom of a site. Using this locally technically still works but increases the live preview time **_a lot_**.).
    1. Open the mkdocs.yml file and scroll to "# GitHub: commented out; local: not commented out".
    2. _Delete_ the '#' in front of 'enabled:'. These two lines should be active locally. Don't forget to save the changes.
3. In the VSCode Terminal: type `mkdocs serve`.
    1. The _git-committers_ plugin should be DISABLED (++ctrl+c++ to abort).
    2. Paste the local adress ito your web browser.
    3. Edits you do are now reflected live (++ctrl+s++ to save a file and update the Wiki).
    4. Use ++ctrl+c++ to shut down.
4. Before pushing to the remote again: Turn _on_ the committers and revision plugins.
    1. Open the mkdocs.yml file and scroll to "# GitHub: commented out; local: not commented out".
    2. _Add_ a '#' in front of 'enabled:'. These two lines should be deactived in the remote repository. Don't forget to save the changes.
5. Commit changes and push them to remote. Merge if necessary.


### Option X: Local Git Only


### Option Y: GitHub Web


---


## Writing content

### Plain text
The content of this Wiki is written in [Markdown](https://markdown.de/). This means that plain text can be written _as is_ and can be lighty modified via the [Markdown syntax](https://www.markdownguide.org/basic-syntax/). For example, this includes **bold text**, _italic text_ or block quotes:

> In the pursuit of great, we failed to do good.

For more nuanced ways to organise your text, you can use [HTML](https://www.w3schools.com/html/).
If more specific modifications are required (e.g.
<span class="rainbow-letters">
  <span>c</span><span>o</span><span>l</span><span>o</span><span>r</span><span>e</span><span>d</span>
</span> text), use [CSS](https://www.w3schools.com/css/).
??? note
    Theoretically, <span class="rainbow-gradient"><i><b>you could go really crazy with this if you wanted to</b></i></nobr></span>.  
    But please remember that too much visual glitter can be distracting and the purpose of this Wiki is documentation, not winning an art contest. Should you do decide to use visual styles with your text, please refrain from using the in-build HTML visual features and instead use CSS.

TODO: formatting with materials for mkdocs instead of html

### Everything beyond plain text
For an extensive list of all additional annotation features (admonitions, code blocks, tooltips etc.) visit the [Materials for MkDcos Reference](https://squidfunk.github.io/mkdocs-material/reference/) page.
Because _Materials for MkDocs_ already offers many formating options it is recommended to check its documentation before trying to implement something manually with HTML or CSS.

TODO: schow examples

---

## Further information

TODO

### Files systems / naming convention


### The mkdocs.yml file


### The ci.yml file


### the .cache file


### the .css file


### Currently activated plugins


### the site folder and offline builds