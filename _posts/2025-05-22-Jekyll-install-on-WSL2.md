---
layout: post
title:  "Jekyll install on WSL2 on Windows 11"
date: 2025-05-22 21:48:38 +0300
categories: jekyll install
---

OK, I finally bit the bullet and decided to go with a static site generator. 

I looked around on how to best go with Github Pages - Jekyll setup and saw that there were many references to setting Jekyll up on Linux, but I was thinking why wouldn't I go with WSL2 on my Windows box?

I managed to do that and got Jekyll working. Here are the steps that I took:

<!--more-->

1. Clone your website repository

{% highlight shell %}
mkdir website
cd website
git clone <your-github-page-username.github.io>
{% endhighlight %}

2. Update your .bashrc with the gem install locations.
*You should never run gems as root.* Following is from the [official Jekyll documentation](https://jekyllrb.com/docs/installation/ubuntu/)

{% highlight shell %}
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
{% endhighlight %}

3. Install prerequisites and Ruby (See note below)

{% highlight shell %}
sudo apt update && sudo apt upgrade -y
sudo apt install make gcc gpp build-essential zlib1g zlib1g-dev ruby-full 
{% endhighlight %}

4. Install bundler and Jekyll

{% highlight shell %}
sudo gem install bundler
sudo gem install jekyll 
jekyll -v # to check if Jekyll installed successfully.
{% endhighlight %}

If Jekyll is installed successfully, you should get the version number, e.g. `jekyll 4.4.1`.

If you run `gem install jekyll` (or `gem install jekyll bundler`) and  without sudo, you will get the following error:

{% highlight shell %}
ERROR:  While executing gem ... (Gem::FilePermissionError)
 You don't have write permissions for the /var/lib/gems/3.0.0 directory.
{% endhighlight %}

5. Switch to your local repository (per Step 1) - note the dot (.) at the end of the bundle exec command

{% highlight shell %}
cd website/your-github-page-username.github.io
bundle exec jekyll new --force --skip-bundle .
bundle init
bundle add jekyll
bundle install
bundle info jekyll # to check if Jekyll installed successfully in our local repository
{% endhighlight %}

Again, if Jekyll website is built successfully, the output would be:

{% highlight shell %}
 * jekyll (4.4.1)
 Summary: A simple, blog aware, static site generator.
 Homepage: https://jekyllrb.com
 Source Code: https://github.com/jekyll/jekyll
 Changelog: https://github.com/jekyll/jekyll/releases
 Bug Tracker: https://github.com/jekyll/jekyll/issues
 Path: /var/lib/gems/3.0.0/gems/jekyll-4.4.1
 Reverse Dependencies:
jekyll-feed (0.17.0) depends on jekyll (>= 3.7, < 5.0)
jekyll-seo-tag (2.8.0) depends on jekyll (>= 3.8, < 5.0)
minima (2.5.2) depends on jekyll (>= 3.5, < 5.0)
{% endhighlight %}

6. Add theme per its documentation

See supported themes: https://pages.github.com/themes/
_Play it safe now, and go with one of the themes in the link_.

7. Copy theme files under your local repository directory. Per Step 2, they are installed under ~/gems directory.

{% highlight shell %}
cd website/your-github-page-username.github.io
bundle show <your theme name>
> /home/your-user-name/gems/gems/theme-name
cp -r /home/your-user-name/gems/gems/theme-name/* .
{% endhighlight %}

8. I'd suggest to run `jekyll new --force .` at this point so that Jekyll installer generates tha necessary configuration files.

{% highlight shell %}
jekyll new --force .
{% endhighlight %}

In this step, if you get an error like the following, delete one of the files. For example, delete about.markdown and index.markdown considering the following error:

{% highlight shell %}
Jekyll Feed: Generating feed for posts
 Conflict: The following destination is shared by multiple files.
  The written file may end up with unexpected contents.
/mnt/c/Users/tolga/repos/websites/tbalci-github-io/_site/about/index.html
 - about.markdown
 - about.md

  Conflict: The following destination is shared by multiple files.
The written file may end up with unexpected contents.
/mnt/c/Users/tolga/repos/websites/tbalci-github-io/_site/index.html
 - index.markdown
 - index.md
{% endhighlight %}


 Credits:
 1. [Stephen Whitaker](https://stackoverflow.com/users/13877791/steven-whitaker) on his post on [StackOverflow](https://stackoverflow.com/questions/61704004/trouble-creating-a-new-jekyll-site-could-not-locate-gemfile-or-bundle-direct)
 2. [Sean Killeen](https://seankilleen.com/2020/07/building-my-jekyll-blog-with-ubuntu-on-wsl2/)
