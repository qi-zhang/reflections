<h3>Weekly Reflections for the week 11/3-11/9</h3>

<h4>Requirements for Data Cleaning up</h4>

Sorry for a little bit late to submit the weekly reflection. I hope it is not too late.

The ultimate target of data cleaning up is to provide the analyzer an easy way to access those information in certain data structure they prefer. The procedure of generate such kind of data structure could be very detail-oriented and natually we want to provide a general way for anyone who want to use it. 

Sometimes, just taking care of missing data is not the end of the story yet. The original data might also contains lots of useless information, even noises which needs further clean up. It would be great to make it possible for the analyzers to selectively chosse data based on their own criterias. Since the criteria is decided by tha analyzer rather than the data curator, it is not possible to have it hard coded in the data cleaning up code. This is is one of the road block before we can move on furhter.

My solution is to allow the analyzer to pass a function which decides whether a data should be kept or not when they call the tools provide by data curator which generates the data structure.

Following pseudo code are based on R.

    # The data curator's code
    flexible_data_generator=function(original_data, select_function=NA) {
        export_data = original_data
        if(is.function(select_function)) {
              keep=select_function(year=original_data$year, mag=original_data$mag, ...)
              export_data = original_data[ keep, ]
        }
    
        # generate ppx data based on export_data
        ...
    }

    # The analyzer's code
    myselect=function(year, mag, ...) {...}
    flexible_data_generator(clean_data, myselect)

There is no need for the analyzer to implement their own select_function, but they can if they want to apply certain rules to the original data.
