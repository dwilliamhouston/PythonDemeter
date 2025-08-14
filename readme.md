# PythonDemeter

PythonDemeter is a tool for downloading the .epub files you don't have from a Calibre library. It does this by building a database of books it has seen based on some clever algorithms. At least, that's the idea.

(Demeter only allows scraping a host every 12 hours to prevent overloading the server.)

# Installation and Usage

You will need Python3 on your desktop computer and you need to put the demeter app in the root directory of the data directory where the OpenCalibre data files are installed.

## Scrape all hosts and store results in the directory ./books and only download the extension pdf

`demeter scrape run -d books -e pdf`

For the rest, use the built in help.

This tool can be used for whatever you want, enjoy.

## important note regarding extensions

The -e flag on the `scrape run` command only affects that specific run, the books are stored without any extension information in the database. In general that means that if you switch from the `-e epub` (default) to `-e mobi`, you will only download new books in the mobi extension. Books that were already present will not be re-downloaded in a different extension.

# Database

Demeter uses the OpenCalibre database that is available weekly to those that subscribe to the CSV level of 
<a href="https://www.buymeacoffee.com/opencalibre" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

# Scraping

When scraping a host, demeter does the following:

- Use the API to collect all book ids
- Check if there a new book ids since the previous scrape
- Use the API to get the details for all the new book ids
- Check the internal db if a book has already been downloaded
- Download the book if it isn't and add it to the internal db
- Mark the host as scraped so it won't do it again within 12 hours
- If the host failed, mark it as failed and disable it after a while

# all commands

```
$ demeter -h
demeter is CLI application for scraping calibre hosts and
retrieving books in epub format that are not in your local library.

Usage:
  demeter [command]

Available Commands:
  dl          download related commands
  help        Help about any command
  host        all host related commands
  scrape      all scrape related commands

$ demeter dl -h
download related commands

Usage:
  demeter dl [command]

Aliases:
  dl, download, downloads, dls

Available Commands:
  add          add a number of hashes to the database
  deleterecent delete all downloads from this time period
  list         list all downloads

$ demeter host -h
all host related commands

Usage:
  demeter host [command]

Available Commands:
  disable                  Disable a host
  disable --disable-all    Disable all enabled hosts
  enabled                  Make a host active
  enable --enable-all      Enable all hosts
  enable --enable-country  Enable all hosts from a particular country using country code  
  list                     List all hosts
  rm                       Delete a host
  stats                    Get host stats

$ demeter scrape -h
all scrape related commands

Usage:
  demeter scrape [command]

Available Commands:
  run         run all scrape jobs

$ demeter scrape run -h
demeter scrape run -h
Go over all defined hosts and if the last scrape
is old enough it will scrape that host.

Usage:
  demeter scrape run [flags]

Flags:
  -h, --help               help for run
  -d, --outputdir string   path to downloaded books to (default "books")
  -e,                      format of book(s) you want to download.  Options are (AZW, PDF, EPUB).
  -n, --stepsize int       number of books to request per query (default 50)
  -u, --useragent string   user agent used to identify to calibre hosts (default "demeter / v1")
  -w, --workers int        number of workers to concurrently download books (default 10)
  -a,                      search by authors. Must have search criteria in quotes.  example: -a "%Brad Thor%" will download all books by Brad Thor as author.
  -t,                      search by title of book.  Must have search criteria in quotes.  example: -t "%Python%" will download ll books with Python in the title of the book.
```
