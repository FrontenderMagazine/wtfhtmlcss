# WTF, HTML and CSS?
Reasons HTML and CSS might make you say what the fuck. A curated list of commonly frustrating HTML and CSS quandaries, miscues, and dilemmas.

### Contents

### Declare a doctype

Always include a doctype. I recommend the simple HTML5 doctype:

[Skipping the doctype can cause issues][1] with malformed tables, inputs, and
more as the page will be rendered in quirks mode.

### Box model math

Elements that have a set `width` become *wider* when they have `padding` and/or
`border-width`. To avoid these problems, make use of the now common 
[`box-sizing: border-box;` reset][2].

### Rem units and Mobile Safari

While Mobile Safari supports the use of `rem`s in all property values, it seems
to shit the bed when`rem`s are used in dimensional media queries and infinitely
flashes the page's text in different sizes.

For now, use `em`s in place of `rem`s.

    <span class="nt">html</span> <span class="p">{</span>
      <span class="k">font-size</span><span class="o">:</span> <span class="m">16px</span><span class="p">;</span>
    <span class="p">}</span>
    
    <span class="c">/* Causes flashing bug in Mobile Safari */</span>
    <span class="k">@media</span> <span class="o">(</span><span class="nt">min-width</span><span class="o">:</span> <span class="nt">40rem</span><span class="o">)</span> <span class="p">{</span>
      <span class="nt">html</span> <span class="p">{</span>
        <span class="k">font-size</span><span class="o">:</span> <span class="m">20px</span><span class="p">;</span>
      <span class="p">}</span>
    <span class="p">}</span>
    
    <span class="c">/* Works great in Mobile Safari */</span>
    <span class="k">@media</span> <span class="o">(</span><span class="nt">min-width</span><span class="o">:</span> <span class="nt">40em</span><span class="o">)</span> <span class="p">{</span>
      <span class="nt">html</span> <span class="p">{</span>
        <span class="k">font-size</span><span class="o">:</span> <span class="m">20px</span><span class="p">;</span>
      <span class="p">}</span>
    <span class="p">}</span>
    

**Help!** *If you have a link to an Apple or WebKit bug report for this, I'd
love to include it. I'm unsure where to report this as it only applies to Mobile,
and not Desktop, Safari.*

### Floats first

Floated elements should always come first in the document order. Floated
elements require something to wrap around, otherwise they can cause a step down 
effect, instead appearing below the content.

    <span class="nt"><div</span> <span class="na">class=</span><span class="s">"parent"</span><span class="nt">></span>
      <span class="nt"><div</span> <span class="na">class=</span><span class="s">"float"</span><span class="nt">></span>Float<span class="nt"></div></span>
      <span class="nt"><div</span> <span class="na">class=</span><span class="s">"content"</span><span class="nt">></span>
        <span class="c"><!-- ... --></span>
      <span class="nt"></div></span>
    <span class="nt"></div></span>
    

### Floats and clearing

If you float it, you *probably* need to clear it. Any content that follows an
element with a`float` will wrap around that element until cleared. To clear
floats, use one of the following techniques.

Use [the micro clearfix][3] to clear your floats with a separate class.

    <span class="nc">.clearfix</span><span class="nd">:before</span><span class="o">,</span>
    <span class="nc">.clearfix</span><span class="nd">:after</span> <span class="p">{</span>
      <span class="k">display</span><span class="o">:</span> <span class="n">table</span><span class="p">;</span>
      <span class="k">content</span><span class="o">:</span> <span class="s2">""</span><span class="p">;</span>
    <span class="p">}</span>
    <span class="nc">.clearfix</span><span class="nd">:after</span> <span class="p">{</span>
      <span class="k">clear</span><span class="o">:</span> <span class="k">both</span><span class="p">;</span>
    <span class="p">}</span>
    

Alternatively, specify `overflow`, with `auto` or `hidden`, on the parent.

    <span class="nc">.parent</span> <span class="p">{</span>
      <span class="k">overflow</span><span class="o">:</span> <span class="k">auto</span><span class="p">;</span> <span class="c">/* clearfix */</span>
    <span class="p">}</span>
    <span class="nc">.other-parent</span> <span class="p">{</span>
      <span class="k">overflow</span><span class="o">:</span> <span class="k">hidden</span><span class="p">;</span> <span class="c">/* clearfix */</span>
    <span class="p">}</span>
    

