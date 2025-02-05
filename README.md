## FlipBook

This tool provides a simple user interface for quickly flipping through images stored in some directory on your computer.
It also optionally shows a form where you can take notes or answer questions about each image or set of images.

### Example uses:
- manual curation / review of sequencing data visualization images such as those generated by [REViewer](https://www.illumina.com/science/genomics-research/reviewer-visualizing-alignments-short-reads-long-repeat.html) for short tandem repeat loci
- machine learning training set creation
- reviewing a pile of photos

### Features:

- simple way to flip through many local image files using your web browser
- crawls a top-level directory to find .png, .jpeg, or .svg image files
- browser interface: home page lists all images
- browser interface: image pages show the image, an optional customizable form where you can take notes or answer questions about the image, next/previous page links, and optional other customizable info for context
- use subdirectories to group images. Any images found in the same subdirectory will be shown on the same page  
- optionally generate a static html website to share your images and form responses publicly. Example @ [https://broadinstitute.github.io/StrPileups/index.html](https://broadinstitute.github.io/StrPileups/index.html)

-------

[![PyPI version](https://img.shields.io/pypi/v/flipbook.svg?style=flat)](https://pypi.org/project/flipbook/)  [![Supported Python versions](https://img.shields.io/pypi/pyversions/flipbook.svg)](https://shields.io/)


### Install:

```
python3 -m pip install flipbook  
```

### Run:

```
python3 -m flipbook    # start server for all images in the current directory and subdirectories
```

Below are more example command lines. Run with `--help` to see all available options and their descriptions.

```
python3 -m flipbook -x temp -x keyword2  /path/dir-with-images    #  -x are keyword(s) of paths to skip and /path/dir-with-images is the top level dir to search instead of the current dir

python3 -m flipbook -t /path/user_responses.xls    #  change where user responses get saved (default: flipbook_form_responses.tsv)

python3 -m flipbook -m /path/metadata.tsv          #  provide a metadata table 
```

After the server is running, open your web browser to [http://localhost:8080](http://localhost:8080) to start reviewing images.


### Screenshots

Image page:

<img width="1781" alt="image" src="https://user-images.githubusercontent.com/6240170/208694093-b5e7c97d-d2a4-46ab-9852-5db7157a2a3d.png">


Home page:

<img width="1786" alt="image" src="https://user-images.githubusercontent.com/6240170/208694216-0a550c82-cca2-4212-b22f-134844da95a5.png">


These pages are also available on the [StrPileups website](https://broadinstitute.github.io/StrPileups/index.html), a GitHub Pages site that was generated by running 
```
python3 -m flipbook --generate-static-website
```


### Options:

- metadata table (`-m`)
  
  It's often useful to add extra info to the image pages to help with review - such as image descriptions, quality scores, etc.
  To enable this, there are several ways to specify arbitrary key-value pairs to add to specific image pages.
  The 1st way is to put a file called `flipbook_metadata.json` next to the image(s). All keys and values from this file
  will appear on that image page. The 2nd way is to use `-m` to pass in a metadata table 
  (`.tsv` or `.xls`) with a `Path` column + arbitrary other columns. If the `Path` value matches the relative directory containing 
  the image(s), entries from that row will be added to this image page.  
  
  Since the keys and values are treated as html, they can be used to add more complex info - such as
  colors, text formatting, <img ..> tags with images from other web pages, iframes containing entire sections of external pages, etc. 

- responses table (`-t`)

  As you fill in the forms at the top of the image pages, the responses are written to this table. If you later restart flipbook with the same `-t`, it will reload previous responses. You can also optionally use this table to provide additional columns to display - sometimes this can be more convenient than using `-m`. 
  
  *Default*: `flipbook_form_responses.tsv`

- custom form schema (`--form-schema-json`)  
   
  If you'd like to use non-default questions in the image page forms, you can specify the path or url of a .json file containing a custom form schema. For a description and examples of the expected format see [main/form_schema_examples](https://github.com/broadinstitute/flipbook/tree/main/form_schema_examples).  
  
- config file (`~/.flipbook_config`)

  Most settings that can be provided on the command line can also be set via this YAML config file instead. For example:

  *~/.flipbook_config*
  ```
  form-schema-json: /path/to/my-schema.json
  hide-metadata-on-home-page: true
  host: 127.0.0.1
  port: 8080
  ```

  
For the full list of command-line options, run:   
```
python3 -m flipbook --help
```

### Comparing reviews:

If 2 or more people review the same images, the reviews can be compared using the `compare_form_response_tables` script.  

For example:
```
python3 -m compare_form_response_tables egor_flipbook_form_responses.tsv  ben_flipbook_form_responses.tsv -o combined_responses.tsv -s1 egor -s2 ben
```

This script will print concordance stats, and output a `combined_responses.tsv` table that contains one image per row as well as each person's review of the image.  
  
To see the full list of args and descriptions, run `python3 -m compare_form_response_tables --help`

### Development:

To create a local dev instance, run

```
git clone git@github.com:broadinstitute/flipbook.git
cd flipbook

# start server in dev mode so it reloads code on change
python3 -m flipbook /path/dir-with-images --dev-mode
```

### Citation:

If you would like to cite FlipBook in your publication, please cite:

```
Dolzhenko E, Weisburd B, Ibañez K, Rajan-Babu IS, Anyansi C, Bennett MF, Billingsley K, Carroll A, Clamons S, Danzi MC, Deshpande V, Ding J, Fazal S, Halman A, Jadhav B, Qiu Y, Richmond PA, Saunders CT, Scheffler K, van Vugt JJFA, Zwamborn RRAJ; Genomics England Research Consortium; Chong SS, Friedman JM, Tucci A, Rehm HL, Eberle MA. REViewer: haplotype-resolved visualization of read alignments in and around tandem repeats. Genome Med. 2022 Aug 11;14(1):84. doi: 10.1186/s13073-022-01085-z. PMID: 35948990; PMCID: PMC9367089.
```
