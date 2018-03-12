# utl_using_sql_arrays_to_avoid_a_long_select_clause
Using sql arrays to avoid a long select clause. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Using sql arrays to avoid a long select clause

    Using explicit SQL passthru to compute many baseball statistics by team for an excel user(fat data).
    Do not disparage EXCEL users they outnumber us 100 to 1;

    Original topic: Shortening my proc SQL with lots of variables

      Same result in WPS and SAS, but I am not sure the generated code was done before passing to WPS

    github
    https://github.com/rogerjdeangelis/utl_using_sql_arrays_to_avoid_a_long_select_clause

    see
    https://tinyurl.com/ycb95sz4
    https://communities.sas.com/t5/Base-SAS-Programming/Shortening-my-proc-SQL-with-lots-of-variables/m-p/444750

    This requires Soer Lassens varlist macro and do_over macro.

    DO_OVER macro
    https://tinyurl.com/y86yugga
    https://github.com/rogerjdeangelis/utl_sql_looping_or_using_arrays_in_sql_do_over_macro/blob/master/utl_sql_looping_or_using_arrays_in_sql_do_over.sas

    VARLIST Macro
    Author: Søren Lassen, s.lassen@post.tele.dk
    https://tinyurl.com/ycb95sz4
    https://github.com/rogerjdeangelis/utl_using_sql_arrays_to_avoid_a_long_select_clause/blob/master/utl_using_sql_arrays_to_avoid_a_long_select_clause_varlist



    INPUT (This is very fast in teradata with 1000 cores)
    ======================================================

     RULE

     Use explicit teradata passthru SQL to compute the min, mean, median and max of 18 variables for excel user.
     (SQL not summary, means, tabulate, report)

     SASHELP.BASEBALL


      TEAM         Char     14    Team at the End of 1986

      NATBAT       Num       8    Times at Bat in 1986
      NHITS        Num       8    Hits in 1986
      NHOME        Num       8    Home Runs in 1986
      NRUNS        Num       8    Runs in 1986
      NRBI         Num       8    RBIs in 1986
      NBB          Num       8    Walks in 1986
      YRMAJOR      Num       8    Years in the Major Leagues
      CRATBAT      Num       8    Career Times at Bat
      CRHITS       Num       8    Career Hits
      CRHOME       Num       8    Career Home Runs
      CRRUNS       Num       8    Career Runs
      CRRBI        Num       8    Career RBIs
      CRBB         Num       8    Career Walks
      NOUTS        Num       8    Put Outs in 1986
      NASSTS       Num       8    Assists in 1986
      NERROR       Num       8    Errors in 1986
      SALARY       Num       8    1987 Salary in $ Thousands
      LOGSALARY    Num       8    Log Salary


     WANT min,media,mean,max for every numeric variable
     Need to generate this code (3 of 18 sections shown) for excel user.

       select
          team

         ,min(NATBAT)       as min_NATBAT
         ,median(NATBAT)    as median_NATBAT
         ,mean(NATBAT)      as mean_NATBAT
         ,max(NATBAT)       as max_NATBAT

         ,min(NHITS)        as min_NHITS
         ,median(NHITS)     as median_NHITS
         ,mean(NHITS)       as mean_NHITS
         ,max(NHITS)        as max_NHITS

          ......
          ......

         ,min(LOGSALARY)    as min_LOGSALARY
         ,median(LOGSALARY) as median_LOGSALARY
         ,mean(LOGSALARY)   as mean_LOGSALARY
         ,max(LOGSALARY)    as max_LOGSALARY

      group
         by team



    PROCESS
    =======

      * create a list of numeric variables and store in nums array;
      %array(nums,values=%varlist(sashelp.baseball,keep=_numeric_));

      * passthru to teradata ( do not have a teradata connection right now);
      proc sql;
        create
          table want as
        select
          team
            %do_over(nums,phrase=%str(
            ,min(?)     as min_?
            ,median(?)  as median_?
            ,mean(?)    as mean_?
            ,max(?)     as max_?)
           )
        from
            sashelp.baseball
        group
            by team
      ;quit;

    OUTPUT
    ======

      Middle Observation(12 ) of want - Total Obs 24


       -- CHARACTER --            VALUE

      TEAM               C14      Milwaukee      Team at the End of 1986


       -- NUMERIC --

      MIN_NATBAT         N8       181
      MEDIAN_NATBAT      N8       331.5
      MEAN_NATBAT        N8       351.4
      MAX_NATBAT         N8       542

      MIN_NHITS          N8       41
      MEDIAN_NHITS       N8       88
      MEAN_NHITS         N8       91.78
      MAX_NHITS          N8       163

      MIN_NHOME          N8       1
      MEDIAN_NHOME       N8       7
      MEAN_NHOME         N8       8.428
      MAX_NHOME          N8       33

      .....
      .....

      MIN_LOGSALARY      N8       4.212
      MEDIAN_LOGSALARY   N8       5.446
      MEAN_LOGSALARY     N8       5.557
      MAX_LOGSALARY      N8       7.138

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

     just use sashelp.baseball

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    * SAS;
    * create a list of numeric variables and store in nums array;
    %array(nums,values=%varlist(sashelp.baseball,keep=_numeric_));

    * passthru to teradata ( do not have a teradata connection right now);
    proc sql;
      create
        table want as
      select
        team
          %do_over(nums,phrase=%str(
          ,min(?)    as min_?
          ,median(?) as median_?
          ,mean(?)   as mean_?
          ,max(?)    as max_?)
         )
      from
          sashelp.baseball
      group
          by team
    ;quit;


    * WPS;
    %utl_submit_wps64('
      libname wrk sas7bdat "%sysfunc(pathname(work))";
      libname hlp "C:/Program Files/sashome/SASFoundation/9.4/core/sashelp";
      %array(nums,values=%varlist(hlp.baseball,keep=_numeric_));
      proc sql;
        create
          table wrk.want as
        select
          team
            %do_over(nums,phrase=%str(
            ,min(?)    as min_?
            ,median(?) as median_?
            ,mean(?)   as mean_?
            ,max(?)    as max_?)
           )
        from
            hlp.baseball
        group
            by team
      ;quit;
    run;quit;
    ');

