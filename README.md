# Ian's Things of Variable Interest

A Jekyll-based blog. Some stuff stolen from <http://jekyllbootstrap.com>.

Content style inspired by the amazing [ReadNow](http://mischneider.net/readnowapp/) application.

Symlink `gh-pages` to `master` branch by adding these to the `[remote "origin"]` section of `.git/config` (courtesy [this](http://stackoverflow.com/a/7472481/102704)):

    push = +refs/heads/master:refs/heads/gh-pages
    push = +refs/heads/master:refs/heads/master

To run:

    $ sudo gem install jekyll rdiscount sass
    $ sass -w style.sass:style.css &
    $ jekyll --server --auto

Then go to http://localhost:4000/
