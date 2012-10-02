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

Work in progress.

## Workflows

Start each project by defining 'blocks', patterns and modules of components from visual printouts. Note that just because a component appears to be different, the structure could remain the same. Presentation and behaviour could be determined by a sub-class or modifier. Read the [BEM methodology](http://coding.smashingmagazine.com/2012/04/16/a-new-front-end-methodology-bem/).

Look for potential development hurdles, inconsistencies or UX anti-patterns and iron them out before you start the build to avoid falling over at a later stage.

**Separate structure from presentation from behavior**. Strictly keep structure (markup), presentation (styling), and behavior (scripting) apart, and try to keep the interaction between the three to an absolute minimum.

Components should be portable with the potential of several instances of them working in unison. While hooking on an elements `id` attribute has performance gains, try to avoid unique IDs on elements and **always opt for using `class` instead**. Modules shouldn't have to know about the system.

Avoid over-specificity, and abstract things where you can.

Always be agnostic and assume nothing. Consider implications of a solution in another situation (i.e. mobile, touch device, browsers without a required feature). **Progressive enhancement is key**.

A good idea is to always think **mobile-first**, that is, not designing for the lowest denominator but starting with a core experience and adding functionality and presentation as and when you need it **dependant on the device**. This workflow and mindset will give you a range of experiences, 'tailored' to the device, for free.

**Don't compromise current and future browsers to adhere to legacy ones**, it is short-sighted. That said, do provide a solution for them, even if it is a more basic experience.

### General Formatting

* Indent correctly and ensure there is no unnecessary whitespace.
* Use 4 spaces for indentation and avoid tabs (configure your IDE to take care of this for you).

## HTML

* Use HTML5 `<!doctype html>`. Include an HTML5 shiv to polyfill browsers that do not support HTML5 elements natively.
* Use valid HTML, check using a validator as often as you can.
* Always close elements. Failing to do this can cause unexpected behaviour in certain browsers.

### Head

* Use UTF-8
* Always include a title
* Provide meta data (only if it's useful, no keywords)
* Make use of the canonical meta if the content of a URI is a direct duplicate of another, see SEO/crawlability.


### Semantics and accessibility

Always use live text, images with alt attributes for titles is not a solution. If a visual isn't achievable without compromising this, flag it up.

Avoid '`div` soup' and always use appropriate semantic elements for the benefit of accessibility and native functionality. If in doubt, use [HTML5 doctor](http://html5doctor.com) as a reference.

Your markup should be semantically constructed and coherent, even if the presentation requires something to appear separated. If you are relying on styles or scripting for content to become understandable, then it is no longer portable or accessible.

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
    
The concept of this content being a tabbed card set is *presentational* and *behavioral* and should be treated as an enhancement. Therefore using Javascript, you can manipulate this structure on the client and style it accordingly. A benefit of this is that you may not want a tabbed UI in certain situations, i.e. mobile devices, so no additional fallback work is required.

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
    
Used for a collection of navigation items. This can include a search form.

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

### Structure separation

**Never use inline CSS or Javascript**. The only exception for this is if properties are set dynamically. For example, a background image set using a CMS, or binding server-side data to an element. Avoid invoking any functions at this point as your dependencies may not be ready.

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

### General good practise

**Be generic**.

Separate structure from presentation, and containers from components. Components should **always** be built to be flexible and fluid; let the grid determine it's width, and the content determine it's height.

If visuals contain redundant inconsistencies of very slightly different rules, merge them for consistency. Creating unnecessary style rules or duplicates of code blocks is inefficient and confusing. For instance, a gutter of 18px and a gutter of 21px probably shouldn't be imitated and it would be a good idea to keep the value consistent so that building blocks are **predictable**.

#### Anti-patterns

##### Redundant specificity

IDs should only appear once anyway. There is no need for defining any more specificity than a straight forward ID selector. Saying that, you should try to avoid styling specific elements anyway, as it requires the presentation to know about the system.

    // Anti-pattern
    
    ul#something {
        ...
    }
    
    // Pattern

    #something {
        ...
    }
    
##### * Selector

Only use `*` as a standalone universal selector. Don't use it as a descendant selector as performance and render speeds can take a hit.
    
    // Anti-pattern
    
    .blah * {
        ...
    }
    
    // Pattern
    
    .blah .child-a,
    .blah .child-b {
        ...
    }
    
    * {
        ...
    }
    
##### Description of presentation

Do not use class names that describe presentation, as this is subject to change for any number of reasons.
    
    // Anti-pattern
    
    .colour-green {
       color: green;
    }
    
    .width-70-percent {
       width: 70%;
    }
    
    // Pattern
    
    .theme-a {
        color: green;
    }
    
    .content-area {
        width: 70%;
    }
    
##### !important overrides

`!important` shouldn't be used, respect your cascade. The exception being if there is the need for overriding styles that are set on the client by a third-party.
    
    // Anti-Pattern
    
    .form .input-wrapper input {
        background-color: white;
    }
    .search input {
        background-color: hotpink !important;
    }
    
    // Pattern

    .form .input-wrapper input {
        background-color: white;
    }
    .search .input-wrapper input {
        background-color: hotpink;
    }

#### Safe selectors

* **IE7+** `>`, `+`, `~`, `:first-child`, `:hover`
* **IE8+** `:focus`, `:before`, `:after`
* **IE9+** `:last-child`, `:nth-of-type`

[Support table](http://kimblim.dk/css-tests/selectors/)

### LESS

We are currently using LESS, a CSS preprocessor, to write style rules for it's variables, calculations, imports and mix-in functionality. Read the [LESS documentation](http://lesscss.org) for more info. Please ensure you keep your LESS components up to date or unexpected (or silent) errors can occur during compilation. We do not use the client-side features of LESS, and always compile CSS into a /styles/css/ directory.

Get the [LESS app](http://incident57.com/less/) for OS X or [WinLess](http://winless.org) for Windows. Ubuntu users can learn about GUIs [here](http://en.wikipedia.org/wiki/Gui).

#### Formatting

Write parentheses on same line as selector.

    // Anti-pattern
    
    .blah
    {
    
    }
    
    // Pattern
    
    .blah {
    
    }
    
Group selectors on multiple lines for quick readability.

    // Anti-pattern
    
    .one, .two, .three {
        ...
    }
    
    // Pattern
    
    .one,
    .two,
    .three {
        ...
    }

Add space between key and value.

    // Anti-pattern
    
    .one {
        key:value;
    }
    
    // Pattern
    
    .one {
        key: value;
    }
    
Lowercase hyphenated properties (underscores for IDs, as they are more likely to be used for scripting).

    // Anti-pattern
    
    .productItem {
        ...
    }
    
    .product_item {
        ...
    }
    
    #Component-1 {
        ...
    }
    
    #COMPONENT1 {
        ...
    }
    
    // Pattern
    
    .product-item {
        ...
    }
    
    #component_1 {
        ...
    }

#### Variables

Store your global variables in styles/global.less and import this to all other LESS files to get access to them. A good idea is to set your colour palette and base units here so you can use them consistently everywhere, if they need to change for any reason, it's simply a case of changing them once.

    @v_space: 18px;
    @h_space: 24px;
    
    @color_a: #222222; // Dark grey
    @color_b: #E11010; // T.C red
    @color_b_hl: lighten(@color_b, 15%); // T.C red highlight

#### Imports

Imports here

#### Mixins

Mixins here

#### Prefixes

Prefix rules here

#### Progressive enhancement

HTML class hooks

### Layout patterns

* Inline block, block, inline
* Percentages
* Box model
* Vertical rhythm
* Multicolumns
* Patterns, anti-patterns (floats, clears)

### Typography patterns

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
