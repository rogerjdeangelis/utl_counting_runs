# utl_counting_runs
Counting runs. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    SAS Forum: Counting runs

    Same result in WPS and SAS

    Ingenious solution by  KSharp

    github
    https://github.com/rogerjdeangelis/utl_counting_runs

    see
    https://goo.gl/2k6moz
    https://communities.sas.com/t5/SAS-Enterprise-Guide/Count-number-of-changes-in-variables-over-time/m-p/432666


    Ksharp profile
    https://communities.sas.com/t5/user/viewprofilepage/user-id/18408

    INPUT
    =====

     ALGORITHM

         Count runs of 1 including single first and last

         COUNTW which treats multiple '..' as one delimiter

         COUNTW function, "word" refers to a substring that has one of the following characteristics:
         is bounded on the left by a delimiter or the beginning of the string
         is bounded on the right by a delimiter or the end of the string


     WORK.HAVE total obs=11
                                                   |
         D     JUL17  AUG17  SEP17  OCT17  NOV17   |   RUN3
                                                   |
       1111      .      .      1      1      1     |    1   -> 111
       1112      1      .      1      1      .     |    2   -> 1 11
       1113      1      .      1      .      1     |    3   -> 1 1 1
       1114      .      1      1      1      1     |
       1115      1      .      1      1      1     |
       1116      1      .      1      .      .     |
       1117      1      .      .      .      1     |
       1118      1      .      .      1      .     |
       1119      .      1      .      1      1     |
       1120      1      .      1      1      .     |
       1121      .      1      1      .      .     |

    PROCESS  (all the code)
    =======================

      data want(keep=id runs);
       set have;
       runs=countw(cat(of jul17--nov17),'.');
      run;quit;

    OUTPUT
    ======

       ID     RUNS

      1111      1
      1112      2
      1113      3
      1114      1
      1115      2
      1116      2
      1117      2
      1118      2
      1119      2
      1120      2
      1121      1

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;


    data have;
     input id jul17 aug17 sep17 oct17 nov17;
    cards4;
    1111 . . 1 1 1
    1112 1 . 1 1 .
    1113 1 . 1 . 1
    1114 . 1 1 1 1
    1115 1 . 1 1 1
    1116 1 . 1 . .
    1117 1 . . . 1
    1118 1 . . 1 .
    1119 . 1 . 1 1
    1120 1 . 1 1 .
    1121 . 1 1 . .
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/

    ;

    * SAS;

    data want(keep=id runs);
     set have;
     runs=countw(cat(of jul17--nov17),'.');
    run;quit;


    * WPS;
    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data wrk.wantwps(keep=id runs);
     set wrk.have;
     runs=countw(cat(of jul17--nov17),".");
    run;quit;
    ');

