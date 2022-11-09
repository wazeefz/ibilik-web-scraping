

#Web Scraping ibilik

library(rvest)
library(RSelenium)
library(tidyverse)
library(data.table)

######################################################################################################################################################

date = gsub("-", "_", Sys.Date())

url = "https://www.ibilik.my/rooms?location_search_name=&page=1"

driver = rsDriver(port = as.integer(sample(1000:10000, 1)), browser = "chrome", chromever = "105.0.5195.19")

remDr = driver[["client"]]

remDr$navigate(url)

Sys.sleep(.5)

######################################################################################################################################################

#Introducing node function to convert node name into readable R statement

nodefxn = function(nodename)  {
  node = (nodename)
  node = gsub(" ",".",node)
  node = gsub(":","\\\\:",node)
  
  return(node)
  
}

######################################################################################################################################################

#GETTING PAGE LINK AND BILIK LINK


ibiliknotfound = nodefxn("rslt-nt-fnd")
ibiliknotfound = paste0("div.",ibiliknotfound, " h1")

resultnotfound = read_html(remDr$getPageSource()[[1]]) %>%
  html_nodes(ibiliknotfound) %>% html_text()

biliknode = nodefxn("col-md-4")
biliknode = paste0("div.",biliknode, " a")

k = 1
ibilikpagelink = vector()
biliklink = vector()

while(length(read_html(remDr$getPageSource()[[1]]) %>%
             html_nodes(ibiliknotfound) %>% html_text()) == 0){
  
        linkstart = ("https://www.ibilik.my/rooms?location_search_name=&page=")
        linknumber = k
        
        pagelink = paste0(linkstart,linknumber)
        
        remDr$navigate(pagelink)
        
        Sys.sleep(0.1)
        
        ibilikpagelink = append(ibilikpagelink,pagelink)
        
        Sys.sleep(0.1)
        
        newbiliklink = read_html(remDr$getPageSource()[[1]]) %>%
          html_nodes(biliknode) %>% html_attr("href")
        
        biliklink = append(biliklink,newbiliklink)
        
        
        # cat(k,"\n")
        
        k = k + 1
        
}

######################################################################################################################################################

# EXTRACTING DETAILS


ibilikdata = data.frame()

for(i in 1:length(biliklink)){
  
  remDr$navigate(biliklink[i])
  
  Sys.sleep(0.1)



      nodename = nodefxn("title-loc")
      nodename = paste0("h2.",nodename)
      
      extract1 = read_html(remDr$getPageSource()[[1]]) %>%
        html_nodes(nodename) %>% html_text() 
      if(length(extract1)!=0){
        extract1 = extract1
      }else{
        extract1 = NA 
      }
      
    Location = str_squish(extract1)
      
      nodename = nodefxn("brightblue weigh-600")
      nodename = paste0("span.",nodename)
      
      extract2 = read_html(remDr$getPageSource()[[1]]) %>%
        html_nodes(nodename) %>% html_text() 
      if(length(extract2)!=0){
        extract2 = extract2
      }else{
        extract2 = NA 
      }
      
    Price = extract2
      
      nodename = nodefxn("room-details")
      nodename = paste0("div.",nodename," p")
      
      extract3 = read_html(remDr$getPageSource()[[1]]) %>%
        html_nodes(nodename) %>% html_text() 
      if(length(extract3)!=0){
        extract3 = extract3
      }else{
        extract3 = NA 
      }
      
    PropertyType = (str_squish(extract3))[1]
    
    RoomType = (str_squish(extract3))[3]
    
      nodename = nodefxn("row room-details")
      nodename = paste0("div.",nodename)
      
      extract4 = read_html(remDr$getPageSource()[[1]]) %>%
        html_nodes(nodename) %>% html_text() 
      if(length(extract4)!=0){
        extract4 = extract4
      }else{
        extract4 = NA 
      }
      
    Preferences = (str_squish(extract4))[1]
      
    Accomodation = (str_squish(extract4))[2]
      
    df = data.frame(Location = Location,
                    Price = Price,
                    PropertyType = PropertyType,
                    RoomType = RoomType,
                    Preferences = Preferences,
                    Accomodation = Accomodation)
    
    
    
    ibilikdata = rbind(ibilikdata, df)
    
    
    cat("Bilik Link :",i," \n")
    
    Sys.sleep(0.1)

}




######################################################################################################################################################


date = gsub("-", "_", Sys.Date())

data.table::fwrite(ibilikdata, paste0("//10.13.10.22/dataSandbox/scraping/misc/ibilik/ibilik_raw", date, ".csv"))














######################################################################################################################################################

biliknode = nodefxn("col-md-4")
biliknode = paste0("div.",biliknode, " a")

biliklink = read_html(remDr$getPageSource()[[1]]) %>%
  html_nodes(biliknode) %>% html_attr("href")


for(i in 1:length(biliklink)){
  remDr$navigate(biliklink[i])
}

remDr$navigate(biliklink[1])

