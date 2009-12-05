This is a Perl one-liner that downloads YouTube videos. I wrote it for fun.

It was written by Peteris Krumins (peter@catonmat.net).
His blog is at http://www.catonmat.net  --  good coders code, great reuse.

The code is licensed under the MIT license.

I explained every bit of it in my article "Downloading YouTube Videos with a
Perl One-Liner". Read it here:

http://www.catonmat.net/blog/downloading-youtube-videos-with-a-perl-one-liner/

------------------------------------------------------------------------------

Here it is:

perl -MWWW::Mechanize -e '$m = WWW::Mechanize->new; $_=shift; ($i) = /v=(.+)/; s/%(..)/chr(hex($1))/ge for (($u) = $m->get($_)->content =~ /l_map": .+(?:%2C)?5%7C(.+?)"/); $m->get($u, ":content_file" => "$i.flv")'

Use it as following:

    $ perl ... <http://www.youtube.com/watch?v=ID>

It will save the video to a file named "ID.flv".

Here is how it works, statement by statement:

------------------------------------.-----------------------------------------
code                                | explanation
------------------------------------+-----------------------------------------
$m = WWW::Mechanize->new;           | creates WWW::Mechanize object
------------------------------------+-----------------------------------------
$_=shift;                           | puts the command line argument in $_
------------------------------------+-----------------------------------------
($i) = /v=(.+)/;                    | extracts the video ID for later user for
                                    | filename.
------------------------------------+-----------------------------------------
s/%(..)/chr(hex($1))/ge for (($u) = | this GETs the content of video page at
$m->get($_)->content =~             | $_, then matches it against a regular
/l_map": .+(?:%2C)?5%7C(.+?)"/);    | expression and captures the video url
                                    | for flv video (id 5). this is evaled in
                                    | list context, therefore the capture goes
                                    | into $u variable, which gets url
                                    | unescaped by s/%(..)/chr(hex($1))/ge
------------------------------------+-----------------------------------------
$m->get($u, ":content_file" =>      | this GETs the video file that was in $u 
            "$i.flv")               | variable and saves it to file "$i.flv"
------------------------------------'-----------------------------------------

That's it. :)

------------------------------------------------------------------------------


Happy downloading!


Sincerely,
Peteris Krumins
http://www.catonmat.net

