# How to add an anchor navigation to your template (v0.1)

##About this recipe
If your are building websites with content blocks by modmore, it is very easy to add an anchor navigation to it.

In this example we will build an anchor navigation with links to all headlines included in your content.

## Add a new propery to your heading template
Go to settings and add a new property **alias** to your template. From now on it will be available as ```[[+alias]]``` inside the heading template.

Each headline will now have a new field where you can add an alias.

## Add id="[[+alias]]" you heading template
Each link of your anchornav needs a target with a unique id attribute. The default template for headlines doesn't have an id attribute so we have to add it to it.

###Template
```
<[[+level]] class="heading" id="[[+alias]]">[[+value]]</[[+level]]>
```

## Template your anchornav
Templating is very easy. You write a chunk for used for displaying each anchorlink and a wrapper where all the chunks will be filled in.

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
[[!cbGetFieldContent?
   &field=`1`
   &resource=`[[*id]]`
   &tpl=`contentblocks_anchornav_tpl`
   &wrapTpl=`contentblocks_wrapTpl`
   &fieldSettingsFilter=`alias!=`
]]
```
Note If you are using multiple levels of headlines, you'll propably want to limit your anchornav to a certain level (for example h3). You can do this by modifying your fieldSettingsFilter property:

``` &fieldSettingsFilter=`alias!=,level==h3` ```


## Conclusion
Now you have an anchornav to all headlines on the current page that have an alias.
