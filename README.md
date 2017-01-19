# EIS Archiver

This repo will describe the tools used to successfully Archive a given set of webpages. This is was developed for EPA's EIS documents serving catalouge, but it is generalized enough to be used for any webpage. The process involves 3 major steps:
1.	Generating a list of URLs to be archived.
2.	Running the [grab-site](https://github.com/edgi-govdata-archiving/grab-site) for the list of URLs as a warcfactory process that is constantly generating WARC format files for the supplied URLs.
3.	Verifying the quality of the generated WARCs using [warcat](https://github.com/chfoo/warcat)
4.	Optional wayback machine [pywb](https://github.com/ikreymer/pywb) that be used to playback the generated WARCs.

# Dependecies

Have the following dependecies installed in your machine before getting started with the harvesting proccess.

## List of URLs
Have a list of URLs in a text file, separate with newline characters.

## Docker
Install the Docker Engine on your machine, its available for Mac/Win/Linux, see [instructions](https://docs.docker.com/engine/installation/) for installation specific to your environment. Although this process has been only tested on Mac/Linux, it requires a bash shell. This process hasn't been tested for a Windows environment with a bash shell.

## pip
Install `pip` if its not installed already [here](https://pip.pypa.io/en/stable/installing/).

# Installation

## warcat
Install warcat using pip or source [here]().

## pywb
This tool can playback WARC files on your localhost, this can be used to verify the generated WARCs, full installation instructions [here](https://github.com/ikreymer/pywb)

# Usage
Once the dependecies, specifically the docker environment is installed 

## grab-site
Full install/reference [instructions](https://github.com/edgi-govdata-archiving/grab-site).

Get the pre-built docker container:
`docker pull slang800/grab-site`

### Running
To run the grab-site server, the following command in the terminal:
Can set the port to whatever you want after `-p` flag.
Set Volume path for data output (ie WARCs) after `-v` flag up to `:/data`

`docker run --detach -p [port]:[port] -v [path-to-folder-to-store-data]/grab-site-date:/data --name warcfactory slang800/grab-site`

Verify the container running using:

`docker ps`

The container `warcfactory` should appear on the output.

#### Starting a crawl

For a single URL crawl use the following command:

`docker exec warcfactory grab-site --no-offsite-links http://example.com`

Supplied in this repo is a shell script `pars-urls.sh` that just reads in a text file of URLs and runs the above command for each and every URLs.
To run the shell script in background and with no outputs run it as:

`nohup sh parse-urls.sh &`

The crawling will be done automatically for each URLs supplied in the text file.
If your machine is setup or exposed to be a public ip, you can monitor it at `localhost:[port]`, the port being the port the grab-site is running on. The dashboard will show current progress and status of the crawl.


# Deprecated
Following scripts are legacy scripts that are not being used in the current process of Archiving and generating warcs, but they still work. Currently the `grab-site` method was chose because of the valid WARCs it has been generating.

The `createWarc.py` python script that will parse through a giving csv with EIS ID's and use the wget function grab and package the EIS URL in a WARC format and download any documents associated with the EIS into a zip format.

The `wget` bash function has a option flag to package the response from the supplied URL into a `.warc` format which the Web ARCHive file format the Internet Archive uses to playback web sites.

The python script `createWarc.py` invokes a wget command as a subprocess in the script to package the response from the supplied URL into a warc format. The pdf links present in the html page cannot be preserved in the html therefore, they are being downloaded separately in a different wget call.

The EIS csv is supplied from this [parser](https://github.com/titaniumbones/eot-sprint-toolkit/tree/master/eis)
This script continues on the work done for the URL scraping list supplied by one of the groups from the Guerrilla Archiving Event taken place at University of Toronto on December 16, 2016.

## Dependecies
This script has only been tested on OSX and Linux environment. If you would like to contribute to support Windows please contribute.

1. wget function available in your bash shell
2. python 2.7

## Usage
Clone this repo then just simply run the python script `python createWarc.py`
Supplied is the csv, if you would like to customize this python or expand and modify the script to a take in generic datasets please modify the csv, base urls accordingly.

This script is specifically written for the Enivornment Impact Statement documents from the EPA [site](https://cdxnodengn.epa.gov/cdx-enepa-public/action/eis/search)
