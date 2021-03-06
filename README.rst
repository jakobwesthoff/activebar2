==========
Activebar2
==========

Scope
=====

This document wants to give a short introduction to the Activebar.
Furthermore it will present examples of how it can be embedded and used in a
website. Moreover a detailed list of possible options, which allow customizing of the
bars default behavior, is provided.

This document does not aim to explain any design decisions made during the
development. It neither is an explanation of the implementation itself. Just
the API visible to the outside world is covered by this document.


What is Activebar?
==================

Activebar is a crossbrowser information bar, which tries to mimic the look and
feel of these bars used by modern browsers. Such bars are commonly used to
display important informations to the user. Requests to open a popup window,
install some components or to remember some sort of password are prominent
examples of what these bars are commonly used for.

Unfortunately modern browsers do not allow any website to access these kind of
information bars because of certain understandable security concerns.
Activebar is written in pure javascript utilizing the jquery__ js library for
crossbrowser compatibility and ease of development. It simulates the behavior
as well as the look and feel of current browser information bars using simple
html and css. Therefore it can easily be embedded into your website to display
certain information to the user in an unobtrusive way.

__ http://jquery.com


Why I wrote Activebar in the first place
========================================

The first version of Activebar was written about two and a half years ago by
myself, to try convincing Internet Explorer 6 users, who were visiting my
website to change to a more modern and robust browser like firefox__. To
achieve this goal I showed a small information bar to any IE user containing
a message that the page might not be displayed 100% correct and that a browser
change/update would fix this.

__ http://mozilla.org/firefox

Because of an initiative of a Norwegian company called `Finn.no`__ to let the
IE6 finally die, I decided to reintegrate the information bar into my pages.
They were lost during redesigns and updates to the page layout.

__ http://finn.no

Instead of using my old implementation a rewrite using more modern javascript
technologies and toolkits seemed more appropriate to me. Therefore I created
Activebar2 which is a complete rewrite of the first version. By using jquery__
crossbrowser portability and an easy to use interface is guaranteed.

__ http://jquery.com

Even given the fact that the main intention behind the bar is to show browser
update messages, it can be used to show any kind of information you want to
present to your users with ease.


Embed Activebar into your own page
==================================

To use Activebar on your own page you need to include the jquery__ base file
and the activebar2.js__ file provided by the downloadable package below
somewhere in your html content. This can easily done using the html script
tags::

	<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
	<script type="text/javascript" src="activebar2.js"></script>

__ http://jquery.com
__ /data/activebar2/activebar-2.0.tar.gz

After the scripts have been included you can use the Activebar inside every
javascript call. In most of the cases you will want to show the Activebar
directly after some of your pages have been loaded. This can easily be done
using jquerys "document ready" shortcut::

    <script type="text/javascript">
        $(function() {
            // Everything inside this function will be automatically executed
            // after the page has been loaded and the DOM is complete
            
            $('<div></div>').html('Hello World').activebar();
        });
    </script>

This simple code snipped will show a default Activebar containing the text
"Hello World". Lets take a look what happens here in detail.

First of all a new *<div>* element is created using an appropriate jquery
call. After that jquerys method chaining feature is used to set the inner html
content of this div to "Hello World". In the last step the activebar function
is called on the created div, which turns the given jquery object into an
Activebar and automatically displays it.

The activebar function can be called on every jquery object. This includes
objects created from any element inside your page. Take a look at the `jquery
selector documentation`__ to get a better understanding of how this feature
can be used.

__ http://docs.jquery.com/Selectors

Available options
=================

The activebar method can be invoked providing a set of options to customize
all different aspects of the Activebar.

Options are simply provided as POJO__ (Plain old java(script) object) directly
to the activebar method call. This look something like this::

    $('.someClass').activebar({
        'option1': 'value1',
        'option2': 'value2',
        'option3': 'value3',
        ...
    });

__ http://en.wikipedia.org/wiki/Plain_Old_Java_Object

Look and feel
-------------

The Activebar is designed to mimic the look and feel of the information bar
used by the Internet Explorer 6. But you may want, for example, to change its
background color, the default font used for the text rendering or the image
files used for the icon and the close button. All this and much more, is
easily achievable using the appropriate options.

background:
    The background color used by the bar. You can use any valid css background
    definition for this property.
highlight:
    The background color used while the mouse is over the bar. This option
    supports any valid css background value as well.
border:
    Color of the 1px border on the bottom of the bar. Only valid css color
    values are supported by this option.
font:
    Font family used by default for any given content.
fontColor:
    Default font color used for the provided content.
fontSize:
    Default font size used for the displayed content node.
icon:
    Path and name to the icon image displayed left of the provided content.
    This icon needs be 16x16 pixels in size.
button:
    Path and name to the image used as a close button representation. The
    image needs to be 16x16 pixels in size.


Behavior
--------

The Activebar allows to link to a certain website whenever it is clicked. The
target of this linking operation can easily provided using the url option.

url:
    Target url to jump to if the bar is clicked.


More complex example
====================

After the options have been explained it is time to provide a more complete
example of how to use these kind of features::

            $('<div></div>').html('This page may not be displayed correctly in this browser. You are strongly encouraged to update to a current release of <a href="http://mozilla.org/firefox">Firefox</a>')
                            .activebar({
                                'font': 'serif',
                                'icon': 'images/information.png',
                                'url': 'http://mozilla.org/firefox'
                            });

This example will show an Activebar containing the provided text and link,
using a serif type font, showing a custom message icon and linking to the
firefox website if it is clicked.


Restricting to a special browser type
=====================================

As I explained above the main intention behind all this was to encourage
Internet Explorer users to switch or update their browser. Therefore the
message should only be displayed if such a browser is used. This can easily be
achieved using `jquerys browser detection`__ functionality::

    if ( $.browser.msie ) {
        // Put your Activebar calling code in here
    }

__ http://docs.jquery.com/Utilities/jQuery.browser

Using this technique it is moreover easily possible to restrict the message
even further by checking for browser versions for example. Take a look at the
corresponding documentation to see how this can be done.


Using together with other js libraries
======================================

Maybe you are already using other javascript libraries on your website like
script.aculo.us__ or moo.fx__. In this case you do not want jQuery to
conflict with the behaviour of the $ function already defined by this
toolkits. Therefore jquery can be told to create `no conflict`__ after it
is loaded. This is done by calling the function noConflict und using the
jQuery for all calls to jQuery after that. Using a function closure you may
even define the $ shortcut for a limited area of your website. Take a look at
the example below::

	<script type="text/javascript">
		jQuery.noConflict();
		jQuery(document).ready(function($) {
			$('<div></div>').html('Your text goes here.')
					.activebar({
						'font': 'serif',
						'icon': 'images/information.png',
						'url': 'http://mozilla.org/firefox'
					});
		});
	</script>

__ http://script.aculo.us/
__ http://moofx.mad4milk.net/
__ http://docs.jquery.com/Using_jQuery_with_Other_Libraries
