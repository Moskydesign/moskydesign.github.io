When you have a site structure where nodes are categorized using taxonomy terms, and those terms have a tree like structure, parrent &gt; child you may want to set a path alias pattern for a node that reflect the structure.

You can do this of course  with the <a href="https://www.drupal.org/project/pathauto">Pathauto module</a>.
On the pattern configuration page (admin/config/search/path/patterns) add a new pattern, select a type and give it a name.

Under Path pattern you can define the pattern you want. Use the token browser to first add the name of your current node <code class="EnlighterJSRAW" data-enlighter-language="generic">node:title</code>.
<h2>Term name</h2>
Then find the field that holds the reference to the taxonomy term you want to use.
Select the name token of this vocabulary, <code class="EnlighterJSRAW" data-enlighter-language="generic">node:term_field_name:entity:name</code>. This will add the term name to your path.
<h2>Adding term parents</h2>
Navigate through the token browser again until you see the 'parents ' selector for that field, it will look something like this <code class="EnlighterJSRAW" data-enlighter-language="generic">[node:term_field_name:entity:parents]</code>. Also add this to your path alias. This will add the parents of your current term to the alias.

The output of all this will be parent-term1-parent-term2/term/node-name.
<h2>Even more structure</h2>
This reflects the structure of the site but would it be nicer if it would output like this <code class="EnlighterJSRAW" data-enlighter-language="generic">parent-term1/parent-term2/term/node-name</code>?

To accomplish this go back to your pattern and append <code class="EnlighterJSRAW" data-enlighter-language="generic">:join-path</code> to the parents token so it ends up looks like <code class="EnlighterJSRAW" data-enlighter-language="generic">[node:term_field_name:entity:parents:join-path]</code>.

The full pattern will be <code class="EnlighterJSRAW" data-enlighter-language="generic">[node:term_field_name:entity:parents:join-path]/[node:term_field_name:entity:name]/[node:title]</code>.