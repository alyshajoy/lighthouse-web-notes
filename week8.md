# Lecture on CSS

* HTML should be used to build the skeleton of the page.
* CSS should be used to style the page.
* JS should be used for the functionality.

### Alternatives to using div:
* article
* aside
* details
* figure
* header
* footer
* nav
* section
* summary
* time
* there are more... you can find lists online

These work the same as divs, but provide us with semantic clarity so that our html is easier to read

Using proper alternatives to div helps with SEO.

* main p {} - all descendents of main parent
* main > p {} - only the children of main parent

### CSS Pseudo-Classes
* used to define a special state of an element
  * eg. style when mouse hovers over it, or style that changes a link that has already been visited

::after pseudo class:
* dynamically inserts html content
* ie. p::after - inserts content before every <p> element
* not used often, as this type of behaviour should be done in html, not css

* when using box model, use border-box to have width include the content size, padding and border. Otherwise, width only includes the content
* to use: < style > { box sizing: border-box; }

* you want to do a restart to remove any of the browser's styling (that we didn't create ourselves with our CSS)
* you can find CSS reset tools online (eg. Meyer reset)... 
  * you can link to it to your index.html file, or copy/paste it into your CSS file