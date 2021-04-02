## reviewer #2

This tool starts a simple image server that lets you quickly flip through image files from a local directory using your web browser.
Also, it optionally shows a customizable form where you can take notes or answer questions about each image or set of images. 


### Example use cases:
- manual curation / review of next-gen sequencing data visualization images such as those generated by [REViewer](https://www.illumina.com/science/genomics-research/reviewer-visualizing-alignments-short-reads-long-repeat.html) for short tandem repeat loci
- machine learning training set creation
- review photos

### Features:

- simple way to flip through many local image files using your web browser
- crawls a top-level directory to find .png, .jpeg, or .svg image files
- top-level web page lists all images
- each image page shows the image, an optional customizable form where you can take notes or answer questions about the image, and next/previous page links
- use subdirectories to group images. Any images found in the same subdirectory will be shown on the same image page. Also, `reviewer2_metadata.json` files can optionally be added to a subdirectory and provide metadata to show at the top of a specific image page.

### TODO - not yet implemented:

- web forms, sqlite db storage
- global config file (for customizing forms, etc.)
- keyboard shortcuts

### Install and Run:

```
# install:
python3 -m pip install reviewer2  

# start server (optional args: -x, -d,  env-vars:  DEBUG, HOST (is "127.0.0.1" by default), PORT (is "8080" by default)): 
#    -x specify image path keyword(s) to exclude from review  
#    -d specifies top level directory to search for images (if not set, it will search under the current dir)
python3 -m reviewer2 -x temp -x keyword2 -d /path/dir-with-images  

# launch web browser
open localhost:8080
```

### Develpoment:

To create a local dev instance, run

```
git clone git@github.com:bw2/reviewer2.git

cd reviewer2

# start server in debug mode:
DEBUG=True python3 -m reviewer2 -d /path/dir-with-images  # start dev server
```
