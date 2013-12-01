<h3>Weekly Reflections for the week 11/14-11/30</h3>

<h4>Keep the Code Clean And Scalable</h4>

I've been working on etas-training.R this week, the Quaker team did a pretty job on this, it runs smoothly out-of-the-box and the final plot is colorful and with lots of information. There wasn't much I could do to improve the plot, so I focused on impoving the code itself by making it neater and faster.

The 1st thing I noticed was that the original code generated several large vectors but never used them. Also, there were some other large vectors served only as intermediate results for those unused vectors. so I made the code shorter, and faster by removing the code related to these variables, which didn't affect the final results.

The 2nd thing I noticed was that the original code sourced Luen's code that contains lots of information. However, the variables and functions defined in Luen's code were not used by etas-training, so I cleaned all variable in current space and reran the etas-training code without source Luen's code. It turned out that the final results remained unchanged.

The 3rd thing I noticed was that in the original code vectors are declared as place holder first and assigned later in a for loop. This approach works, but the better approach in R is to do it with sapply function. 

Finally, I went through the code step-by-step and tried to improve the performance of the code. I think reproducibility does not only mean to share the code and dataset with other people, but also means that the code is scalable in order to handle different dataset. I noticed that the it was possible to improve the code for calculating intermediate variable w58.list and w58.dist

    for(KK in 1:length(w58.list)){
      w58.list[KK]=min((times[1+KK]-times[1:(KK)])/(5.8^mags[1:(KK)]))}
    for(KK in 1:length(timelist)){
      w58.dist[KK]=min((timelist[KK]-times[1:n.events[KK]])/(5.8^mags[1:n.events[KK]]))}
    
The code needed to get power series 5.8^n inside the loop so it did the calculation everytime, and the calculation results were abandoned immediately after evaluation of that expression. On the other hand, all of these power values would be recalculated again for next KK value. Take w58.list as example, K ranges from 1 to N, so we need to get a power series of N items but in the code the power was evaluated about N*N/2 times. Considering the fact that power evaluation is a pretty expensive operation slowing the processing, I modified this part by adding power series generation code before for loop. Below is the new code.

    power_mags=5.8^mags
    w58.list=sapply(c(1:n.training), function(x) {
      min((times[1+x]-times[1:x])/(power_mags[1:x]))
    })
    w58.dist=sapply(c(1:length(timelist)), function(x) {
      min((timelist[x]-times[1:n.events[x]])/(power_mags[1:n.events[x]]))
    })

By combining all of these efforts together, the improved code I checked in runs almost 2 times faster than the original code.
