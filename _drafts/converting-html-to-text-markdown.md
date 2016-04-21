Converting HTML to Text or Markdown
===================================

http://www.cantoni.org/2013/01/19/converting-html-to-text-markdown

I’m working on a documentation project where I might need to convert
some existing HTML pages back into text or Markdown format for the new
system. Rather than manually editing the HTML source, I’m testing with a
couple different ways to script it automatically. In the examples below,
I’m using a documentation page for our GoToMeeting API method [Get
Meetings](https://developer.citrixonline.com/api/gotomeeting-rest-api/apimethod/get-meetings).

Lynx
----

[Lynx](http://lynx.isc.org/ "Lynx Text Web Browser") is an open-source
text web browser that is usually present on Linux machines and can be
installed for Mac and Windows. I’ve used it in the past to see how web
pages will appear to search engines or for [accessibility
testing](http://chronicle.com/blogs/profhacker/using-lynx-to-test-modern-websites-for-accessibility/31731 "Using Lynx to Test Modern Web Sites for Accessibility").
In both cases, you can quickly tell whether your text is sufficiently
communicating your content.

For the case of saving web pages in text format, Lynx also has a
command-line option “-dump”:

    $ lynx -dump http://www.whatismyip.com/ > example.txt

In my test case I couldn’t convince Lynx to fetch an SSL page, so I
download it with Curl and pipe it into Lynx:

    $ curl --silent https://developer.citrixonline.com/api/gotomeeting-rest-api/apimethod/get-meetings | lynx -dump -stdin > lynx.txt

Here's a sample section of the output:

    URL
    https://api.citrixonline.com/G2M/rest/meetings
    Method
    GET
    Response Type
    JSON
    Parameters
    scheduled A string "true" to get all future meetings.
    history A string "true" to get past meetings within date range.
    startDate If history=true, required start of date range, in ISO8601 UTC
    format.
    endDate If history=true, required end of date range, in ISO8601 UTC
    format.

Pandoc
------

[Pandoc](http://johnmacfarlane.net/pandoc/ "Pandoc - Universal Document Converter")
is an open-source "universal document converter" which understands (and
can convert between) about two dozen different formats. It's well suited
for writing a document in a primary source, then converting to other
formats for different publishing options.

The option we'll use here is Pandoc's ability to convert from HTML to
Markdown, for example:

    $ pandoc -s -r html http://www.whatismyip.com/ -o pandoc.md

For my page, I use the same trick as above because Pandoc can't connect
to SSL directly:

    $ curl --silent https://developer.citrixonline.com/api/gotomeeting-rest-api/apimethod/get-meetings | pandoc -s -r html -o pandoc.md

And here's the sample output of the same section as above:

    ### URL
    https://api.citrixonline.com/G2M/rest/meetings
    ### Method
    GET
    ### Response Type
    JSON
    ### Parameters
    **scheduled** A string "true" to get all future meetings.
    **history** A string "true" to get past meetings within date range.
    **startDate** If history=true, required start of date range, in ISO8601
    UTC format.
    **endDate** If history=true, required end of date range, in ISO8601 UTC
    format.

Conclusion
----------

Both of these options do a pretty decent job of converting HTML into
text or Markdown format. Pandoc seems slightly better in terms of
getting to Markdown format, but I would need to run some more samples to
see how much manual editing would be needed after.

I'm also going to play a bit more with Aaron Schwartz's
[Html2Text](https://github.com/aaronsw/html2text). In my quick test, it
appeared to have a problem with malformed HTML so I need to do some
further testing with it.

