## reviewer #2

This tool starts a simple image server that lets you quickly flip through image files from a local directory using your web browser.
Also, it optionally shows a customizable form where you can take notes or answer questions about each image or set of images.

### Example uses:
- manual curation / review of sequencing data visualization images such as those generated by [REViewer](https://www.illumina.com/science/genomics-research/reviewer-visualizing-alignments-short-reads-long-repeat.html) for short tandem repeat loci
- machine learning training set creation
- reviewing a pile of photos

### Features:

- simple way to flip through many local image files using your web browser
- crawls a top-level directory to find .png, .jpeg, or .svg image files
- web interface: home page lists all images
- web interface: image pages show the image, an optional customizable form where you can take notes or answer questions about the image, next/previous page links, and optional other customizable info for context
- use subdirectories to group images. Any images found in the same subdirectory will be shown on the same page  

-------

[![PyPI version](https://img.shields.io/pypi/v/reviewer2.svg?style=flat)](https://pypi.org/project/reviewer2/)  [![Supported Python versions](https://img.shields.io/pypi/pyversions/reviewer2.svg)](https://shields.io/)


### Install:

```
python3 -m pip install reviewer2  
```

### Run:

```
python3 -m reviewer2    # start server for all images in the current directory and subdirectories
```

Below are more examples (all args are optional). Run it with `--help` to see the full list of args and descriptions.

```
python3 -m reviewer2 -x temp -x keyword2  /path/dir-with-images    #  -x are keyword(s) of paths to skip and /path/dir-with-images is the top level dir to search instead of the current dir

python3 -m reviewer2 -t /path/user_responses.xls    #  change where user responses get saved (default: reviewer2_form_responses.tsv)

python3 -m reviewer2 -m /path/metadata.tsv          #  provide a metadata table 
```

After the server is running, open your web browser to [http://localhost:8080](http://localhost:8080) to start reviewing images.

### Optional Inputs:

- responses table (`-t`)

  As you fill in the forms at the top of the image pages, the responses are written to this table. 
  
  *Default*: `reviewer2_form_responses.tsv`

- metadata table (`-m`) :
  
  It's often useful to add extra info to the image pages to help with review - such as image descriptions, quality scores, etc.
  To enable this, there are several ways to specify arbitrary key-value pairs to add to specific image pages.
  The 1st way is to put a file called `reviewer2_metadata.json` next to the image(s). All keys and values from this file
  will appear on that image page. The 2nd way is to use `-m` to pass in a metadata table 
  (`.tsv` or `.xls`) with a `Path` column + arbitrary other columns. If the `Path` value matches the relative directory containing 
  the image(s), entries from that row will be added to this image page.  
  
  Since the keys and values are treated as html, they can be used to add more complex info - such as
  colors, text formatting, <img ..> tags with images from other web pages, iframes containing entire sections of external pages, etc. 

- custom form schema (`--form-schema-json`):  
   
  Path of .json file containing a custom form schema. For the expected format see the FORM_SCHEMA value in 
  [https://github.com/broadinstitute/reviewer2/blob/main/reviewer2/__init__a.py](https://github.com/broadinstitute/reviewer2/blob/main/reviewer2/__init__a.py)
  
- `~/.reviewer2_config`  

  Most settings that can be provided on the command line can optionally be set via this YAML config file instead.
  
  
For more details, run:   
```
python3 -m reviewer2 --help
```

### Comparing reviews:

If 2 or more people review the same images, the reviews can be compared using the `compare_form_response_tables` script.  

For example:
```
python3 -m compare_form_response_tables egor_reviewer2_form_responses.tsv  ben_reviewer2_form_responses.tsv -o combined_responses.tsv -s1 egor -s2 ben
```

To see the full list of args and descriptions, run `python3 -m compare_form_response_tables --help`

### Development:

To create a local dev instance, run

```
git clone git@github.com:broadinstitute/reviewer2.git
cd reviewer2

# start server in dev mode so it reloads code on change
python3 -m reviewer2 /path/dir-with-images --dev-mode
```
