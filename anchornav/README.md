# How to add an anchor navigation to your template (v0.1) with contentblocks by modmore

##About this recipe
If your are building websites with content blocks (cb) by modmore, it is very easy to add an anchor navigation to it.

In this example we will build an anchor navigation with links to all headlines included in your content.

## Create a new content block field type

1. Open your "Content Blocks" CMP under "Extras". 
2. Duplicate the "Heading" template. Give it a new name (for example: "Anchor").
3. Now edit your new field type. 
###General: 
Add a meaningful description.

###Properties: 
Change the headling template to include the id attribute. Use the "alias" placeholder to give it a value.
```
<[[+level]] class="heading" id="[[+alias]]">[[+value]]</[[+level]]>
```

###Availability
No changes needed here.

###Settings
Each link of your anchornav needs a target with a unique id attribute. The default template for headlines doesn't have an id attribute so we have to add it to it. 

Because naming the new field "id" might cause some problems, we'll call this field "alias" instead.

Add a new property to your anchor headline:
* Reference: "alias"
* Title: "Alias"
* Field Type: "Text"
* Default value: "Empty"
* Expose field: Whatever you prefer.

From now on alias will be available while editing an anchor.

## Build your anchor navigation
Now all we have to do is to add the **cbGetFieldContent** snippet to your template. It will grab all necessary data from your content and create a simple anchornar based on your templates.

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

**fields:** The field paramter must contain the id of your new field. As ids are hidden from view in the list of fields, you have to switch them on. Move your mouse over the field label. Right-cloick on the arrow that appears on the right side. Hover over "Columns" and then activate the id column.
**tpl/wrapTpl:** Templating cbgetFieldContent is very easy. You write a chunk used for displaying each anchorlink (tpl) and a wrapper where all the chunks will be filled in (wrapTpl).

```
<!-- contentblocks_anchornav_tpl --> 
 <li>
    <a href="[[++site_url]][[~[[*id]]]]#[[+alias]]">
        [[+value]]
    </a>
</li>

<!-- contentblocks_anchornav_wrapTpl -->
<ul class="nav navbar">
[[+output]]
</ul> 
```
**fieldSettingsFilter:** You only want to add Anchors to your list that don't target empty alias fields. If you are using multiple levels of headlines, you'll propably want to limit the items in your your anchornav to a certain level (for example h3). You can do this by modifying your fieldSettingsFilter property:

``` &fieldSettingsFilter=`alias!=,level==h3` ```

## Conclusion
Now you have an anchornav to all headlines on the current page that have an alias.
