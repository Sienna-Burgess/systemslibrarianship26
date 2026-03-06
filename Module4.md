# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026                             
### Module 4: LAMP Server                                                                   

                                                                                                                                                        
### Module 7 Notes: Installing the Apache Web Server###                                    
 - Goal: To install and use the Apache web server, which is one of the most popular web serv
                                                                                            
 ***Command Line Notes:                                                                     
 - To figure out the specific package name, use:                                        
         apt search apache2 | head                                                          
 - The first package returned is what is needed:                                        
         apache2/noble-updates, noble securuty 2.4.58-1 ubuntu8.1amd64                      
 - To install apache, we begin with:                                                    
         cd /var/                                                                           
         ls                                                                                 
         sudo apt install apache2                                                           
 - To get information about the status of apache2 and make sure it is enabled and runnin
         systemctl status apache2                                                           
 - Text-Based Web Browser: the command line browser chosen was [elinks]. To instal, use:
         sudo apt install elinks                                                            
 - The loopback address is named localhost. There are two commands that allow us to chec
         elinks 127.0.0.1                                                                   
             or                                                                             
         elinks localhost                                                                   
 - To get the IP address, use:                                                          
         ip a                                                                               
         (This IP address will be located after the 2nd 'inet')                             
 - To get to the Apache2 default page, use:                                             
         elinks http://10.128.0.3                                                           
 - To see the default page outside of our VM, we need our servers public IP address, whi




                                                                
                                                                                           
                                                                                              