Be aware that `overflow` can cause other unintended side effects, typically
around positioned elements within the parent.

**Pro-Tip!** *Keep your future self and your coworkers happy by including a
comment like`/* clearfix */` when clearing floats as the property can be used
for other reasons.*

### Floats and computed height

A parent element that has only floated content will have a computed 
`height: 0;`. Add a clearfix to the parent to force browsers to compute a
height.

### Floated elements are block level

Elements with a `float` will automatically become `display: block;`. Do not set
both as there is no need and the`float` will override your `display`.

    <span class="nc">.element</span> <span class="p">{</span>
      <span class="k">float</span><span class="o">:</span> <span class="k">left</span><span class="p">;</span>
      <span class="k">display</span><span class="o">:</span> <span class="k">block</span><span class="p">;</span> <span class="c">/* Not necessary */</span>
    <span class="p">}</span>
    

**Fun fact:** *Years ago, we *had* to set `display: inline;` for most floats to
work properly in IE6 to avoid the[double margin bug][4]. However, those days
have long passed.
*

### Vertically adjacent margins collapse

Top and bottom margins on adjacent elements (one after the other) can and will
collapse in many situations, but never for floated or absolutely positioned 
elements.[Read this MDN article][5] or the CSS2 spec's 
[collapsing margin section][6] to find out more.

Horizontally adjacent margins will **never collapse**.

### Styling table rows

Table rows, `<tr>`s, do not receive `border`s unless you set 
`border-collapse: collapse;` on the parent `<table>`. Moreover, if the 
`<tr>` and children `<td>`s or `<th>`s have the *same* 
`border-width`, the rows will not see their border applied. 
[See this JS Bin link for an example.][7]

### Firefox and `<input>` buttons

For reasons unknown, Firefox applies a `line-height` to submit and button 
`<input>`s that cannot be overridden with custom CSS. You have two
options in dealing with this:

1.  Stick to `<button>` elements
2.  Don't use `line-height` on your buttons

Should you go with the first route (and I recommend this one anyway because 
`<button>`s are great), here's what you need to know:

    <span class="c"><!-- Not so good --></span>
    <span class="nt"><input</span> <span class="na">type=</span><span class="s">"submit"</span> <span class="na">value=</span><span class="s">"Save changes"</span><span class="nt">></span>
    <span class="nt"><input</span> <span class="na">type=</span><span class="s">"button"</span> <span class="na">value=</span><span class="s">"Cancel"</span><span class="nt">></span>
    
    <span class="c"><!-- Super good everywhere --></span>
    <span class="nt"><button</span> <span class="na">type=</span><span class="s">"submit"</span><span class="nt">></span>Save changes<span class="nt"></button></span>
    <span class="nt"><button</span> <span class="na">type=</span><span class="s">"button"</span><span class="nt">></span>Cancel<span class="nt"></button></span>
    

Should you wish to go the second route, just don't set a `line-height` and use*
only* `padding` to vertically align button text. [View this JS Bin example][8]
in Firefox to see the original problem and the workaround.

**Good news!** *It looks like [a fix for this][9] might be coming in Firefox 30
. That's good news for our future selves, but be aware this doesn't fix older 
versions.*

### Firefox inner outline on buttons

Firefox [adds an inner outline][10] to buttons (`<input>`s and 
`<button>`s) on `:focus`. Apparently it's for accessibility, but its
placement seems rather odd. Use this CSS to override it:

    <span class="nt">input</span><span class="o">:</span><span class="nd">:-moz-focus-inner</span><span class="o">,</span>
    <span class="nt">button</span><span class="o">:</span><span class="nd">:-moz-focus-inner</span> <span class="p">{</span>
      <span class="k">padding</span><span class="o">:</span> <span class="m">0</span><span class="p">;</span>
      <span class="k">border</span><span class="o">:</span> <span class="m">0</span><span class="p">;</span>
    <span class="p">}</span>
    

