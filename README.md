# utl-elegant-algorithm-to-normalize-messy-string-data
Elegant algorithm to normalize messy string data  
    %let pgm=utl-elegant-algorithm-to-normalize-messy-string-data;

    Elegant algorithm to normalize messy string data

    gitHub
    https://tinyurl.com/4rhm24em
    https://github.com/rogerjdeangelis/utl-elegant-algorithm-to-normalize-messy-string-data

    StackOverflow
    https://tinyurl.com/2h4k94j8
    https://stackoverflow.com/questions/70394683/using-regex-to-split-a-sting-into-multiple-variables-sas

    Joe
    https://stackoverflow.com/users/1623007/joe

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    data have;
       length code $20 ;
       input id code $;
    cards4;
    101 K2K5K8F10F26F2
    102 L7P13P4
    103 L1
    ;;;;
    run;quit;

    Up to 40 obs WORK.HAVE total obs=3 05JAN2022:12:50:51

    Obs    code               id

     1     K2K5K8F10F26F2    101
     2     L7P13P4           102
     3     L1                103

    /*     _                  _ _   _
      __ _| | __ _  ___  _ __(_) |_| |__  _ __ ___
     / _` | |/ _` |/ _ \| `__| | __| `_ \| `_ ` _ \
    | (_| | | (_| | (_) | |  | | |_| | | | | | | | |
     \__,_|_|\__, |\___/|_|  |_|\__|_| |_|_| |_| |_|
             |___/
    */

     Think of the alpha charaters in a string as delimiters that you want to keep

      1. Count the number of slpha characters in the string
      2. Iterate over the number of alpha charcaters (assume only one alpha between digits))
           Scan to get position of the alpha character and the length to the next alpha character
           Note we are using the alpha characters as delimiters
      3. Extract the substring starting at pos-1 to length+1
      4. Outout
      5. Move to the next alpha delimiter

     Up to 40 obs WORK.WANT total obs=10 05JAN2022:13:06:15
                                                            WE WANT
                                                            THIS
     Obs    code               id    count    pos    len    substring   Think like

       1    K2K5K8F10F26F2    101      1        2      1    K2           K2,K5,K8,F10,F26,F2
       2    K2K5K8F10F26F2    101      2        4      1    K5
       3    K2K5K8F10F26F2    101      3        6      1    K8
       4    K2K5K8F10F26F2    101      4        8      2    F10
       5    K2K5K8F10F26F2    101      5       11      2    F26
       6    K2K5K8F10F26F2    101      6       14      7    F2
       7    L7P13P4           102      1        2      1    L7
       8    L7P13P4           102      2        4      2    P13
       9    L7P13P4           102      3        7     14    P4
      10    L1                103      1        2     19    L1

    K2K5K8F10F26F2

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    Up to 40 obs WORK.WANT total obs=10 05JAN2022:13:24:36

    Obs     id    code              substring    Sort of

      1    101    K2K5K8F10F26F2       K2
      2    101    K2K5K8F10F26F2       K5
      3    101    K2K5K8F10F26F2       K8
      4    101    K2K5K8F10F26F2       F10
      5    101    K2K5K8F10F26F2       F26
      6    101    K2K5K8F10F26F2       F2

      7    102    L7P13P4              L7
      8    102    L7P13P4              P13
      9    102    L7P13P4              P4

     10    103    L1                   L1

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    data want /*(keep=id code substring)*/;
      retain id code substring;
      set have;
      do count = 1 to countw(code,,'a');
        call scan(code,count,pos,len,,'a');
        substring = substr(code,pos-1,len+1);
        output;
      end;
    run;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
