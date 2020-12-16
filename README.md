# sphinx-tutorial

Steps to make beautiful documentations using sphinx. Follow the demonstration video here ([Youtube](https://www.youtube.com/watch?v=b4iFyrLQQh4&t)).

More themes and packages here: [sphinx-themes.org](https://sphinx-themes.org/).

## Step 1: Installing Sphinx

    sudo apt-install python-sphinx -y
    
## Step 2: Setting up the documentation folder

Assume that the project directory is `my_project` and is stored inside `/home/$USER/. This is the top level module that we're concerned with.

    my_project
    ├── __init__.py
    ├── models
    │   ├── __init__.py
    │   ├── pytorch.py
    │   └── tensorflow.py
    └── utils
        ├── __init__.py
        ├── algorithms.py
        └── ioutils.py

2.1. Navigate into the current working directory (`my_project`).

        $ cd /home/$USER/my_project
        $ pwd
        /home/naveen/my_project

2.2. Create a folder where documentation will be stored. Here we call it `docs`.

        $ mkdir docs
        $ cd docs
        $ pwd
        /home/naveen/my_project/docs

2.3. Perform Sphinx quickstart.

        $ sphinx-quickstart
   
   In the prompt, keep most of the settings to default (just hit [enter] when prompted) except for the following:
   
   a. `Project name`: Enter an appropriate name. The project name will occur in several places in the built documentation.
   
   b. `Author name(s)`: Add an appropriate name. 
   
   c. `autodoc: automatically insert docstrings from modules (y/n) [n]`: Set this to `y`
   
   d. `> Create Makefile? (y/n) [y]`: Set this to `y`
   
   e. `> Create Windows command file? (y/n) [y]`: Set this to `n` (If not working on windows).

2.4. After this setup, your documentation folder (`docs`) will look like this:

        $ tree
        .
        ├── _build
        ├── conf.py
        ├── index.rst
        ├── Makefile
        ├── _static
        └── _templates

   Note the `conf.py` file above. We will be making changes to this file.

## Step 3: Updating the configurations (source files, themes, docstrings styles etc)

3.1. Open `conf.py` and edit the path to the project folder. To do so, uncomment the following lines, 

        # import os
        # import sys
        # sys.path.insert(0, os.path.abspath('.'))
   
   and modify the string inside `abspath` to point to the folder for which you want to generate documentation:
   
        import os
        import sys
        sys.path.insert(0, os.path.abspath('..'))
   
   Note, here we set the root to be '..' as the parent of the `docs` folder (current working directory) is `my_project`. You can also make this point to a submodule (such as `../utils`) and it would generate documentation specific to the module.

3.2. Update the theme for the HTML files. Here we shall use read-the-docs theme.

    a. Install read-the-docs theme for sphinx
    
        $ sudo pip install sphinx_rtd_theme
    
    b. Find the following line in `conf.py`:
    
        html_theme = 'alabaster'
        
      and change it to:
        
        html_theme = 'sphinx_rtd_theme'

3.3. Add Napoleon to the extensions to parse [Google style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) docstrings.

    a. Install napoleon
    
        $ pip install sphinxcontrib-napoleon
    
    b. Add napoleon to the extensions in `conf.py`. If you followed the above process, the extension `sphinx.ext.autodoc` would be present in the extensions list.
    
        extensions = [
            'sphinx.ext.autodoc',
        ]
        
      To this list, add napoleon:
      
        extensions = [
            'sphinx.ext.autodoc', 'sphinxcontrib.napoleon'
        ]

    Now the configuration file is ready. The next step will generate the documentation.

## Step 4: Generating documentation

1. Ensure that you are inside the `docs` folder as above.

        $ pwd
        /home/naveen/my_project/docs

2. Run `sphinx-apidoc` with the following format:
        
        sphinx-apidoc -o <documentation_output_path> <module_path>
   
   In our example, we will set documentation output path to be the `docs` folder, and the module path to be the `my_project` folder.
   
        $ sphinx-apidoc -o . ..

3. Time to build the documentation. Run `make html`.

        $ make html
        
4. Done! The documentation would have been generated inside the `_build/html` folder. Check it out using:

        $ firefox _build/html/index.html
