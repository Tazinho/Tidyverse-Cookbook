

# RStudio project management

## Project types

Empty project, package or shiny web applications

## Project options



## Version control

* copy the link from a remote repository, when creating a new package
* git add .
* git add -m "some commit message"
* push to remote

## Packrat and package dependencies

Resources: 

* [Packrat Webinar by RStudio](https://www.rstudio.com/resources/webinars/managing-package-dependencies-in-r-with-packrat/)
* [Packrat Homepage](https://rstudio.github.io/packrat/)
    
Most important commands:

* `packrat::init()`, initializes an isolated package library for the current project.
* `packrat::snapshot()` saves all changes within the packrat project library since the last snapshot.
* `packrat::restore()` deletes all changes within the packrat project library since the last snapshot.
* `packrat::bundle()` creates a tarball of the project, the packages and its dependencies.
* `packrat::unbundle(bundle = "path//to//the//bundle", where = ".")` unbundles (restores) a project within a subdirectory of another (conveniently empty) project.
* `packrat::off()` lets you temporarily step out of "packrat mode", which means, that you will now use your global user library instead of the packrat project library.
* `packrat::on()` lets you go back into "packrat mode".

Collaboration:

* Initialise a packrat project on github, let everyone clone it and let them then call `packrat::restore()`.

Updated R Version:

* When you unbundle a project, which was created under an older (or newer) R version, then installed, packrat will warn you and install the older (or newer) R version when calling `packrat::snapshot()`. This is one among other reasons, why it is easier to work with packrat, while having access to the internet.

## Addins

1. **Task:** Structure/separate code into blocks by different lines of lentgh 80 (by default).

    __<span style="color:grey">General:</span>__ Use `devtools::install_github("lorenzwalthert/strcode")` within your project (to install the `strcode` package from [here](https://github.com/lorenzwalthert/strcode)). That will enable the following:
    
    
    ```r
    strcode::insert_l1_break() # or strg + c + 1 (shortcut)
    #   ____________________________________________________________________________
    strcode::insert_l2_break() # or strg + c + 1
    ##  ............................................................................
    strcode::insert_l3_break() # or strg + c + 1
    ### .. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
    ```

## Miscellaneous

1. **Task:** Build a website with rmarkdown.

    __<span style="color:grey">General:</span>__
    
    1. Create a github repository with the name \<yourusername\>.github.io
    
    1. Create an empty RStudio project synchronized with the github repository
    
    1. Create a textfile _site.yml and two rmarkdown files index.Rmd and about.Rmd (also include an empty textfile called .nojekyll [this tells GitHub Pages to not process your site with the Jekyll engine])
    
    **\_site.yml**
    
    
    ```
    name: "my-website"
    navbar:
    title: "My Website"
      left:
        - text: "Home"
          href: index.html
        - text: "About"
          href: about.html
    output:
      html_document:
        theme: cosmo
        highlight: textmate
    output_dir: "."
    ```
    output_dir "." is set to achive that the output htmls are created on the top level of the repository and not within the site folder.
    
    **index.Rmd**
    
    ```
    ---
    title: "My Website"
    ---

    Hello, Website!
    ```
    
    **about.Rmd**
    
    ```
    ---
    title: "About This Website"
    ---

    More about this website.
    ```
    
    1. Save the files above and call rmarkdown::render_site() or close and reopen RStudio and lock for a build pane within RStudio, which lets you build the side. You should see the output now in the viewer pane
    
    1. Open the shell and call git add .
    
    1. Then call commit -m "first build"
    
    1. Then call git push
    
    1. You should see the output within the browser under \<yourusername\>.github.io
    
    * Resources: 
        
        * [rmarkdown website instructions](http://rmarkdown.rstudio.com/rmarkdown_websites.html)
        * [almost the same tutorial as above](https://github.com/privefl/rmarkdown-website-template#make-your-website-with-r-markdown-in-minutes)
        * [video](https://www.youtube.com/watch?v=0GjUWgl03vA)
        * [detailed tutorial](http://nickstrayer.me/RMarkdown_Sites_tutorial/)
