# reviewer2

This simple tool starts an image server that lets you quickly flip through images from a local directory using your web browser.
It optionally shows a customizable form where you can take notes or answer questions about each image or set of images. 

Example use-cases:
- manual curation / review of next-gen sequencing data visualization images such as those generated by the [REViewer tool](https://www.illumina.com/science/genomics-research/reviewer-visualizing-alignments-short-reads-long-repeat.html) for short tandem repeat loci
- machine learning training set creation
- review old photos

### Features

- simple way to flip through many local image files using your web browser
- crawls a top-level directory to find .png, .jpeg, or .svg image files
- top-level web page lists all images
- each image page shows the image, an optional customizable form where you can take notes or answer questions about the image, and next/previous page links
- use subdirectories to group images - any images found in the same subdirectory will be shown on the same image page. Also, `reviewer2_metadata.json` files can optionally be added to a subdirectory and provide metadata to show at the top of a specific image page.

### Install and Run

```
# install
python3 -m pip install reviewer2  

# start server
python3 -m reviewer2 --directory /path/dir-with-images   

open localhost:8080 in web browser
```

