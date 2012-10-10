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

Rather than diving in to recreate visuals, start each project by defining 'blocks', patterns and modules of components from visual printouts. Look for potential development hurdles, inconsistencies or UX anti-patterns and iron them out before you start the build to avoid ambiguity and delays. Always think about how things will work in different situations and the visuals are not always a means to an end, for instance, how should things behave:

* on mobile
* at different screen widths
* with larger or smaller amounts of content than the visuals show
* at different font-sizes (fluid/relative typography)
* with different image aspect ratios
* without a certain required browser feature
* without a pointer (touch devices)

Note that just because a component appears to be different, structure could remain the same. Presentation and behaviour could be determined by a sub-class or modifier. Read the [BEM methodology](http://coding.smashingmagazine.com/2012/04/16/a-new-front-end-methodology-bem/).

**Separate structure from presentation from behavior**. Strictly keep structure (markup), presentation (styling), and behavior (scripting) apart, and try to keep the interaction between the three to an absolute minimum.

Components should be portable with the potential of several instances of them working in unison. While hooking on an elements `id` attribute has performance gains, try to avoid unique IDs on elements and **always opt for using `class` instead**. Modules shouldn't have to know about the system and relying on there being one instance of a component is not a good idea.

Avoid over-specificity, and abstract things where you can.

Always be agnostic and assume nothing. Consider implications of a solution in another situation (i.e. mobile, touch device, browsers without a required feature). **Progressive enhancement is key**.

**Don't compromise current and future browsers to adhere to legacy ones**, it is short-sighted. That said, do provide a solution for them, even if it is a more basic experience.

### General Formatting

* Indent correctly and ensure there is no unnecessary whitespace.
* Use 4 spaces for indentation and avoid tabs (configure your IDE to take care of this for you).

### Progressive enhancement

Responsive design implementation can be messy. Regressively stripping back a desktop-optimised site for mobile devices is less efficient to develop and has a negative impact on the user.

For instance, markup and CSS is purposely built for a complex interactive component that is entirely unusable on mobile devices; this component is now set in stone and the only option we have to serve mobile users is to remove it entirely on the client-side. Choosing to remove it is under the presumption that this content is not important to mobile users, and **they still get the overhead anyway**.

Instead, we can use progressive enhancement.

Progressive enhancement is the concept of adding functionality and decoration from a baseline. Adding these features is entirely dependant on the browsers capability and size. It is important that we don't use user-agent sniffing as it is a short-term solution that doesn't solve the underlying problem. Testing the browser for a feature at runtime on the client is a much more reliable method and ensures the product is future-proof.

This workflow and mindset will give you a range of experiences, 'tailored' to the device, for free.

## HTML

* Use HTML5 `<!doctype html>`. Include an HTML5 shiv to polyfill browsers that do not support HTML5 elements natively.
* Use valid HTML, check using a validator as often as you can.
* Always close elements. Failing to do this can cause unexpected behaviour in certain browsers.

### Head

* Use UTF-8
* Always include a title
* Provide meta data (only if it's useful, no keywords)
* Make use of the canonical meta if the content of a URI is a direct duplicate of another, see SEO/crawlability
* Order of elements should always be meta, styles, scripts.


### Semantics and accessibility

Always use live text, images with alt attributes for titles is not a solution. If a visual isn't achievable without compromising this, flag it up.

Avoid '`div` soup' and always use appropriate semantic elements for the benefit of accessibility and native functionality. If in doubt, use [HTML5 doctor](http://html5doctor.com) as a reference.

Your markup should be semantically constructed and coherent, even if the presentation requires something to appear separated. If you are relying on styles or scripting for content to become understandable, then it is no longer portable or accessible.

#### Example

A visual shows a tabbed UI, which could be implemented using:

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
    
Used for a collection of (generally internal) navigation items. This can include a search form.

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

Separate structure from presentation, and containers from components. Components should **always** be built to be flexible and fluid; let the grid / containers determine it's width, and the content determine it's height.

If visuals contain redundant inconsistencies of very slightly different rules, merge them for consistency. Creating unnecessary style rules or duplicates of code blocks is inefficient and confusing. For instance, a gutter of 18px and a gutter of 21px probably shouldn't be imitated and it would be a good idea to keep the value consistent so that building blocks are **predictable** and portable.

#### Anti-patterns

##### Redundant specificity

IDs should only appear once. There is no need for defining any more specificity than a straight forward ID selector. However, you should try to avoid styling specific elements anyway, as it requires the presentation to know about the system and goes against the concept of being modular.

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
    
##### Property definition

Keep the order of how you define properties consistent. Define layout properties first and then decoration.

    // Anti-Pattern
    
    .box {
        background-color: red;
        width: 25%;
        border-bottom: black solid 1px;
        height: 100px;
        position: relative;
        display: block;
    }
    
    .block {
        height: 100px;
        border-bottom: black solid 1px;
        width: 25%;
        display: inline-block;
        background-color: red;
    }
    
    // Pattern
    
    .box {
        display: block;
        position: relative;
        width: 25%;
        height: 100px;
        border-bottom: black solid 1px;
        background-color: red;
    }

#### Safe selectors

* **IE7+** `>`, `+`, `~`, `:first-child`, `:hover`
* **IE8+** `:focus`, `:before`, `:after`
* **IE9+** `:last-child`, `:nth-of-type`

[Support table](http://kimblim.dk/css-tests/selectors/)

#### Formatting

Write curly brackets on the same line as the selector.

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
    
Lowercase hyphenated properties (underscores for IDs, to adhere to JS code convention).

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
    
### LESS

We are currently using LESS, a CSS pre-processor, to write style rules for it's variables, calculations, imports and mix-in functionality. Read the [LESS documentation](http://lesscss.org) for more info. Ensure you keep your LESS components up to date or unexpected (or silent) errors can occur during compilation. We do not use the client-side features of LESS, and always compile CSS into a /styles/css/ directory.

Get the [LESS app](http://incident57.com/less/) for OS X or [WinLess](http://winless.org) for Windows. Ubuntu users can learn about GUIs [here](http://en.wikipedia.org/wiki/Gui).

#### Variables

Store your global variables in styles/global.less and import this to all other LESS files to access them. A good idea is to set your colour palette and base units here so you can use them consistently everywhere, if they need to change for any reason, it's simply a case of changing them once.

    @v_gutter: 18px;
    @h_gutter: 24px;
    
    @color_a: #222222; // Dark grey
    @color_b: #E11010; // T.C red
    @color_b_hl: lighten(@color_b, 15%); // T.C red highlight

#### Imports

Import other LESS files at the top of your file to make your dependencies clear. Importing compiles the file into the current stylesheet, it does not do this by reference so **use it wisely**. Don't import files for the sake of it or you will end up with duplicate code, potentially harming your cascade. The exception for this is the global.less file, which should only ever contain variables and mix-ins that will be ignored in the output CSS.

    @import "global";
    @import "typography";

#### Mix-ins

Mix-ins allow you to inherit properties from other style blocks. Ensure you invoke mix-ins as 'functions' by using parentheses, this will stop the mix-in block from being output as CSS during compilation. Additionally, you can pass parameters.

    .font-size(@font_size) {
        @rem: (@font_size / 10);
        font-size: @font_size * 1px;
        font-size: ~"@{rem}rem";
    }
    
    h1 {
        .font-size(16);
    }
    
Compiles to:

    h1 {
        font-size: 16px; // no rem support fallback
        font-size: 1.6rem;
    }
    
#### Prefixes

Set the un-prefixed property last, as the prefixed properties will inevitably be phased out by vendors. Define a mix-in to abstract prefixed properties for convenience.

    .background-size(@background_size) {
        -webkit-background-size: @background_size;
        -moz-background-size: @background_size;
        -o-background-size: @background_size;
        background-size: @background_size;
    }
    
#### Nesting

Nesting selectors helps keep things modular and readable. Use a line-break before defining nested selectors for readability.

    .related-area {
    
        .related-header {
            ...
        }
    
        .product {
            ...
        }
        
        .cta {
            ...
        }
        
        &.theme-a {
            ...
        }
    }
    
Compiles to:

    .related-area .related-header {
        ...
    }
    
    .related-area .product {
        ...
    }
    
    .related-area .cta {
        ...
    }
    
    .related-area.theme-a {
        ...
    }
    
However, be cautious of overusing nesting and treating it like markup structure, this causes over-specificity and CSS bloat.

### Progressive enhancement

A custom build of Modernizr is used in all of our projects, along with IE conditional classes and a JavaScript-enabled class. Classes are added to the html element, allowing us to hook in to features in our styles.

#### Feature class hooks

A slideshow requires slides to be contained within a slide-wrapper through absolute positioning. This is not a viable solution for devices without Javascript as there will be no behaviour to transition between items. Instead, we can choose to simply stack the slides vertically.

    .no-js {
    
        .slide-wrapper {
        
            .slide {
                margin-bottom: @v_gutter;
            }
        }
    }
    
    .js {
    
        .slide-wrapper {
            position: relative;
            width: 100%;
            height: 0;
            padding-bottom: 56.25%; // 16:9 aspect ratio fix
        
            .slide {
                position: absolute;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                z-index: 0;
                opacity: 0;
                
                &.is-active {
                    z-index: 1;
                    opacity: 1;
                }
            }
        }
    }
    
#### Style modules

Alternatively, you can choose to keep styles for a certain feature within it's own file and include it with an AMD type approach. An example for this is including hover styles / UI specific to devices with a pointer in a file 'no_touch'. Devices with touch support can instead be served a file 'touch'.

### Typography

Typography is a good place to start within a project, by initially defining type styles, things fall into place throughout.

#### Typefaces

Font stacks allow browsers to gracefully fall back from left to right if fonts are not available. Ensure everyone is covered by including a default system font last. [CSS font-stack](http://cssfontstack.com) is a good resource for providing information on cross-platform font availability and appropriate fallback fonts.

    @face_a: "Helvetica Neue", HelveticaNeue, Helvetica, Arial, sans-serif;
    @face_b: Baskerville, "Baskerville Old Face", Garamond, "Times New Roman", serif;
    
    body {
        font-family: @face_a;
    }
    
    blockquote {
        font-family: @face_b;
        font-style: italic;
    }
    
If the project requires a non-system font, font-face can be used **providing we have a license**. A font-face mix-in is available in the template.

    @font_path: '../../styles/fonts/'; // defined in global

    .font-face (@family_name, @path, @font_weight: normal, @font_style: normal) {
        @font-face {
            font-family: @family_name;
            src: url('@{font_path}@{path}.eot');
            src: url('@{font_path}@{path}.eot?#iefix') format('embedded-opentype'),
                 url('@{font_path}@{path}.woff') format('woff'),
                 url('@{font_path}@{path}.ttf') format('truetype'),
                 url('@{font_path}@{path}.svg#@{family_name}') format('svg');
            font-weight: @font_weight;
            font-style: @font_style;
        }
    }
    
    .font-face('DecimaMonoRegular', 'decima_mono_regular/decima_mono-webfont', normal, normal);

#### Baseline grid

A baseline grid achieves vertical rhythm that results in balanced, predictable layout and neat typography.

Factors that affect the baseline grid are:

* line-height (leading)
* margins
* padding
* borders
* element heights

These should be calculated with care and kept consistent throughout to keep the rhythm. To do this, we can use multiples or divisions of a base unit.

A base unit should be derived from your base leading which is known as the 'magic number'. 16 or 18 are a good place to start, as they are easily divisible and readable for body copy leading. Always set a default on the body element so that elements inherit properties by default.

    @size_base: 13px;
    @leading_base: 18px; // Magic number

    body {
        font-size: @size_base;
        line-height: @leading_base;
    }
    
    h1 {
        font-size: @size_base * 2;
        line-height: @leading_base * 2;
        margin-bottom: @leading_base * 2;
    }
    
    h2 {
        font-size: @size_base * 1.5;
        line-height: @size_base * 1.5;
        margin-bottom: @leading_base;
    }
    
    p {
        margin-bottom: @leading_base;
    }

[Read about baseline grids in CSS](http://webdesign.tutsplus.com/articles/design-theory/setting-web-type-to-a-baseline-grid/).

#### Hierarchy

TBC

#### Relative units

TBC

#### Specimens

TBC

### Layout

#### Grids

TBC

#### Fluid containers

Responsive layout requires elements to be constantly in flux dependant on browser width. Because of this, it is imperative that we make all container elements (or columns) fluid and **avoid fixed widths**, opt for percentage values instead. The inner components should be allowed to flow and fill their containers. Set as little widths as possible, and let the grid do the work.

Similarly, it should be left to the content to determine the height, rather than setting an explicit value. If fixed heights are unavoidable, ensure content is being properly truncated or cropped without visibly flowing outside it's container.

#### Baseline grid

To maintain the baseline grid determined by your typography, use consistent vertical spacing.

    @v_gutter: 18px; // defined in global

    .section {
        margin-bottom: @v_gutter * 2;
    }
    
    .section-header {
        padding-top: @v_gutter / 2;
        padding-bottom: @v_gutter / 2;
        border-bottom: @color_a solid 1px;
        margin-bottom: @v_gutter - 1;
    }

#### Box model

A block-level element's total width is determined by its width + padding + border. Margins are rendered outside of the element, affecting the surrounding elements layout.

If an elements `box-sizing` property is changed to `border-box`, as opposed to the default `content-box`, padding and border widths are included **within the elements width**. Therefore we can set a fluid width with a fixed padding value, making it an ideal solution for fluid grid-based layout.

    .grid-item {
        .inline-block;
        width: 25%;
        padding-right: @h_gutter / 2;
        padding-left: @h_gutter / 2;
    }
    
If our intention is to create a 4 column grid, this would not work as the elements total width would exceed 25%.

    .grid-item {
        .inline-block;
        box-sizing: border-box;
        width: 25%;
        padding-right: @h_gutter / 2;
        padding-left: @h_gutter / 2;
    }
    
By changing the elements `box-sizing` to `border-box`, the padding and properties are included in the width.

The major flaw in this layout technique is **there is no support in IE7**. A potential workaround is to reduce the width percentage and make up the difference using percentage padding values (for IE7 only).

    .ie-lt8 {
        
        .grid-item {
            width: 23%;
            padding-right: 1%;
            padding-left: 1%;
        }
    }
    
Alternatively as a more robust solution, you can apply the padding to an inner element. However, this does contribute to un-semantic markup bloat.

    // HTML
    
    <div class="grid-item">
        <div class="inner">
            ...
        </div>
    </div>
    
    // LESS
    
    .grid-item {
        .inline-block;
        width: 25%;
        
        > .inner {
            padding-right: @h_gutter / 2;
            padding-left: @h_gutter / 2;
        }
    }

#### Breakpoints

If we require a major change where the layout appears to break, media queries can be used to apply rules under certain conditions. This is one of the core techniques in applying a responsive design.

    @media only screen and (max-width: 800px) {
    
        .grid-item {
            width: 20%;
        }
        
    }
    
    @media only screen and (max-width: 799px) {
        
        .grid-item {
            width: 25%;
        }
        
    }

#### Float vs inline-block vs multi-columns

TBC

#### Anti-patterns

TBC

## Javascript (TBC)

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
