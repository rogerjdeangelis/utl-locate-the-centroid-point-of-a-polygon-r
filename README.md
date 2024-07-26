# utl-locate-the-centroid-point-of-a-polygon-r
Locate the centroid point of a polygon r
    %utl pgm=utl locate the centroid point of a polygon r;

    Locate the centroid point of a polygon r

       Two Parts
          1 locate centroid
          2 suplemental ascii plots

    github
    https://tinyurl.com/2p8axjez
    https://github.com/rogerjdeangelis/utl-locate-the-centroid-point-of-a-polygon-r

    Rick
    https://blogs.sas.com/content/iml/2016/01/13/compute-centroid-polygon-sas.html

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* LOCATE INTERIOR CENTROID                                                                                               */
    /*                                                                                                                        */
    /*                      x                                                                                                 */
    /*      0       1       2       3       4                                                                                 */
    /*   y --+-------+-------+-------+-------+--- y                                                                           */
    /*     |                                    |                                                                             */
    /*     | Centroid Point (1.904762 2.047619) |                                                                             */
    /*     |                                    |                                                                             */
    /*   4 +               /\ 2.4               + 4                                                                           */
    /*     |              /  \                  |                                                                             */
    /*     |             /    \                 |                                                                             */
    /*   3 +            /      \                + 3                                                                           */
    /*     |           /        \               |                                                                             */
    /*     |          /          \              |                                                                             */
    /*   2 +     1,2 *     *1.9,2 ---> centroid + 2                                                                           */
    /*     |         |             \            |                                                                             */
    /*     |         |              \           |                                                                             */
    /*   1 +         *---------------* 3,1      + 1                                                                           */
    /*     |          1,1                       |                                                                             */
    /*     |                                    |                                                                             */
    /*   0 +                                  ) + 0                                                                           */
    /*     |                                    |                                                                             */
    /*     --+-------+-------+-------+-------+---                                                                             */
    /*       0       1       2       3       4                                                                                */
    /*                       x                                                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    Related repos
    ---------------------------------------------------------------------------------------------------
    https://github.com/rogerjdeangelis/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon
    https://github.com/rogerjdeangelis/utl-AI-internal-angles-of-polygon-from-vertex-coordinates-in-r
    https://github.com/rogerjdeangelis/utl-AI-labeling-centroids-inside-and-within-a-boundary-polygon
    https://github.com/rogerjdeangelis/utl-area-of-intersection-of-arbitrary-polygons-R-AI
    https://github.com/rogerjdeangelis/utl-calculate-the-area-within-a-latittude-longitude-polygon
    https://github.com/rogerjdeangelis/utl-draw-polygons-around-clusters-of-points-r-convex-hulls
    https://github.com/rogerjdeangelis/utl-how-many-triangles-in-the-polygon-r-igraph-AI
    https://github.com/rogerjdeangelis/utl-identify-points-inside-overlapping-polygons-R-spacial-package
    https://github.com/rogerjdeangelis/utl_AI_minimum_distance_from_a_poin_to_a_polygon
    https://github.com/rogerjdeangelis/utl_convex_hull_polygons_encompassing_a_three_dimensional_scatter_plot
    https://github.com/rogerjdeangelis/utl_simple_convex_hull_polygon_envelop_for_a_scatter_plot

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    * Input is a macro list

    %let polygon = %str(
          1, 1,
          1, 2,
          2, 4,
          3, 1,
          1, 1
          );

    %put &=polygon;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Macro variable POLYGON                                                                                                */
    /*                                                                                                                        */
    /*  POLYGON = 1, 1, 1, 2, 2, 4, 3, 1, 1, 1                                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*   _                 _                        _             _     _
    / | | | ___   ___ __ _| |_ ___    ___ ___ _ __ | |_ _ __ ___ (_) __| |
    | | | |/ _ \ / __/ _` | __/ _ \  / __/ _ \ `_ \| __| `__/ _ \| |/ _` |
    | | | | (_) | (_| (_| | ||  __/ | (_|  __/ | | | |_| | | (_) | | (_| |
    |_| |_|\___/ \___\__,_|\__\___|  \___\___|_| |_|\__|_|  \___/|_|\__,_|
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(ggplot2)
    library(ggridges)
    library(dplyr)
    df<-read_sas("d:/sd1/have.sas7bdat");
    # Load necessary libraries
    library(sf)
    library(ggplot2)

    # Create a sample polygon (example coordinates)
    polygon_coords <- matrix(c( &polygon
    ), ncol = 2, byrow = TRUE)

    # Convert coordinates to an sf object
    polygon <- st_polygon(list(polygon_coords))
    sf_polygon <- st_sfc(polygon)
    sf_polygon <- st_sf(geometry = sf_polygon)

    # Calculate the centroid
    centroid <- st_centroid(sf_polygon)
    centroid
    pdf("d:/pdf/centroid.pdf",height=4,width=4)
    # Plot the polygon and its centroid
    ggplot() +
      geom_sf(data = sf_polygon, fill = "lightblue", color = "black") +
      geom_sf(data = centroid, color = "red", size = 3)
    writeClipboard(as.character(centroid))
    ;;;;
    %utl_rendx(return=centroid);

    %put &=centroid;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  SAS Macro variable centroid                                                                                           */
    /*                                                                                                                        */
    /*  %put &=centroid;                                                                                                      */
    /*                                                                                                                        */
    /*  CENTROID=list(c(1.9047619047619, 2.04761904761905))                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*___                   _ _         _       _
    |___ \    __ _ ___  ___(_|_)  _ __ | | ___ | |_ ___
      __) |  / _` / __|/ __| | | | `_ \| |/ _ \| __/ __|
     / __/  | (_| \__ \ (__| | | | |_) | | (_) | |_\__ \
    |_____|  \__,_|___/\___|_|_| | .__/|_|\___/ \__|___/
                                 |_|
    */


    data have;
       array xys[5,2] _temporary_ (&polygon);
       do row=1 to 5;
         x=xys[row,1];
         y=xys[row,2];
         ano=cats(x,',',y);
         output;
       end;
       x=1.9;
       y=2;
       ano=cats('centroid 1.9',',',y);
       output;
       stop;
    run;quit;

    options ls=64 ps=24;
    proc plot data=have(rename=y=y1234567890123456789012345);
       plot y1234567890123456789012345*x='*' $ ano/box haxis= 0 to 4 by 1 vaxis= 0 to 4 by 1;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*      0       1       2       3       4                                                                                 */
    /*    --+-------+-------+-------+-------+--                                                                               */
    /*    |                                   |                                                                               */
    /*  4 +                 * 2,4             + 4                                                                             */
    /*    |                                   |                                                                               */
    /*    |                                   |                                                                               */
    /*  3 +                                   + 3                                                                             */
    /*    |                                   |                                                                               */
    /*    |                                   |                                                                               */
    /*  2 +         * 1,2  X centroid 1.9,2   + 2                                                                             */
    /*    |                                   |                                                                               */
    /*    |                                   |                                                                               */
    /*  1 +         * 1,1           * 3,1     + 1                                                                             */
    /*    |                                   |                                                                               */
    /*    |                                   |                                                                               */
    /*  0 +                                   + 0                                                                             */
    /*    |                                   |                                                                               */
    /*    --+-------+-------+-------+-------+--                                                                               */
    /*      0       1       2       3       4                                                                                 */
    /*                      x                                                                                                 */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
