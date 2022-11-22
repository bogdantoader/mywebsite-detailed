Hugo website, with XMIN (https://xmin.yihui.name) theme.

While developing, run:

`hugo server -D`

which will start a server locally.

To generate the website, change the baseUrl property in config.toml so that
it uses the correct one (bogdantoader.github.io) then run

`hugo --theme=hugo-xmin`

which will generate all the files in the 'public' directory. Then simply
copy those files to the github.io repo, push and 
access the website at

https://bogdantoader.github.io

The stuff to edit is in the 'content' folder.


