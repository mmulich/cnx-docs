==================
Framework Analysis
==================


The frameworks/technologies under analysis are as follows:

- `Responsive with Twitter Bootstrap atop Pyramid with Jinja2
  <http://static3.cnx.mulich.com>`_
  (`rhaptos_viewer <https://github.com/pumazi/rhaptos_viewer>`_)
- `Pyramid 1.3 with Jinja2 2.6 <http://static2.cnx.mulich.com/>`_
  (`webview <https://github.com/Connexions/webview>`_)
- `Django 1.4 with Jinja2 2.6 <http://static1.cnx.mulich.com/>`_
  (`cnx-django-webview <https://github.com/pumazi/cnx-django-webview>`_)
- `html5 with JQuery 1.7 <http://static0.cnx.mulich.com/>`_
  (`cnx-html5-webviewer <https://github.com/pumazi/cnx-html5-webviewer>`_)

Templating with Jinja2 and Html5
--------------------------------

In the Python framework cases I choose the `Jinja2 templating
framework <http://jinja.pocoo.org/docs/>`_,
because it is:

1. Widely used across many Python web frameworks
2. Easy to understand (extend, include, blocks, etc.) and 
   in many cases an intuitive person can use it without constantly
   referencing documentation
3. Easy to extend and reuse the extensions across other applications.

In the Pyramid case(s) I am using the `pyramid_jinja2
<http://pypi.python.org/pypi/pyramid_jinja2>`_ integration
package, because by default Pyramid ships with TAL. There is word that
the framework will drop support for a templating language out of the
box, so there is no need to worry if one way is the best way. The
pyramid_jinja2 package doesn't do anything fancy. It simply pulls in
the Jinja2 package and registers a template renderer for files with
the `.jinja2` extension.

Pyramid by default uses the TAL templating system. Many of the pyramid
plugins also use the TAL templating system. But this has very little
effect on the running application because each piece runs independent
of the other. For example, Deform (a library for building web forms)
uses TAL templates to produce the individual form elements. If a
change needs to be made to one of the default templates, one can
either use the TAL template as a base or rewrite it in Jinja2. And
because these widgets aren't large pieces of code, this is often a
medial task.

Django uses a slightly basterdized version of Jinja2, but for the most
part, the syntax remains the same. In my exploration so far, I have
run into one small difference, however I'm fairly confident that there
are other differences. For example, Django looks up the parent block
information using the `{{ block.super }}` statement, while vanilla
Jinja2 simply uses `{{ super() }}`, which is actually more pythonic.

Reusing a snippet of markup in Jinja2 is fairly easy with the Jinja2's
include statement. And for more detailed situations, Jinja2 provides
macros that often contain more logic. I know that one of the primary
concerns is to keep logic out of the templates. I would say that
sometimes it can't be helped, but those cases are few and far
between. In the case that more logic is needed, a macro can be used to
isolate that piece of logic rather than jamming it into the
template. These include and macro features could make it nice for us
to find a happy medium between producing markup in code or in the templates.


In the html5 with JQuery solution the templating system is JQuery
itself. I know many developers dislike building content client side,
because there are many unknowns. At this point, the content we are
working with from cnx.org is clean and clear. However, we still have
an issue with the module content body references (See the issues section).


Failover
--------

All of these frameworks have failover. The static html5 solution is
dependend on the failover of the webserver. Similarly, the Python
frameworks have failover throught the webserver, if they are being run
under the WSGI specification.

Other
-----

I lean in the favor of using a server side framework (pyramid, django,
flask, etc.), because it will allow the developer to have a higher
level of control over the end result. Using JQuery with a static
html5 page does not give us enough control over the display.

Currently, the only downside to using html5 with JQuery are the
URLs. This solution uses the hashbang URL formatting. This formatting
was initially introduced by Google to allow them to index pages that
contain AJAX acquired content. The hashbang syntax is used by many new
projects, but it is quickly being pushed out of favor with the
introduction of html5's javascript history. Twitter currently uses the
hashbang URL structure, but is in the process of moving away from this
URL format. In order to make our html5 version of this code work
without hashbangs we will need to put a route aware server in front of
it. Or, we could use an apache rewrite rule on the server to fake the
URLs.

Some of the things I believe we need to answer before we make a
decision on which frame work to go with are: 1) where is this
application going to be run? 2) How far are we going to take this?
3) Are we going to be reusing any of this implementation in other
applications?


Issues
------

* The body content contains relative URLs based on the repository's
  URLs, which may not be the same as the webview's URLs. In each of
  these frameworks the content from the rhaptos repository needs to be
  adjusted. This is a PITA in all the frameworks.

* Searching fine when done in the Python frameworks, but a decent
  amount of work would be required to make opensearch work within a
  stand-alone webpage. This is because opensearch is typically used
  through the browser itself.
  We could add webview to the browsers opensearch
  registry, but IMHO this is still confusing for some users. The
  confusion occures because the opensearch interface is not
  standardized in all browsers. For example, Chrome uses shorcut
  keyword based searching, which leads users to often believe the same
  search boxes that exist on IE and Firefox do not exist in Chrome.

* There doesn't appear to be an easy way to acquire Collection
  data. Many of the `documented API calls for Collections
  <https://trac.rhaptos.org/trac/rhaptos/wiki/API/WebServices#CoursesCollections>`_
  do not exist. This has limited the ability to get meaningful data
  about the collection. For the time being the some of the examples
  use methods that are built on the collection model to produce an
  html content tree. This is not the ideal solution.
