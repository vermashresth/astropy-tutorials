Contributing
============

Overview
--------

Each tutorial is essentially an [IPython notebook](http://ipython.org/notebook.html)
file. The notebooks are each saved in a separate directory within the `tutorials`
subdirectory in this project. Let's look in [FITS-Header](https://github.com/astropy/astropy-tutorials/tree/master/tutorials/FITS-header/)
as an example. There is a single IPython notebook file that contains the text
and code for the tutorial, and a FITS file used in the tutorial. The notebook
file is automatically run and converted into a static HTML page [example](http://tutorials.astropy.org/FITS-header.html), which is then displayed in
the tutorial listing on http://tutorials.astropy.org. Each tutorial notebook
file also contains metadata about the tutorial such as the author's name, month
and year it was written, and any other information that should be associated
with the tutorial.

Content Guidelines
--------
Narrative:
- Please read through the other tutorials to get a sense of the desired tone and length. 
- Use the first-person inclusive plural ("we"). For example, "We are going to make a plot which..", "Above, we did it the hard way, but here is the easier  way..."
- Avoid words such as "obviously","just", "simply", "easily". For example, "we just have to do this one thing."
- Include brief explanations and descriptions
 
Content:
- Demo ~2-3 astro-relevant functions and 2-3 generic but commonly used functions (e.g., numpy, matplotlib)  
- Roughly follow this progression:
  - Intput/Output: read in some data 
  - Analysis: do something insightful/useful 
  - Visualization: make a pretty figure
- Include relevant uses for these packages 
  - [astropy.units](http://docs.astropy.org/en/stable/units/)
  - [astropy.coordinates](http://docs.astropy.org/en/stable/coordinates/)
  - [astroquery](https://astroquery.readthedocs.io/en/latest/)

Code:
- Demonstrate good commenting practice
- Demonstrate best practices of variable names. 
   - Variables should be all lower case with words separated by underscores.
   - Variable names should be descriptive. E.g., galaxy_mass, u_mag.
- As much as possible, comply with [PEP8](https://www.python.org/dev/peps/pep-0008/)

Description:
- Compile a list of the functions and packages the tutorial demonstartes and include a short a description with the pull request.

Procedure
---------

If you are unfamiliar with git, you should first get familiar with git and
github. There are a number of resources available for learning git, but a good
place to start is with the [github interactive tutorial](http://try.github.io/).
You should also get familiar with using pull requests and forks on github:
https://help.github.com/articles/using-pull-requests

To create and contribute a new tutorial, you will first need to fork the
astropy-tutorials repository on github and clone this fork locally to your
machine (replace <GITHUB USERNAME> with your github username)::

    git clone git@github.com:<GITHUB USERNAME>/astropy-tutorials.git

Next, create a branch in your local repository with the name of the tutorial
you'd like to contribute. Let's imagine we're adding a tutorial to demonstrate
spectral line fitting -- we might call it `Spectral-Line-Fitting`:

    git checkout -b Spectral-Line-Fitting

Next we'll create a new directory in `tutorials/` with the same name as the
branch:

    mkdir tutorials/Spectral-Line-Fitting

All files used by the tutorial -- e.g., example data files, the IPython
notebook file itself -- should go in this directory. Now you can start writing
the tutorial! Change directories into this new path and start up an
IPython notebook server:

    cd tutorials/Spectral-Line-Fitting
    ipython notebook --matplotlib inline

Create a new notebook file, and write away! (Following the Content Guidelines above.)
Remember to place any extra files
used by the tutorial in the directory with the notebook file, and place them
under git version control.

You will also need to edit the notebook file metadata.
(IPython notebook --> edit menu --> edit notebook metadata)
The metadata contains any extra information about the tutorial you may want to add.
The metadata must contain, at minimum, the following fields:

- link_name (the name of the link which will appear in the list of tutorials)
- author
- date (month year, e.g. 'July 2013')

Here is an example of one of these files: [FITS-header.ipynb](https://github.com/astropy/astropy-tutorials/blob/master/tutorials/FITS-header/FITS-header.ipynb) (be sure to hit the "raw" button to see the metadata).

You will also need to specify any python packages that the tutorial depends on.
Almost always this will include a specific version of `astropy`, and perhaps other affiliated packages.
You do this by placing a file called `requirements.json` in the directory that contains the tutorial notebook file.
To see in example of that, have a look at [requirements.json](https://github.com/astropy/astropy-tutorials/blob/master/tutorials/FITS-header/requirements.json).

When you feel like your tutorial is complete, push your local branch up to your
fork of the repository on github (by default, named 'origin'):

    git push origin Spectral-Line-Fitting

Then you will file a pull request against the main `astropy-tutorials`
repository for review.


Data Files
----------

### For tutorial authors

If your tutorial includes large data files (where large means >~ 1 MB), we
don't want them in the astropy/astropy-tutorials git repository, as that will
drastically slow down cloning the repository.  Instead, we encourage use of the
`astropy.utils.download_files` function, and will host data files on the
http://data.astropy.org server.  To make this easy, use the following procedure
when you have large data files.

* When writing your tutorial, just include the files in your tutorial's
  directory (e.g., ``tutorials/My-tutorial-name/mydatafile.fits``).  Those who
  are reviewing your tutorial will have to download them, but they would need
  them anyway, so it's ok. _IMPORTANT_: when you add or modify data files, make
  sure the only thing in that commit involves the data files.  That is, do
  _not_ edit your notebook and add/change data files in the same commit.  This
  will make it much easier to remove the data files when your tutorial is
  actually merged.

* To actually access your data files in the notebook, do something like this at
  the top of the notebook:

      from astropy.utils.data import download_file

      tutorialpath = ''
      mydatafilename1 = download_file(tutorialpath + 'mydatafile1.fits', cache=True)
      mydatafilename2 = download_file(tutorialpath + 'mydatafile2.dat', cache=True)

  And then use them like this:

      fits.open(mydatafilename1)
      ...
      with open(mydatafilename2) as f:
          ...

  If you do this, the only change necessary on merging your notebook will be to
  set `tutorialpath` to
  ``'http://data.astropy.org/tutorials/My-tutorial-name/'``.


### For repository maintainers

If this above procedure is followed, you only need to do these steps when merging your pull request:

1. Do ``git rebase -i`` and delete the commits that include the data files
2. Upload the data files to ``http://data.astropy.org/tutorials/My-tutorial-name/``
3. Update the `tutorialpath` variable.
