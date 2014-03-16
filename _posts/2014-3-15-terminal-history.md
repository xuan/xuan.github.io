---
layout: post
title: Adding search history in terminal with your up arrow
---

Create a file in your home directory called <strong>.inputrc</strong> and add the following contents:

{% highlight bash %}
cd ~
nano .inputrc
{% endhighlight %}

Paste in the following contents:

{% highlight bash %}
## arrow up
"\e[A":history-search-backward
## arrow down
"\e[B":history-search-forward
{% endhighlight %}

Save the file and restart your terminal.  You can now search for your previous commands by typing the first 2 letters followed by an up arrow key.  You can then scroll through your history by tapping the up arrow key.