Best Practices Working With Spatial Data in R
========================================================
author: Alex M Chubaty
date: 02 May 2019
autosize: true
transition: rotate

Getting started
========================================================

Lots of great resources:

- <https://www.rspatial.org/>
- <https://www.r-spatial.org/> (note the dash)
- <https://datacarpentry.org/geospatial-workshop/>

A note about DataCamp:

- Recent mis-handling of sexual assault by DataCamp
- Many instructors advising people *not* use their courses, and are making them available elsewhere
- See <https://noamross.github.io/datacamp-sexual-assault/> for more info

Today's Workshop
========================================================

Notes available from <https://github.com/achubaty/r-talks/tree/master/spatial-data-best-practices>

0. CRAN taskviews:
  - <https://cran.r-project.org/web/views/Spatial.html>
  - <https://cran.r-project.org/web/views/SpatioTemporal.html>
1. Reading/loading spatial data
2. Basic plotting
3. **Gotchas and best practices**

Best Practices
========================================================

## References

- Bryan, J. 2017. Excuse me, do you have a moment to talk about version control? PeerJ Prepr. 5:1–23. https://peerj.com/preprints/3159v2/
- Visser M. D. _et al._. 2015. Speeding up ecological and evolutionary computations in R; essentials of high performance computing for biologists. PLOS Comput. Biol.  11:e1004140.
- Wilson, G. _et al._ 2014. Best practices for scientific computing. PLoS Biol. 12:e1001745.

Write scripts for _people_, not computers
========================================================

> it's great if others can use your code, but it's more important that you can reuse it

- comment your code with _why_ (design & purpose), not the _what_ (mechanics)
- use descriptive names (_nouns_ for objects; _verbs_ for functions)
- use consistent style/formatting
  - check formatting with `lintr`

Make incremental changes
=========================================================

- use version control (e.g., git & GitHub)

Don't repeat yourself
=========================================================

- don't copy/paste
- don't copy paste
- don't copy/paste
- modularize code for reuse (use functions)

Expect mistakes
=========================================================

- use assertions & tests

Collaborate
=========================================================

- pair programming
- code review

Optimize your code
=========================================================

- speed and memory improvements
- **only do this after code is working correctly**

Don't be greedy
========================================================

Especially on a shared system 😄

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

Caching and memoisation
========================================================

- `reproducible::prepInputs` and `reproducible::Cache`
