# utl-round-trip-sas-time-values-to-excel-and-back-sas-and-r-openxlsx
Round trip sas time values to excel and back sas and r openxlsx
    %let pgm=utl-round-trip-sas-time-values-to-excel-and-back-sas-and-r-openxlsx;

    %stop_submission;

    Round trip sas time values to excel and back sas and r openxlsx

    github
    https://tinyurl.com/yw7b8pre
    https://github.com/rogerjdeangelis/utl-round-trip-sas-time-values-to-excel-and-back-sas-and-r-openxlsx

    sas communities
    https://tinyurl.com/3ft266uj
    https://communities.sas.com/t5/SAS-Programming/Write-SAS-Time-fields-to-Excel-loses-time-format/td-p/751568

    related repos
    
https://github.com/rogerjdeangelis/utl-Import-the-datepart-of-an-excel-datetime-formatted-columns
    
https://github.com/rogerjdeangelis/utl-importing-excel-datetime-values-in-xlsx-and-xlsx-workbooks
    
https://github.com/rogerjdeangelis/utl-safe-way-import-excel-time-value
    
https://github.com/rogerjdeangelis/utl-safely-sending-dates-or-datetimes-back-and-forth-to-excel



    /**************************************************************************************************************************/
    /* d:/xls/sheet1.xlsx               | 1 SAS TIME VALUES TO EXCEL TIME           | SD1.WANT                                */
    /*                                  | ===============================           |                                         */
    /* -------------------------+       |                                           | HOUR    TIME                            */
    /* | A1| fx  | HOUR         |       | STEPS                                     |                                         */
    /* --------------------------       |                                           |   1     1:00                            */
    /* [_] |  A  |      B       |       |   1 INPUT sas time to excel time          |   2     2:00                            */
    /* --------------------------       |     excel_time=hour/24;                   |   3     3:00                            */
    /*  1  |HOUR | EXCEL_TIME   |       |     hour as a fraction of day             |   4     4:00                            */
    /*  -- |-----+--------------+       |   2 Create r dataframe from excel         |   5     5:00                            */
    /*  2  |  1  | 0.0416666667 |       |   3 Convert excel to sas time             |                                         */
    /*  -- |-----+--------------+       |     want$TIME=want$TIME*86400;            |                                         */
    /*  3  |  2  | 0.0833333333 |       |     86400 second in a day                 |                                         */
    /*  -- |-----+--------------+       |   4 print with format hhmm.               |                                         */
    /*  4  |  3  | 0.125        |       |                                           |                                         */
    /*  -- |-----+--------------+       |   %utl_rbeginx;                           |                                         */
    /*  5  |  4  | 0.1666666667 |       |   parmcards4;                             |                                         */
    /*  -- |-----+---------+----+       |   library(haven)                          |                                         */
    /*  6  |  5  | 0.2083333333 |       |   library(openxlsx)                       |                                         */
    /*  -- |-----+--------------+       |   source("c:/oto/fn_tosas9x.R")           |                                         */
    /* [SHEET1]                         |   want <- read.xlsx(                      |                                         */
    /*                                  |     "d:/xls/sheet1.xlsx"                  |                                         */
    /* /*--- make excel time ---*/      |     ,sheet = 1)                           |                                         */
    /* data have;                       |   want$TIME=want$TIME*86400;              |                                         */
    /*   do hour=1 to 5 ;               |   want                                    |                                         */
    /*     time=hour/24 ;               |   fn_tosas9x(                             |                                         */
    /*     output;                      |         inp    = want                     |                                         */
    /*   end;                           |        ,outlib ="d:/sd1/"                 |                                         */
    /* run;                             |        ,outdsn ="want"                    |                                         */
    /*                                  |   )                                       |                                         */
    /* %utlfkil(d:/xls/sheet1.xlsx);    |   ;;;;                                    |                                         */
    /*                                  |   %utl_rendx;                             |                                         */
    /* ods excel                        |                                           |                                         */
    /*  file="d:/xls/sheet1.xlsx"       |   libname sd1 "d:/sd1";                   |                                         */
    /*  options (sheet_name='Sheet1');  |   proc print data=sd1.want;               |                                         */
    /* proc print data=have  noobs;     |     format time hhmm.;                    |                                         */
    /*  var hour time;                  |   run;quit;                               |                                         */
    /* run;                             |                                           |                                         */
    /* ods excel close ;                |                                           |                                         */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    /*--- make excel time ---*/
    data have;
      do hour=1 to 5 ;
        time=hour/24 ;
        output;
      end;
    run;

    %utlfkil(d:/xls/sheet1.xlsx);

    ods excel
     file="d:/xls/sheet1.xlsx"
     options (sheet_name='Sheet1');
    proc print data=have  noobs;
     var hour time;
    run;
    ods excel close ;

    /**************************************************************************************************************************/
    /* WORK.HAVE         | d:/xls/sheet1.xlsx                                                                                 */
    /*                   |                                                                                                    */
    /* HOUR      TIME    | -------------------------+                                                                         */
    /*                   | | A1| fx  | HOUR         |                                                                         */
    /*   1     0.04167   | --------------------------                                                                         */
    /*   2     0.08333   | [_] |  A  |      B       |                                                                         */
    /*   3     0.12500   | --------------------------                                                                         */
    /*   4     0.16667   |  1  |HOUR | EXCEL_TIME   |                                                                         */
    /*   5     0.20833   |  -- |-----+--------------+                                                                         */
    /*                   |  2  |  1  | 0.0416666667 |                                                                         */
    /*                   |  -- |-----+--------------+                                                                         */
    /*                   |  3  |  2  | 0.0833333333 |                                                                         */
    /*                   |  -- |-----+--------------+                                                                         */
    /*                   |  4  |  3  | 0.125        |                                                                         */
    /*                   |  -- |-----+--------------+                                                                         */
    /*                   |  5  |  4  | 0.1666666667 |                                                                         */
    /*                   |  -- |-----+---------+----+                                                                         */
    /*                   |  6  |  5  | 0.2083333333 |                                                                         */
    /*                   |  -- |-----+--------------+                                                                         */
    /*                   | [SHEET1]                                                                                           */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */


    STEPS

      1 INPUT sas time to excel time
        excel_time=hour/24;
        hour as a fraction of day
      2 Create r dataframe from excel
      3 Convert excel to sas time
        want$TIME=want$TIME*60*20;
        want$time is fraction od a day
        86400 second in a day
      4 print with format hhmm.


    %utlfkil(d:/xls/sheet1.xlsx);

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(openxlsx)
    source("c:/oto/fn_tosas9x.R")
    want <- read.xlsx(
      "d:/xls/sheet1.xlsx"
      ,sheet = 1)
    want$TIME=want$TIME*86400;
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
    )
    ;;;;
    %utl_rendx;

    libname sd1 "d:/sd1";
    proc print data=sd1.want;
      format time hhmm.;
    run;quit;

    /**************************************************************************************************************************/
    /* STEPS                                 | HOUR    TIME                                                                   */
    /*                                       |                                                                                */
    /*   1 INPUT sas time to excel time      |   1     1:00                                                                   */
    /*     excel_time=hour/24;               |   2     2:00                                                                   */
    /*     hour as a fraction of day         |   3     3:00                                                                   */
    /*   2 Create r dataframe from excel     |   4     4:00                                                                   */
    /*   3 Convert excel to sas time         |   5     5:00                                                                   */
    /*     want$TIME=want$TIME*60*20;        |                                                                                */
    /*     want$time is fraction od a day    |                                                                                */
    /*     86400 second in a day             |                                                                                */
    /*   4 print with format hhmm.           |                                                                                */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

