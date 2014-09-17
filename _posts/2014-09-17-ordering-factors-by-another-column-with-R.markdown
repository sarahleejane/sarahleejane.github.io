---
layout: post
title:  "ordering factors by another column with R"
date:   2014-09-17 12:23:28
categories: Learning R
---

### Goal
Plot a clear ggplot2 bar chart with x-axis labels appearing in the frequency with which they occur in the data frame.

### Data Sample
Suppose we have a sample of students who come from different countries.  This gives us a data frame of student origins, and the number of students who come from each origin.

{% highlight r %}
#sample of student origin in class
cc.df
{% endhighlight %}

![Sample of student origins](/assets/student_origin.png)

{% highlight r %}
#plot is hard to interpret when origin ordered alphabetically
library(ggplot2)
country.count.plot <- 
  ggplot(cc.df, aes(x=origin, y=count)) + 
  geom_bar(stat="identity", colour="black", fill="white") + 
  xlab("") + ylab("") 

country.count.plot 
{% endhighlight %}

![Plot of unordered origins](/assets/unordered_origins.png)


### Solution (Up)
Using the reorder function we are able to order *origin* factor by ascending *count*.

{% highlight r %}
#reorder origin by ascending count
cc.df$origin <- reorder(cc.df$origin, cc.df$count)

#plot is much more clear
country.count.plot <- 
  ggplot(cc.df, aes(x=origin, y=count)) + 
  geom_bar(stat="identity", colour="black", fill="white") + 
  xlab("") + ylab("") 

country.count.plot
{% endhighlight %}

![Plot of ascending origins](/assets/ascending_origins.png)


### Solution (Down)
We can also order *origin* by descending *count*.

{% highlight r %}
#order by descending count
cc.df$origin <- reorder(cc.df$origin, -cc.df$count)

#voila the plot I was looking for
country.count.plot <- 
  ggplot(cc.df, aes(x=origin, y=count)) + 
  geom_bar(stat="identity", colour="black", fill="white") + 
  xlab("") + ylab("") 

country.count.plot 
{% endhighlight %}

![Plot of descending origins](/assets/descending_origins.png)

### Feedback
Always feel free to get in touch with other solutions, general thoughts or questions.