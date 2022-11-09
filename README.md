# iBilik Scraper

Date commence:   October 2022

Maintainer: @wazeefz

Developer: @wazeefz

Objective: To scrape `https://www.ibilik.my` to get metadata from several categories.

Version: 1.0.0

Date modified: 1 November 2022

## Package Dependecies:
 
1. pacman
2. RSelenium
3. RPostgreSQL
4. tidyverse
5. rvest
6. data.table

## Functions:

1. getprodlink()

     1.1 Description:
     
     A function that gets the list of links per category. A crucial step when scraping as this determines the URL to be scraped.
     
     1.2 Usage:
     
        getprodlink(url_listing)
     
     1.3 Arguments:
     
        url_listing
     
     A list of all the categories' URL. The dataset that used to be input in the argument is available in lelong.fcn.r.
     
     1.4 Output:
        The function will return a dataset of all the categories' links
