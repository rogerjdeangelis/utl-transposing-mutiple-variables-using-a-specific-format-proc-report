# utl-transposing-mutiple-varaibles-using-a-specific-format-proc-report
Transposing mutiple varaibles using a specific format proc report .
    Transposing mutiple varaibles using a specific format proc report                                                                 
                                                                                                                                      
    https://tinyurl.com/y8srvzkt                                                                                                      
    https://github.com/rogerjdeangelis/utl-transposing-mutiple-variables-using-a-specific-format-proc-report                          
                                                                                                                                      
    This provides an output table which you can further format.                                                                       
                                                                                                                                      
    SAS-L                                                                                                                             
    https://mail.google.com/mail/u/0/#inbox/FMfcgxwBVMjbGpBdsTMPVNTsJqFfnzTq                                                          
                                                                                                                                      
    INPUT                                                                                                                             
    =====                                                                                                                             
                                                                                                                                      
    data have;                                                                                                                        
    input Loc$ Var$ val cnt;                                                                                                          
    cards4;                                                                                                                           
    Loc1 var1 0.9 12                                                                                                                  
    Loc2 var1 0.8 10                                                                                                                  
    Loc3 var1 0.9 9                                                                                                                   
    Loc4 var1 0.75 3                                                                                                                  
    Loc1 var2 1.2 15                                                                                                                  
    Loc2 var2 2.2 7                                                                                                                   
    Loc3 var2 3.5 20                                                                                                                  
    Loc4 var2 4.2 5                                                                                                                   
    ;;;;                                                                                                                              
    run;quit;                                                                                                                         
                                                                                                                                      
    /*                                                                                                                                
     WORK.HAVE total obs=8                                                                                                            
                            VAR_                                                                                                      
      LOC     VAR     VALUE    COUNTS                                                                                                 
                                                                                                                                      
      Loc1    var1     0.90      12                                                                                                   
      Loc2    var1     0.80      10                                                                                                   
      Loc3    var1     0.90       9                                                                                                   
      Loc4    var1     0.75       3                                                                                                   
      Loc1    var2     1.20      15                                                                                                   
      Loc2    var2     2.20       7                                                                                                   
      Loc3    var2     3.50      20                                                                                                   
      Loc4    var2     4.20       5                                                                                                   
    */                                                                                                                                
                                                                                                                                      
    EXAMPLE OUTPUT                                                                                                                    
    --------------                                                                                                                    
                                                                                                                                      
     WORK.WANT total obs=4                                                                                                            
                                                                                                                                      
      LOC        VAR1           VAR2                                                                                                  
                                                                                                                                      
      Loc1    0.90 ( 12 )    1.20 ( 15 )                                                                                              
      Loc2    0.80 ( 10 )    2.20 ( 7 )                                                                                               
      Loc3    0.90 ( 9 )     3.50 ( 20 )                                                                                              
      Loc4    0.75 ( 3 )     4.20 ( 5 )                                                                                               
                                                                                                                                      
                                                                                                                                      
    PROCESS                                                                                                                           
    =======                                                                                                                           
                                                                                                                                      
    * there is  bug in ods report. Report does not honor the ods destination;                                                         
    * ods output just duplicates 'out=';                                                                                              
                                                                                                                                      
    * I think you can do it with one report, but complex computes are not worth it?;                                                  
    proc report data=have out=havRpt(drop=_break_) missing nowd;                                                                      
    cols loc var, ( val cnt);                                                                                                         
    define loc / group;                                                                                                               
    define val / analysis sum;                                                                                                        
    define cnt /analysis sum;                                                                                                         
    define var / across;                                                                                                              
    run;quit;                                                                                                                         
                                                                                                                                      
    /*                                                                                                                                
    WORK.HAVRPT total obs=4                                                                                                           
                                                                                                                                      
      LOC     _C2_    _C3_    _C4_    _C5_    _BREAK_                                                                                 
                                                                                                                                      
      Loc1    0.90     12      1.2     15                                                                                             
      Loc2    0.80     10      2.2      7                                                                                             
      Loc3    0.90      9      3.5     20                                                                                             
      Loc4    0.75      3      4.2      5                                                                                             
                                                                                                                                      
    */                                                                                                                                
                                                                                                                                      
    data want;                                                                                                                        
      set havRpt;                                                                                                                     
      var1=catx(" ",put(_c2_,5.2),'(',put(_c3_,2.),')');                                                                              
      var2=catx(" ",put(_c4_,5.2),'(',put(_c5_,2.),')');                                                                              
      keep loc var1 var2;                                                                                                             
    run;quit;                                                                                                                         
                                                                                                                                      
                                                                                                                                      
    OUTPUT;                                                                                                                           
                                                                                                                                      
    Up to 40 obs WORK.WANT total obs=4                                                                                                
                                                                                                                                      
    Obs    LOC        VAR1           VAR2                                                                                             
                                                                                                                                      
     1     Loc1    0.90 ( 12 )    1.20 ( 15 )                                                                                         
     2     Loc2    0.80 ( 10 )    2.20 ( 7 )                                                                                          
     3     Loc3    0.90 ( 9 )     3.50 ( 20 )                                                                                         
     4     Loc4    0.75 ( 3 )     4.20 ( 5 )                                                                                          
                                                                                                                                      
                                                                                                                                      
