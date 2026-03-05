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

                                                                                         
 42 [yaz-client] Notes:                                                                       
 43 - [yaz-client] is an information retrieval client that uses the Z39.50/SRU protocols to qu
 44     - Z39.50 is a standard protocol in libraries for:                                     
 45         - Sharing                                                                         
 46         - Querying                                                                        
 47         - Retrieving bibliographic information between library databases.                 
 48 - The [yaz-client] is a SRU (search/retrieve via URL) as well. The [yaz-client] allows us 
 49                                                                                           
 50 [yaz-client] Commandline Notes:                                                           
 51 - To access the man page for the [yaz-client] documentation, use:                         
 52     man yaz-client                                                                        
 53     This opens the program and will give us a list of [yaz-client] commands.              
 54 - To search quite a few bibliographic attributes, including many metadata fields, use:    
 55     man bib1-attr                                                                         
 56     *This is perfect in searching MARC record information.                                
 57 Link to [Bib-1 Attribute Set](https://www.loc.gov/z3950/agency/defns/bib1.html)           
 58 - To start the [yaz-client], use:                                                         
 59     yaz-client                                                                            
 60     *This creates a new commandline interface with a new prompt.                          
 61 - Then connect to a library's OPAC or discovery service using the [open] command:         
 62     open saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY                                 
 63     *This connects us to InfoCat!                                                         
 64 - Each search we do here uses a number that corresponds to the number in the [Bib-1 Attrib
 65 - In our example, we entered in:                                                          
 66     find @and @attr 1=4 "information" @attr 1=21 "library science"                        
 67         or the slightly shorter version:                                                  
 68     f @and @attr 1=4 "information" @attr 1=21 "library science"                           
 69     *There we pulled 723 records but it does not show the records. To see the records use:
 70     show 1                                                                                
 71     *This will show the first record. To see the second record, use:                      
 72     show 2                                                                                
 73     *and so on.                                                                           
 74     *To clear the screen on yaz-client, use:                                              
 75     ctrl+L                                                                                
 76 - To search a personal name, use:                                                         
 77     f @attr 1=1 "first name, last"                                                        
 78                                        

                                                                                      
 44 Opening yaz-client with -m                                                                
 45 - To open the yaz-client with -m, we use:                                                 
 46     yaz-client -m records.marc                                                            
 47 - To open the UKY portal againm we can use the up arrow key to scroll through previous com
 48     *After doing a search, we exited.                                                     
 49 - To look at the records we have, use:                                                    
 50     ls -l                                                                                 
 51     *This will also show records.marc                                                     
 52 - To look at this file, use:                                                              
 53     head records.marc                                                                     
 54     *What pulls up is not human friendly. It is showing us the MARC record.               
 55 - To see what files are, use:                                                             
 56     file records.marc                                                                     
 57 - To see what all was downloaded when we installed yaz, use:                              
 58     apropos yaz                                                                           
 59 - To-?                                                                                    
 60     yaz-marcdump -o json records.marc > records.json                                      
 61 - Then to transfer the record into a json filem use:                                      
 62     head records.json                                                                     
 63     *A json file is a standard-based format for structured data. json is basically a compe
 64 - To examine these files in a better way, use:                                            
 65     jq . records.json > records_formatted.json                                            
 66     *Then to open that up, use:                                                           
 67     vi records_formatted.json                                                             
 68 - To pull out all the fields, use:                                                        
 69     jq '.fields[]'records_formatted.json                                                  
 70 - We can also specify fields, for example, the 650 field:                                 
 71     jq '.fields[] | select(has("650")) | ["650].subfields[] | select(has("a")) | .a' recor
 72                                                                                       


                                                                                            
41                                                                                            
42 ### Module 7 Notes: Installing the Apache Web Server###                                    
43 - Goal: To install and use the Apache web server, which is one of the most popular web serv
44                                                                                            
45 ***Command Line Notes:                                                                     
46     - To figure out the specific package name, use:                                        
47         apt search apache2 | head                                                          
48     - The first package returned is what is needed:                                        
49         apache2/noble-updates, noble securuty 2.4.58-1 ubuntu8.1amd64                      
50     - To install apache, we begin with:                                                    
51         cd /var/                                                                           
52         ls                                                                                 
53         sudo apt install apache2                                                           
54     - To get information about the status of apache2 and make sure it is enabled and runnin
55         systemctl status apache2                                                           
56     - Text-Based Web Browser: the command line browser chosen was [elinks]. To instal, use:
57         sudo apt install elinks                                                            
58     - The loopback address is named localhost. There are two commands that allow us to chec
59         elinks 127.0.0.1                                                                   
60             or                                                                             
61         elinks localhost                                                                   
62     - To get the IP address, use:                                                          
63         ip a                                                                               
64         (This IP address will be located after the 2nd 'inet')                             
65     - To get to the Apache2 default page, use:                                             
66         elinks http://10.128.0.3                                                           
67     - To see the default page outside of our VM, we need our servers public IP address, whi
68                                                                                            
                                                                                           
                                                                                              








