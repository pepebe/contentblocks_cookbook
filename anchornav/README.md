# How to add an anchor navigation to your template (v0.1) with contentblocks by modmore

##About this recipe
If your are building websites with content blocks (cb) by modmore, it is very easy to add an anchor navigation to it.

In this example we will build an anchor navigation with links to all headlines included in your content.

## Craete a new cb template

First go to settings and add a new property **alias** to your template. From now on it will be available as ```[[+alias]]``` inside the heading template.

Next, duplicate the heading template and give it a new name (For example: Anchor).

## Add id="[[+alias]]" to your new template
Each link of your anchornav needs a target with a unique id attribute. The default template for headlines doesn't have an id attribute so we have to add it to it.

###Template: heading
The exact html of your headline depends on your personal needs. If you want to be flexible, just add the id attribute to the template:
```
<[[+level]] class="heading" id="[[+alias]]">[[+value]]</[[+level]]>
```
If you think that only headlines of a level 3 (h3) should be available, remove the level placeholder and use h3 instead. 
```
<h3 class="heading" id="[[+alias]]">[[+value]]</h3>
```
As the level property doesn't make sense in this context, you can remove level from the property list for this template.

## Template your anchornav
Templating is very easy. You write a chunk used for displaying each anchorlink and a wrapper where all the chunks will be filled in.

### Chunk: contentblocks_anchornav_tpl
```
<!-- contentblocks_anchornav_tpl --> 
 <li>
    <a href="[[++site_url]][[~[[*id]]]]#[[+alias]]">
        [[+value]]
    </a>
</li>
```

###Chunk: contentblocks_anchornav_wrapTpl
```
<!-- contentblocks_anchornav_wrapTpl -->
<ul class="nav navbar">
[[+output]]
</ul> 
```

## Add a snippet call to your template
Now all we have to do is to add the **cbGetFieldContent** snippet to your template. It will grab all necessary data from your content and create a simple anchornar based on your templates.

###Snippet: cbGetFieldContent
```
<!-- cbGetFieldContent -->
[[cbGetFieldContent?
   &field=`1`
   &resource=`[[*id]]`
   &tpl=`contentblocks_anchornav_tpl`
   &wrapTpl=`contentblocks_wrapTpl`
   &fieldSettingsFilter=`alias!=`
]]
```
**Note:** If you are using multiple levels of headlines, you'll propably want to limit the items in your your anchornav to a certain level (for example h3). You can do this by modifying your fieldSettingsFilter property:

``` &fieldSettingsFilter=`alias!=,level==h3` ```


## Conclusion
Now you have an anchornav to all headlines on the current page that have an alias.