You can see this fix in action in the same [JS Bin example][8] mentioned in the
previous section.

**Pro-Tip!** *Be sure to include some focus state on buttons, links, and inputs
. Providing an affordance for accessibility is paramount, both for pro users who
tab through content and those with vision impairments.*

### Always set a `type` on `<button>`s

The default value is `submit`, meaning any button in a form can submit the form
. Use`type="button"` for anything that doesn't submit the form and 
`type="submit"` for those that do.

    <span class="nt"><button</span> <span class="na">type=</span><span class="s">"submit"</span><span class="nt">></span>Save changes<span class="nt"></button></span>
    <span class="nt"><button</span> <span class="na">type=</span><span class="s">"button"</span><span class="nt">></span>Cancel<span class="nt"></button></span>
    

For actions that require a `<button>` and are not in a form, use the 
`type="button"`.

    <span class="nt"><button</span> <span class="na">class=</span><span class="s">"dismiss"</span> <span class="na">type=</span><span class="s">"button"</span><span class="nt">></span>x<span class="nt"></button></span>
    

**Fun fact:** *Apparently IE7 doesn't properly support the `value` attribute on
`<button>`s. Instead of reading the attribute's content, it pulls from
the innerHTML (the content between the opening and closing`<button>` tags
). However, I don't see this as a huge concern for two reasons: IE7 usage is way
down, and it seems rather uncommon to set both a`value` and the innerHTML on 
`<button>`s.*

### Internet Explorer's selector limit

Internet Explorer 9 and below have a max of 4,096 selectors per stylesheet.
There is also a limit of 31 combined stylesheets and
`<style></style>` includes per page. Anything after this limit is
ignored by the browser. Either split your CSS up, or start refactoring. I'd 
suggest the latter.

As a helpful side note, here's how browsers count selectors:

    <span class="c">/* One selector */</span>
    <span class="nc">.element</span> <span class="p">{</span> <span class="p">}</span>
    
    <span class="c">/* Two more selectors */</span>
    <span class="nc">.element</span><span class="o">,</span>
    <span class="nc">.other-element</span> <span class="p">{</span> <span class="p">}</span>
    
    <span class="c">/* Three more selectors */</span>
    <span class="nt">input</span><span class="o">[</span><span class="nt">type</span><span class="o">=</span><span class="s2">"text"</span><span class="o">],</span>
    <span class="nc">.form-control</span><span class="o">,</span>
    <span class="nc">.form-group</span> <span class="o">></span> <span class="nt">input</span> <span class="p">{</span> <span class="p">}</span>
    

### Position explained

Elements with `position: fixed;` are placed relative to the browser viewport.
Elements with`position: absolute;` are placed relative to their closest parent
with a position other than`static` (e.g., `relative`, `absolute`, or `fixed`

### Position and width

Don't set `width: 100%;` on an element that has `position: [absolute|fixed];`
`left`, and `right`. The use of `width: 100%;` is the same as the combined use
of`left: 0;` and `right: 0;`. Use one or the other, but not both.

### Fixed position and transforms

Browsers break `position: fixed;` when an element's parent has a `transform`
set. Using transforms creates a new containing block, effectively forcing the 
parent to have`position: relative;` and the fixed element to behave as 
`position: absolute;`.

[See the demo][11] and read [Eric Meyer's post on the matter][12].

 [1]: http://quirks.spec.whatwg.org
 [2]: http://www.paulirish.com/2012/box-sizing-border-box-ftw/
 [3]: http://nicolasgallagher.com/micro-clearfix-hack/
 [4]: http://www.positioniseverything.net/explorer/doubled-margin.html
 [5]: https://developer.mozilla.org/en-US/docs/Web/CSS/margin_collapsing
 [6]: http://www.w3.org/TR/CSS2/box.html#collapsing-margins
 [7]: http://jsbin.com/yabek/2/
 [8]: http://jsbin.com/yabek/4/
 [9]: https://bugzilla.mozilla.org/show_bug.cgi?id=697451#c43
 [10]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button
 [11]: http://jsbin.com/yabek/1/

 [12]: http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/