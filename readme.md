# Codename: Velociraptor
---

This guide aims to unify front-end development techniques and achieve:

* Quality code
* Best practise
* Being cross-platform
* Consistency within a project
* Portability between projects
* Better readability
* Common vocabulary
* True happiness

Please note this is work in progress.

## Workflows

Start each project by defining 'blocks', patterns and modules of components from visual printouts. Note that just because a component appears to be different, the structure  could remain the same. Presentation could be determined by it's location. For instance, a CTA block should not be written twice; it can be presented differently depending on whether it appears in an aside or a feature grid. Similarly, a subscribe form could have the same behaviour as a search form, but can be presented differently. Read the [BEM methodology](http://coding.smashingmagazine.com/2012/04/16/a-new-front-end-methodology-bem/).

Look for potential development hurdles, inconsistencies or UX anti-patterns and iron them out before you start the build to avoid falling over at a later stage.

**Separate structure from presentation from behavior**. Strictly keep structure (markup), presentation (styling), and behavior (scripting) apart, and try to keep the interaction between the three to an absolute minimum.

Always be agnostic and assume nothing. Consider implications of a solution in another situation (i.e. mobile, touch device, browsers without a required feature). **Progressive enhancement is key**.

Components should be portable with the potential of several instances of them working in unison. While hooking on an elements `id` attribute has performance gains, try to avoid unique IDs on elements and **always opt for using `class` instead**.

Avoid over-specificity, and abstract things where you can.

### General Formatting

* Use 4 spaces for indentation and avoid tabs (configure your IDE to take care of this for you).


## HTML

* Use HTML5 `<!doctype html>`. Include an HTML5 shiv to polyfill browsers that do not support HTML5 elements natively.
* Use valid HTML, check using a validator as often as you can.
* Always close elements. Failing to do this can cause unexpected behaviour in certain browsers.

### Head

* Use UTF-8
* Always include a title
* Provide meta data (only if it's useful, i.e. not keywords)
* Make use of the canonical meta if the content of a URI is a direct duplicate of another or Google will get grumpy.


### Semantics and accessibility

Avoid '`div` soup' and always use appropriate semantic elements for the benefit of accessibility and native functionality. If in doubt, use [HTML5 doctor](http://html5doctor.com) as a reference.

Your markup should be semantically constructed and coherent, even if the presentation requires something to appear separate. If you are relying on styles or scripting for content to become understandable, then it is no longer portable or accessible.

For example, a visual shows a tabbed UI, which could be implemented using:

    // Anti-pattern
    
    <div class="tabs">
        <h1 class="tab">Item 1 heading</h1>
        <h1 class="tab">Item 2 heading</h1>
    </div>
    <div class="card-set">
        <section class="card">
            <p>Item 1 content</p>
        </section>
        <section class="card">
            <p>Item 2 content</p>
        </section>
    </div>
    
This requires styling and scripting to make the elements appear to be linked. To a screen-reader, crawler, or browser with Javascript disabled it is incomprehensible. 
    
    // Pattern
    
    <div class="card-set">
        <section class="card">
            <header class="tab">
                <h1>Item 1 heading</h1>
            </header>
            <div class="content">
                <p>Card 1</p>
            </div>
        </div>
        <section class="card">
            <header class="tab">
                <h1>Item 2 heading</h1>
            </header>
            <div class="content">
                <p>Card 2</p>
            </div>
        </div>
    </div>
    
The concept of this content being a tabbed card set is *presentational* and *behavioral* and should be treated as an enhancement. Therefore using Javascript, you can manipulate this structure on the client and style it accordingly.

`section`, `article`, `aside` are section level elements. All elements are semantically relative to the nearest section level element.

#### Nav

    <nav>
        <ul>
            <li><a href="#" title="">One</a></li>
            <li><a href="#" title="">One</a></li>
            <li><a href="#" title="">One</a></li>
        </ul>
        <form action="" method="get">
        	<label for="search">Search</label>
        	<input type="search" name="search" id="search" />
        	<input type="submit" value="Go" />
        </form>
    </nav>
    
Used for a collection of navigation items. This can include a search form, as this is just another way of navigating to content.

#### Section

    <section>
        <header>
            <h1>Section Heading</h1>
        </header>
        <div class="content">
            <p>Some copy</p>
        </div>
    </section>
    
Used for standalone sections on a page.

#### Article

    <article>
    	<header>
    		<h1>Article Heading</h1>
    	</header>
    	<div class="content">
    		<p>Some copy</p>
    	</div>
    	<footer>
    		<p>By Stefan Pearson</p>
    	</footer>
    </article>
    
Used for news articles, posts etc.

#### Aside

    <aside>
        <section class="cta">
        	<a href="#" title="">
        		<h1>Call to action</h1>
        		<img src="image.jpg" />
        	</a>
        </section>
    </aside>

Used for related content.

### Structure

**Never use inline CSS or Javascript**. The only exception for this is if properties are set dynamically. For example, a background image set using a CMS, or binding server-side data to an element.

    // Anti-pattern
    
    <div style="float: left;" onclick="doSomething();">…</div>
    
    // Exception
    
    <div id="component" style="background-image: url(image.jpg);">…</div>
    <script>
    	(function($, global) {
    		var component_data = {
    			a: '1',
    			b: '2'
    		};
    		$('#component').data('component_data', component_data);
    	})(jQuery, this);
    </script>

## Styles

* LESS
* Syntax
* Variables
* Mixins
* Prefixes
* Patterns
* Modernizr and progressive enhancement
* Compiling CSS

### Layout

* Inline block, block, inline
* Percentages
* Box model
* Vertical rhythm
* Multicolumns
* Patterns, anti-patterns (floats, clears)

### Typography

* Base units
* Relative ems
* Relative spacing
* Vertical rhythm

## Javascript

* Closures, modules
* Object notation
* Prototypes
* Inheritance
* jQuery

## Optimisation

* Sprite sheets
* Minimalising requests
* AMD
* PNG compression
* CSS/JS minifying
