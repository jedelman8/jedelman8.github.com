---
layout: post
title: Generating Web Docs for Ansible Modules
tags: ansible
---

If you have ever worked with Ansible, it’s almost a guarantee that you have used their [online docs](http://docs.ansible.com/modules_by_category.html) to figure out what parameters a given module supports, how they should be used, or what their defaults are.  Over the past few weeks, I’ve been working on a few custom modules and was trying to find a way to generate web docs for them, and have them locally accessible or easily posted to GitHub.  

Ansible offers a way to “[make webdocs](http://docs.ansible.com/developing_modules.html#building-testing),” but it generates the complete module inventory and truth be told, I didn’t get this work for my custom modules, so I figured I would explore a “simplified” way --- a way that should be able to generate docs as needed for one or more modules on an as needed basis.  

The outcome was the creation of an Ansible module and Jinja2 template that automatically generates a markdown file (that can then be viewed or posted anywhere). 

### How does it work?

The modules you’ve built or are local to your machine (even Ansible core modules) that you want to generate a web doc for must be documented according to [Ansible standards](http://docs.ansible.com/developing_modules.html#documenting-your-module).  That’s the only major requirement.

From there, you create a playbook that’ll generate the required markdown document. 

Here is a sample playbook that generates a markdown doc for the Ansible [files](http://docs.ansible.com/list_of_files_modules.html) modules.

![pb-web](/img/pbweb.png)

There are three main areas to cover about this playbook.  (1) vars (2) obtaining the docs and (3) creating the markdown file.

Three variables can optionally be defined (heading, subheading, and requirements) that would then be displayed when the markdown doc is generated.

The first task uses the new custom module called [ansible_docstring](https://github.com/jedelman8/ansible-webdocs/blob/master/library/ansible_docstring).  This module gathers the appropriate module documentation and examples from all modules in a given directory.  These docs and examples are then stored in a new variable called modules.

The second task then generates the final markdown doc based on the new [Jinja2 template](https://github.com/jedelman8/ansible-webdocs/blob/master/templates/ansible-docs.j2).

In order to get started, just make sure you have Ansible installed, do a git clone of the repo, and execute the playbook.  You’ll then need a way of viewing the markdown file in a web browser if you aren’t using GitHub.

Sound easy enough?

Below are a few screen shots of what the playbook generates.  To see the actual thing, check it out [here](https://github.com/jedelman8/ansible-webdocs/blob/master/web/ansiblefilesdoc.md).

### Sample 1

![sample1](/img/sample1.png)

### Sample 2

![sample2](/img/sample2.png)

### Sample 3

![sample3](/img/sample3.png)

Because my blog platform lacks significant features (well, it actually does not- just haven't gone back to do all updates yet!), I can’t post the markdown or full output here.  To see the full detail, check it out on [GitHub](https://github.com/jedelman8/ansible-webdocs).

> Note: it’s not shown in this post, but the playbook is generating a markdown file and then I was previewing the markdown in HTML using a markdown viewer plugin for Sublime Text 3.  Following that, I then just uploaded the created markdown file to GitHub.

Since everything has been posted to GitHub, feel free to download the example playbook, template, and the `ansible_docstring` module to try it out.  There are likely some error checks that could be improved, so if you want to contribute, open an issue or pull request!

On a side note, if I didn't end up creating this, I would have spent a ton of time manually creating tables in markdown one at a time for each module I was building!  And just imagine making a small change like adding a row or column to each one!  

Happy Documenting!

Thanks,
Jason

Twitter: @jedelman8




