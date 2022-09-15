**University of Pennsylvania, CIS 565: GPU Programming and Architecture,
Project 1 - Flocking**

* Penghai Wei
  * [LinkedIn](https://www.linkedin.com/in/penghai-wei)
* Tested on: Windows 11 Build 22000, i9-12900k @ 5.20GHz, RTX 3080ti 12GB


Coherent uniform grid 10000 boids with 0.2 delta time

![Coherent uniform grid 10000 boids](images/10000.gif)

Coherent uniform grid 50000 boids with 0.2 delta time

![Coherent uniform grid 50000 boids](images/50000.gif)

Coherent uniform grid 100000 boids with 0.2 delta time

![Coherent uniform grid 100000 boids](images/100000.gif)

Coherent uniform grid 50000 boids with 0.05 delta time

![Coherent uniform grid 50000 boids](images/50000-0.05.gif)

Coherent uniform grid 100000 boids with 0.05 delta time

![Coherent uniform grid 100000 boids](images/100000-0.05.gif)

Frame Rate Histogram (1-to-1)

![Histogram](images/Histogram.png)

Frame Rate Histogram (Logarithmic)

![LogHistogram](images/LogHistogram.png)

Questions to answer:

1. For each implementation, how does changing the number of boids affect performance? Why do you think this is?

Brute force search's performance degrades quadratically. This is because it has a O(n^2) complexity for iterating all other boids for each boid. The other two degrades in a close-to-linear rate. They only search boids in neighboring grids, and that's probably a fixed constant ratio to the total number of boids influenced by block size. The uniform grid without coherence optimization suffers from cache miss so it's little worse.

2. For the coherent uniform grid: did you experience any performance improvements with the more coherent uniform grid? Was this the outcome you expected? Why or why not?

Yes. Since it reorganize the memory is a way that the consecutive accesses are continuous in memory region, cache miss is much less frequent for each cuda warp. And boids in the same warp are also probably now in neighboring or same block in space.

<!-- 3. Did changing cell width and checking 27 vs 8 neighboring cells affect performance? Why or why not? Be careful: it is insufficient (and possibly incorrect) to say that 27-cell is slower simply because there are more cells to check!

This depends on multiple things. In each warp, kernels might be accessing different 8 cells within the 27 cells, and memory has to be loaded for all 27 cells in this situation.  -->