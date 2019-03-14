spatial-data-best-practices
========================================================
author: Alex M Chubaty
date: 02 May 2019
autosize: true

First Slide
========================================================

For more details on authoring R presentations please visit <https://support.rstudio.com/hc/en-us/articles/200486468>.

- Bullet 1
- Bullet 2
- Bullet 3

Slide With Code
========================================================


```r
summary(cars)
```

```
     speed           dist       
 Min.   : 4.0   Min.   :  2.00  
 1st Qu.:12.0   1st Qu.: 26.00  
 Median :15.0   Median : 36.00  
 Mean   :15.4   Mean   : 42.98  
 3rd Qu.:19.0   3rd Qu.: 56.00  
 Max.   :25.0   Max.   :120.00  
```

Slide With Plot
========================================================

![plot of chunk unnamed-chunk-2](spatial-data-best-practices-figure/unnamed-chunk-2-1.png)

Best Practices
========================================================

Don't be greedy on a shared system :)

1. only use what you need;
2. improve your code so it's less greedy;
3. communicate your needs with other users.

What Does It Mean For Code To Be Greedy?
========================================================

1. RAM usage;
2. Disk I/O;
3. Disk storage (user and temp folders);
4. CPU time;
5. Network use.

The Basics
========================================================

> you wouldn't turn the oven on and walk away without setting a timer...why risk burning the computer down with your R scripts?

1. remove intermediate objects (save them to disk for retrieval later if you need them); - RAM
2. have the script quit the rsession when done; - RAM

    ```r
    exit <- Q <- function(save = "no", status = 0, runLast = TRUE) {
      q(save = save, status = 0, runLast = TRUE)
    }
    ```

3. stagger you jobs using `Sys.sleep()`. - RAM; CPU; disk; Network

Code Profiling
========================================================

Identify bottlenecks + high resource use

1. `Sys.time()`;
2. `object.size()`
3. Rstudio profiler

Memory Management in R
========================================================

Garbage collection is handled by R and your OS will try to free RAM that is no longer being used.

However, you can make it more efficient by frequently removing transient/intermediate objects from memory:

- save to disk often, and load from disk when recovering your session (so you don't need to rerun everything each time);
- cache your intermediate results

Temp Storage
========================================================

Many `raster` operations use tempfiles behind the scenes, which will clutter your temp drive (a big problem when the machine isn't rebooted frequently).

Use `rasterOptions()` to set `tmpdir` manually and be sure to cleanup (`unlink(raster::tmpDir(), recursive = TRUE)`) at the end of your script/session.

Turn Off Your Clusters
========================================================

Cluster operations on Windows use more RAM than they should...

Always close your clusters when done to ensure that RAM gets freed

Other stuff
========================================================

- `reproducible::prepInputs` and `reproducible::Cache`
- code examples we can work through and try
- before workshop, send out quick survey to find out which packages / operations people are using
- version control (GitHub)