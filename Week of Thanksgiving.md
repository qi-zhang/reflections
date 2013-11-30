<h3>Weekly Reflections for the week 11/14-11/30</h3>

<h4>Keep the Code Clean And Scalable</h4>

I was working on etas-training.R this week, the Quaker team did a pretty job on this, it runs smoothly out-of-the-box and the final plot is colorful and with lots of information. There isn't much I can do to improve the plot, so I focused on impoving the code itself by make it cleaner and faster.

The 1st thing I noticed is that the original code generates several large vectors but never use them. Also, there are some other large vectors served only as intermediate results for those unused vectors. I can definitely make the code shorter, cleaner and faster by removing any code related to these variables and there is no impact to the final results.

The 2nd thing I noticed is that the original code source Luen's code which contains lots of information. However nothing defined in Luen's code has been refered in the following part which implies it is possible that the code works fine without source Luen's code. So I cleaned all variable in current space and reran the etas-training code without source Luen's code. It turned out that there is no change to the final result.

The 3rd thing I noticed is that in the original code vectors are declared as place holder first and assigned later in a for loop. This approach works, but the better approach in R is to do it with sapply function. 

Finally, I went through the code step-by-step. I noticed that the code for calculating intermediate varaible w58.list and w58.dist

  for(KK in 1:length(w58.list)){
    w58.list[KK]=min((times[1+KK]-times[1:(KK)])/(5.8^mags[1:(KK)]))}
  for(KK in 1:length(timelist)){
    w58.dist[KK]=min((timelist[KK]-times[1:n.events[KK]])/(5.8^mags[1:n.events[KK]]))}
    
The code needs to get power series 5.8^n inside the loop so it dose the calculation everytime, and the calculation results are abandoned immediately after evaluation of that expression. On the other hand, all of these power values will be recalculated again for next KK value. Apparently it can be improved. Take w58.list as example, K ranges from 1 to N, which means it is necessary to get a power series of N items and the code in the for-loop will evaluate power about N*N/2 times. Take the fact that power evaluation is a pretty expensive operation into consideration, there is notable room for performance improvement here.

Combine all of these effort together, I checked-in the impvoed version which runs pretty much 2 times faster than the original code.
