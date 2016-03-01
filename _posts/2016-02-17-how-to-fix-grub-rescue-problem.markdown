---
layout: post
title:  "how to fix grub rescue problem ?"
date:   2016-02-17 08:06:16 +0000
description: Jekyll is a static site generator, an open-source tool for creating simple yet powerful websites of all shapes and sizes.
categories: linux
---
1. ls
2. ls "your disk"
3. if not found, check another disk
4. grub rescue> set boot=(hd0,msdos5)
5. grub rescue> set prefix=(hd0,msdos5)/boot/grub
6. grub rescue> insmod normal
7. grub rescue> normal
8. boot to your linux
9. open terminal and type sudo grub-install /dev/sda

<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
