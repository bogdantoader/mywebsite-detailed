Hugo website, with XMIN (https://xmin.yihui.name) theme.

While developing, run:

`hugo server -D`

which will start a server locally.

To generate the website, change the baseUrl property in config.toml so that
it uses the correct one (...maths.ox.ac.uk...), then run

`hugo --theme=strange-case-mod`

which will generate all the files in the 'public' directory. Then simply
copy those files to the server and access the website at

https://people.maths.ox.ac.uk/toader/


