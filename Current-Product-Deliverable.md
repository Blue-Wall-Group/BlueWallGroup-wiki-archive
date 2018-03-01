# MediaWiki Accessibility Product Deliverable
### Version: 1.0.0

**The Blue Wall Group:**  
    Jesse Buck  
    Jaziel Pauda  
    Michael Cornacchio  
    Daniel Collier  
    Hunter Hobbs  

Date: 03/01/2018

**Abstract:** This document is meant to organize all contributions and enhancements made to the
MediaWiki Accessibility HFOSS project. Bellow, you will find a list of issues and defects that our team worked on throughout the Spring 17’ semester in CS4260 – Software Engineering Practices. For each issue, there is listed a description with a link to the issue on the MediaWiki Accessibility work board – Phabricator, a link to the patch submission review site (Gerrit) where a maintainer reviews, resubmissions, continuous integration test results and merges can be viewed. Code differences between patch versions or the main trunk can be viewed from the Gerrit web interface as well. If a patch submission was accepted and merged, then a link to the commit history for the MediaWiki trunk will also be provided. Contributing to this open source software project was part of our senior capstone project at Metropolitan State University of Denver.

***

### Issue T161612 - Buttons in MMV are not really buttons and are thus not semantic

_Description:_ This issue focused on a MediaWiki extensions called Multimedia Viewer. Some of the buttons that are part of the multimedia viewer were tagged in the HTML as `<div>` or `<a>` elements. The result of this is that if the button does not have a label attribute, then a screen reader does not have any textual information to provide content to the button for the user.

_Work performed:_ We went through each JavaScript file in the Multimedia Viewer directory and looked for button elements that were tagged improperly. In some cases, the `<a>` tags were able to just be replaced with `<button>` tags. In the case of the `copyButton`, we added an `aria-label` attribute with a value of “copy text” that was output via the MW-messages API.  We needed to fix some of the button styling in the associated CSS files as well. 

This issue is not completely resolved yet. We are currently working on patchset 5. The history of our changes and patchsets, as well as the maintainer review and continuous integration test results can be view on the Gerrit web interface.

_Changes:_

* mmv.ui.download.pane.js:
```diff 
-   this.$attributionCopy = this.$copyButton = $( '<a>' )
+   this.$attributionCopy = this.$copyButton = $( '<button>' )
+       .attr( 'aria-label', mw.msg( 'multimediaviewer-download-attribution-copy' ) )
```

* mmv.ui.permission.js:
```diff 
-   this.$close = $( '<div>' )
+   this.$close = $( '<button>' )
```

* mmv.ui.reuse.embed.js:
```diff 
-   this.$copyButton = $( '<a>' )
+   this.$copyButton = $( '<button>' )
+       .attr( 'aria-label', mw.msg( +'multimediaviewer-reuse-copy-embed' ) )
```

* mmv.ui.reuse.embed.less:
```diff 
-   width: 1em;
-   height: 1em;
-   margin: 10px 0.5em 20px 0;
-   padding: 0 0 0 5px;
+   width: 1.5em;
+   height: 1.5em;
+   margin: 10px 0.7em 20px 0.7em;
+   border: 0;
+   background-color: transparent;
```

* mmv.ui.reuse.share.js:
```diff 
-    this.$copyButton = $( '<a>' )
+    this.$copyButton = $( '<button>' )
+       .attr( 'aria-label', mw.msg( 'multimediaviewer-reuse-copy-share' ) )
```

* mmv.ui.reuse.share.less:
```diff 
-   width: 1.5em;
-   height: 1.5em;
+   width: 2em;
+   height: 2em;
-   margin: 8px 0.5em 8px 0;
-   padding: 0 0 0 5px;
+   margin: 8px 0.5em 8px 0.5em;
+   border: 0;
+   background-color: transparent;
```

* mmv.ui.stripeButtons.js:
```diff 
-   .prependTo( this.$buttonContainer );
+   .prependTo( this.$buttonContainer )
+   .attr( 'role', 'button' )
+   .attr( 'tabindex', '0' );
```

[Issue T161612 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T161612)

[Issue T161612 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/408577/)

***

### Issue T175937 - Flow could use article tags

_Description:_ Within the MediaWiki Flow extension, several `<div>` elements needed to be changed into `<article>` elements in order to better follow semantic HTML conventions.

_Work Performed:_

_Changes:_

* flow_block_topic_moderate_post.handlebars.php:
```diff
-   '.$sp.' <div class="flow-post-content mw-parser-output">
+   '.$sp.' <article class="flow-post-content mw-parser-output">
    ⋮
-   '.$sp.' </div>
+   '.$sp.' </article>
```

* flow_block_topic_moderate_topic.handlebars.php:
```diff
-   '.$sp.'    <div class="flow-post-content mw-parser-output">
+   '.$sp.'    <article class="flow-post-content mw-parser-output">
    ⋮
-   '.$sp.'    </div>
+   '.$sp.'    </article>
```

* flow_post.handlebars.php:
```diff
-   '.$sp.'    <div class="flow-post-content mw-parser-output">
+   '.$sp.'    <article class="flow-post-content mw-parser-output">
    ⋮
-   '.$sp.'    </div>
+   '.$sp.'    </article>
```

* flow_post_inner.partial.handlebars:
```diff
-   <div class="flow-post-content mw-parser-output">
+   <article class="flow-post-content mw-parser-output">
    ⋮
-   </div>
+   </article>
```

[Issue T175937 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T175937)

[Issue T175937 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/413091/)

[Issue T175937 MediaWiki Phabricator merged commit](https://phabricator.wikimedia.org/rEFLWa703662e1c62c251ad1e83bf0b84715dfb5ff437)
