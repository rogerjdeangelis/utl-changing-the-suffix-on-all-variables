# utl-changing-the-suffix-on-all-variables
Change the suffix on all variables.
    Change the suffix on all variables                                                                                                
                                                                                                                                      
    Rename D1 - D99 to D000 - D495                                                                                                    
                                                                                                                                      
    see                                                                                                                               
    https://tinyurl.com/yd8oc4r3                                                                                                      
    https://communities.sas.com/t5/SAS-Programming/How-to-rename-all-the-column-names-of-a-dataset/m-p/509743/highlight/true#M137083  
                                                                                                                                      
    github                                                                                                                            
    https://github.com/rogerjdeangelis/utl-changing-the-suffix-on-all-variables                                                       
                                                                                                                                      
                                                                                                                                      
    INPUT  (99 variables D1-D99)                                                                                                      
    ============================                                                                                                      
                                                                                                                                      
    Middle Observation(1 ) of Last dataset = WORK.HAVE - Total Obs 1                                                                  
                                                                                                                                      
     -- NUMERIC --                                                                                                                    
                                                                                                                                      
    Variable   Type/Length   Value                                                                                                    
    --------   -----------   -----                                                                                                    
    D1            N8           1                                                                                                      
    D2            N8           1                                                                                                      
    D3            N8           1                                                                                                      
    ...                                                                                                                               
    D97           N8           1                                                                                                      
    D98           N8           1                                                                                                      
    D99           N8           1                                                                                                      
                                                                                                                                      
                                                                                                                                      
    EXAMPLE OUTPUT                                                                                                                    
    ==============                                                                                                                    
                                                                                                                                      
     -- NUMERIC --                                                                                                                    
                                                                                                                                      
    Variable   Type/Length   Value                                                                                                    
    --------   -----------   -----                                                                                                    
    D000          N8           1     D1 renamed to D000                                                                               
    D002          N8           1                                                                                                      
    D003          N8           1                                                                                                      
    ...                                                                                                                               
    D097          N8           1                                                                                                      
    D098          N8           1                                                                                                      
    D099          N8           1                                                                                                      
                                                                                                                                      
                                                                                                                                      
    PROCESS (with input)                                                                                                              
    ====================                                                                                                              
                                                                                                                                      
    proc datasets lib=work kill;                                                                                                      
    run;quit;                                                                                                                         
                                                                                                                                      
    * create data;                                                                                                                    
    data have;                                                                                                                        
           array d[99] (99*1);                                                                                                        
    run;quit;                                                                                                                         
                                                                                                                                      
                                                                                                                                      
    data log;                                                                                                                         
                                                                                                                                      
        if _n_ = 0 then do; %let rc=%sysfunc(dosubl('                                                                                 
           data _null_;                                                                                                               
             length rens $32756;                                                                                                      
             do ren=1 to 99;                                                                                                          
               rens= catx(" ",rens,cats("D",put(ren,3.),"=D",put(5*(ren-1),z3.)));                                                    
             end;                                                                                                                     
             call symputx("rens",rens);                                                                                               
           run;quit;                                                                                                                  
           '));                                                                                                                       
        end;                                                                                                                          
                                                                                                                                      
        rc=dosubl('                                                                                                                   
           proc datasets ;                                                                                                            
              modify have;                                                                                                            
              rename  &rens.;                                                                                                         
           run;quit;                                                                                                                  
           %let cc=&syserr;                                                                                                           
        ');                                                                                                                           
                                                                                                                                      
        if symgetn('cc')=0 then status='Rename Successful';                                                                           
        else status='Rename Failed';                                                                                                  
                                                                                                                                      
    run;quit;                                                                                                                         
                                                                                                                                      
    LOG                                                                                                                               
                                                                                                                                      
    NOTE: Renaming variable D1 to D000.                                                                                               
    NOTE: Renaming variable D2 to D005.                                                                                               
    NOTE: Renaming variable D3 to D010.                                                                                               
    ....                                                                                                                              
    NOTE: Renaming variable D97 to D480.                                                                                              
    NOTE: Renaming variable D98 to D485.                                                                                              
    NOTE: Renaming variable D99 to D490.                                                                                              
                                                                                                                                      
