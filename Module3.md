# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026                             
### Module 3: CLI Work                                                                     

### Week 5 Notes:                                                                          
- Goal: To search using the [grep] command to search through data.                         
- Context: Utilizing the [grep] command to search and sort through large amounts of biblio 

 ** [grep] Command Line Notes:**                                                           
 Note: (filename) in these notes is where you enter name of the file you are searching in, 
 Note: "word" in these notes is what thing you want to search for, you have to have the wor
 Note: [grep] is case sensitive but there is a way to make it ignore case sensetivity.     
 [grep] Commandline Notes:                                                                 
- Basic [grep] search: grep "word" (file name)                                            
- To ignore wordcase sensitivity: grep -i "word" (file name)                              
- To search lines that do not match out string, use the combination (by having the "i" in 
- To remove the first row (best for spreadsheets with headers), you will need to have the 
- To count the lines with a certain word, use: grep -ic "word" (file title)               
- To conduct a Boolean OR search in a data sheet, you will need the quotations and parenth
- To search more in a Boolean search, add more vertical bars: grep -Ei "(word1|word2|word3


### Week 6 Notes- the [sudo] Command and [yaz-client]:                                    
The [sudo] command can be used to:                                                        
     - Modify files                                                                        
     - Create directories                                                                  
     - Perform other maintanence tasks needed to install and manage software               
                                                                                          
 [sudo] Commandline Notes:                                                                 
 - To create a "data" directory, for example in: /usr/local/bin  first, you open that file:
     cd /usr/local/bin                                                                     
   Then use sudo:                                                                          
     sudo mkdir data                                                                       
                                                                                           
 - To check for updates in your system, use the command:                                   
     sudo apt update                                                                       
    If there are updates that are needed, we will use the command for the upgrades:         
     sudo apt upgrade                                                                      

 - To remove cashed package files and free up disc space, use the command:                 
     sudo apt clean                                                                        
                                                                                           
 - To pull up simple help pages and examples of the most commonly used commands, use:      
     apt search tldr                                                                       
   *Cool note: tldr means too long don't read                                             
                                                                                         
 - To get more specific information about a package, we use the command:                   
     apt show tldr                                                                         
   This will give us the package name, the version numberm and the url.  

                                                                                         
 [yaz-client] Notes:                                                                       
  - [yaz-client] is an information retrieval client that uses the Z39.50/SRU protocols to qu
  - Z39.50 is a standard protocol in libraries for:                                     
    - Sharing                                                                         
    - Querying                                                                        
    - Retrieving bibliographic information between library databases.                 
 - The [yaz-client] is a SRU (search/retrieve via URL) as well. The [yaz-client] allows us 
                                                                                         
[yaz-client] Commandline Notes:                                                           
  - To access the man page for the [yaz-client] documentation, use:                         
      man yaz-client                                                                        
     This opens the program and will give us a list of [yaz-client] commands.              
 - To search quite a few bibliographic attributes, including many metadata fields, use:    
      man bib1-attr                                                                         
    *This is perfect in searching MARC record information.                                
 Link to [Bib-1 Attribute Set](https://www.loc.gov/z3950/agency/defns/bib1.html)           
 - To start the [yaz-client], use:                                                         
      yaz-client                                                                            
    *This creates a new commandline interface with a new prompt.                          
 - Then connect to a library's OPAC or discovery service using the [open] command:         
      open saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY                                 
    *This connects us to InfoCat!                                                         
 - Each search we do here uses a number that corresponds to the number in the [Bib-1 Attrib
 - In our example, we entered in:                                                          
      find @and @attr 1=4 "information" @attr 1=21 "library science"                        
        or the slightly shorter version:                                                  
      f @and @attr 1=4 "information" @attr 1=21 "library science"                           
    *There we pulled 723 records but it does not show the records. To see the records use:
      show 1                                                                                
    *This will show the first record. To see the second record, use:                      
      show 2                                                                                
    *and so on.                                                                           
    *To clear the screen on yaz-client, use:                                              
      ctrl+L                                                                                
  - To search a personal name, use:                                                         
      f @attr 1=1 "first name, last"                                                        
                                         

### Opening yaz-client with -m                                                                
 - To open the yaz-client with -m, we use:                                                 
      yaz-client -m records.marc                                                            
 - To open the UKY portal againm we can use the up arrow key to scroll through previous com
    *After doing a search, we exited.                                                     
 - To look at the records we have, use:                                                    
      ls -l                                                                                 
     *This will also show records.marc                                                     
 - To look at this file, use:                                                              
      head records.marc                                                                     
     *What pulls up is not human friendly. It is showing us the MARC record.               
 - To see what files are, use:                                                             
      file records.marc                                                                     
 - To see what all was downloaded when we installed yaz, use:                              
      apropos yaz                                                                           
 - To-?                                                                                    
      yaz-marcdump -o json records.marc > records.json                                      
 - Then to transfer the record into a json filem use:                                      
      head records.json                                                                     
     *A json file is a standard-based format for structured data. json is basically a compe
 - To examine these files in a better way, use:                                            
      jq . records.json > records_formatted.json                                            
     *Then to open that up, use:                                                           
      vi records_formatted.json                                                             
 - To pull out all the fields, use:                                                        
      jq '.fields[]'records_formatted.json                                                  
 - We can also specify fields, for example, the 650 field:                                 
      jq '.fields[] | select(has("650")) | ["650].subfields[] | select(has("a")) | .a' record

      
