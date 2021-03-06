# utl_importing_json_file_data_into_sas_as_a_dataset
Importing any JSON File Data into SAS as a dataset
    Importing any JSON File into SAS as a dataset

    github
    https://github.com/rogerjdeangelis/utl_importing_json_file_data_into_sas_as_a_dataset

    INPUT
    =====

       d:/json/have.json

          {
           "2017-11-17-blog-post-01": {
              "title": "Blog Post 01",
              "layout": "post",
              "categories": [
                "Category1",
                "Category2"
              ],
              "comments": true,
              "published": true,
              "permalink": "/blog-post-01.html",
              "basename": "2017-11-17-blog-post-01"
            },
            "2017-11-30-blog-post-02": {
              "title": "Blog Post 2",
              "layout": "post",
              "categories": [
                "Category2",
                "Category3"
              ],
              "comments": true,
              "published": true,
              "permalink": "/2017-11-30-blog-post-02.html",
              "basename": "2017-11-30-blog-post-02"
            }
          }

    WORKING CODE  (generic code for many JSON files)
    ================================================

        jsn <- as.data.frame(fromJSON(paste(readLines("d:/json/have.json"), collapse="")));
        jsnxpo<-cbind(names(jsn),t(jsn));

    OUTPUT
    ======

        WORK.SIMPLER total obs=14

        From here it is easy to reshape the data


                        V1                     V2                               V3

        X2017.11.17.blog.post.01.title         Blog Post 01                     Blog Post 01
        X2017.11.17.blog.post.01.layout        post                             post
        X2017.11.17.blog.post.01.categories    Category1                        Category2
        X2017.11.17.blog.post.01.comments      TRUE                             TRUE
        X2017.11.17.blog.post.01.published     TRUE                             TRUE
        X2017.11.17.blog.post.01.permalink     /blog-post-01.html               /blog-post-01.html
        X2017.11.17.blog.post.01.basename      2017-11-17-blog-post-01          2017-11-17-blog-post-01

        X2017.11.30.blog.post.02.title         Blog Post 2                      Blog Post 2
        X2017.11.30.blog.post.02.layout        post                             post
        X2017.11.30.blog.post.02.categories    Category2                        Category3
        X2017.11.30.blog.post.02.comments      TRUE                             TRUE
        X2017.11.30.blog.post.02.published     TRUE                             TRUE
        X2017.11.30.blog.post.02.permalink     /2017-11-30-blog-post-02.html    /2017-11-30-blog-post-02.html
        X2017.11.30.blog.post.02.basename      2017-11-30-blog-post-02          2017-11-30-blog-post-02


    see
    https://goo.gl/ttEPQD
    https://stackoverflow.com/questions/47605173/importing-json-file-data-into-r-as-data-frame-for-nlp

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data _null_;
     file "";
     input;
     put _infile_;
     putlog _infile_;
    cards4;
    {
     "2017-11-17-blog-post-01": {
        "title": "Blog Post 01",
        "layout": "post",
        "categories": [
          "Category1",
          "Category2"
        ],
        "comments": true,
        "published": true,
        "permalink": "/blog-post-01.html",
        "basename": "2017-11-17-blog-post-01"
      },
      "2017-11-30-blog-post-02": {
        "title": "Blog Post 2",
        "layout": "post",
        "categories": [
          "Category2",
          "Category3"
        ],
        "comments": true,
        "published": true,
        "permalink": "/2017-11-30-blog-post-02.html",
        "basename": "2017-11-30-blog-post-02"
      }
    }
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64(resolve('
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk "%sysfunc(pathname(work))";
    proc r;
    submit;
    library("rjson");
    jsn <- as.data.frame(fromJSON(paste(readLines("d:/json/have.json"), collapse="")));
    jsnxpo<-cbind(names(jsn),t(jsn));
    endsubmit;
    import r=jsnxpo data=wrk.simpler;
    run;quit;
    '));



    From here it is easy to reshape the data

    WORK.SIMPLER total obs=14


    Obs                    V1                     V2                               V3

      1    X2017.11.17.blog.post.01.title         Blog Post 01                     Blog Post 01
      2    X2017.11.17.blog.post.01.layout        post                             post
      3    X2017.11.17.blog.post.01.categories    Category1                        Category2
      4    X2017.11.17.blog.post.01.comments      TRUE                             TRUE
      5    X2017.11.17.blog.post.01.published     TRUE                             TRUE
      6    X2017.11.17.blog.post.01.permalink     /blog-post-01.html               /blog-post-01.html
      7    X2017.11.17.blog.post.01.basename      2017-11-17-blog-post-01          2017-11-17-blog-post-01

      8    X2017.11.30.blog.post.02.title         Blog Post 2                      Blog Post 2
      9    X2017.11.30.blog.post.02.layout        post                             post
     10    X2017.11.30.blog.post.02.categories    Category2                        Category3
     11    X2017.11.30.blog.post.02.comments      TRUE                             TRUE
     12    X2017.11.30.blog.post.02.published     TRUE                             TRUE
     13    X2017.11.30.blog.post.02.permalink     /2017-11-30-blog-post-02.html    /2017-11-30-blog-post-02.html
     14    X2017.11.30.blog.post.02.basename      2017-11-30-blog-post-02          2017-11-30-blog-post-02


