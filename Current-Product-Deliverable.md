# MediaWiki Accessibility Product Deliverable
### Version: 1.0.0

**The Blue Wall Group:**
    Jesse Buck
    Jaziel Pauda
    Michael Cornacchio
    Daniel Collier
    Hunter Hobbs

Date: 03/08/2018

**Abstract:** This document is meant to organize all contributions and enhancements made to the
MediaWiki Accessibility HFOSS project. Bellow, you will find a list of issues and defects that our team worked on throughout the Spring 18’ semester in CS4260 – Software Engineering Practices. For each issue, there is listed a description with a link to the issue on the MediaWiki Accessibility work board – Phabricator, a link to the patch submission review site (Gerrit) where a maintainer reviews, resubmissions, continuous integration test results and merges can be viewed. Code differences between patch versions or the main trunk can be viewed from the Gerrit web interface as well. If a patch submission was accepted and merged, then a link to the commit history for the MediaWiki trunk will also be provided. Contributing to this open source software project was part of our senior capstone project at Metropolitan State University of Denver.

***

### Issue T161612 - Buttons in MMV are not really buttons and are thus not semantic

_Description:_ This issue focused on a MediaWiki extensions called Multimedia Viewer. Some of the buttons that are part of the multimedia viewer were tagged in the HTML as `<div>` or `<a>` elements. The result of this is that if the button does not have a label attribute, then a screen reader does not have any textual information to provide content to the button for the user.

_Work performed:_ We went through each JavaScript file in the Multimedia Viewer directory and looked for button elements that were tagged improperly. In some cases, the `<a>` tags were able to just be replaced with `<button>` tags. In the case of the `copyButton`, we added an `aria-label` attribute with a value of “copy text” that was output via the MW-messages API.  We needed to fix some of the button styling in the associated CSS files as well.

This issue is not completely resolved yet. We are currently working on patchset 5. The history of our changes and patchsets, as well as the maintainer review and continuous integration test results can be viewed on the Gerrit web interface.

_Changes (outdated):_

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

_Description:_ Within the MediaWiki Flow extension, several `<div>` elements needed to be changed into `<article>` elements in order to better follow semantic HTML conventions, specifically W3 and MDN html standards.

_Work Performed:_ The work started with verifying the issue author was correct that the `<div>` tags specified should become `<article>` tags by going over W3 and MDN standards. After this, the `<div>` tags that needed to be changed were identified by searching for the tags that had the CSS classes "flow-post-content" and "mw-parser-output". Then, the identified tags were changed to `<article>` tags, templates were compiled, and finally the changes were tested using PHPUnit.

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

***

### Issue T156450 - QuizGame Special:QuestionGameHome should order headings correctly for semantics and accessibility

_Description:_ In the QuizGame extension, several `<h1>` elements were present on a single page of the extension. For accessibility and semantics reasons, all of these `<h1>` tags needed to be changed into `<h2>`'s.

_Work Performed:_ The work started with determining if having multiple `<h2>` tags in one page was appropriate. After this, the specified heading tags were changed to `<h2>` tags and CSS classes with selectors for the `<h1>` tags were removed from the code base.

_Changes:_

* SpecialQuizGameHome.php:
```diff
-   <h1>' . $this->msg( 'quizgame-admin-flagged' )->text() . "</h1>
+   <h2>' . $this->msg( 'quizgame-admin-flagged' )->text() . "</h2>
    ⋮
-   <h1>" . $this->msg( 'quizgame-admin-protected' )->text() . "</h1>
+   <h2>" . $this->msg( 'quizgame-admin-protected' )->text() . "</h2>
    ⋮
-   <h1>' . $this->msg( 'quizgame-submitted-by' )->text() . "</h1>
+   <h2>' . $this->msg( 'quizgame-submitted-by' )->text() . "</h2>
    ⋮
-   <h1>" . $this->msg( 'quizgame-question' )->text() . "</h1>
+   <h2>" . $this->msg( 'quizgame-question' )->text() . "</h2>
    ⋮
-   <h1>" . $this->msg( 'quizgame-answers' )->text() . "</h1>
+   <h2>" . $this->msg( 'quizgame-answers' )->text() . "</h2>
    ⋮
-   <h1>" . $this->msg( 'quizgame-picture' )->text() . "</h1>
+   <h2>" . $this->msg( 'quizgame-picture' )->text() . "</h2>
    ⋮
-   <h1>" . $this->msg( 'quizgame-submitted-by' )->text() . "</h1>
+   <h2>" . $this->msg( 'quizgame-submitted-by' )->text() . "</h2>
    ⋮
-   <h1>' . $this->msg( 'quizgame-create-write-question' )->text() . '</h1>
+   <h2>' . $this->msg( 'quizgame-create-write-question' )->text() . '</h2>
    ⋮
-   <h1 class="write-answer">' . $this->msg( 'quizgame-create-write-answers' )->text() . '</h1>
+   <h2 class="write-answer">' . $this->msg( 'quizgame-create-write-answers' )->text() . '</h2>
    ⋮
-   <h1 style="margin-top:20px">' .
-       $this->msg( 'quizgame-create-add-picture' )->text() . '</h1>
+   <h2 style="margin-top:20px">' .
+       $this->msg( 'quizgame-create-add-picture' )->text() . '</h2>
```

* questiongame.css:
```diff
-   .quizgame-edit-question h1 {
-       font-size: 16px;
-       font-weight: bold;
-       border-bottom: none;
-       color: #333;
-       margin: 20px 0 10px 0 !important;
-   }
    ⋮
-   .quizgame-admin h1 {
-       font-size: 18px;
-       color: #333;
-       font-weight: bold;
-       margin: 0 0 20px 0 !important;
-   }
    ⋮
-   .quizgame-flagged-item h1 {
-       font-size: 14px;
-       font-weight: bold;
-       color: #333;
-       margin: 0 0 10px 0 !important;
-   }
    ⋮
-   .credit-box h1 {
-       border-bottom: none;
-       color: #333;
-       font-size: 16px;
-       font-weight: bold;
-       padding: 0;
-       margin: 0 0 10px !important;
-   }
    ⋮
-   .create-message h1 {
-       font-size: 22px;
-       border-bottom: none;
-       font-weight: bold;
-       margin: 0 0 10px !important;
-   }
-
-   .quizgame-create-form h1 {
-       color: #333;
-       font-size: 16px;
-       font-weight: bold;
-       border-bottom: none;
-       margin: 20px 0 15px 0 !important;
-   }
-
-   h1.write-answer {
-       color: #333;
-       font-size: 16px;
-       margin: 20px 0 10px 0 !important;
-   }
```


[Issue T156450 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T156450)

[Issue T156450 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/416887/)

***

### Issue T185533 - Wikistats Beta: Fix accessibility/markup issues of Wikistats 2.0 (Part 1: .subdued classes and footer area)

_Description:_ 

_Work Performed:_ Changed several elements to use colors from the WikimediaUI Base color palette. Changed text using the "subdued" class, the color of text and links within the footer area, and the background color of the footer area. Although not explicitly mentioned in T185533, the metrics buttons within the Detail view was also changed.

_Changes:_
* App.vue:
```diff
.ui.attached.footer.segment {
-   background-color: #3B3B3B;
-   color: #AAAAAA;
+   background-color: #54595d;
+   color: #fff;
}
```

* BottomFooter.vue:
```diff
-   a, a:visited { color: #888; }
+   a, a:visited { color: #eaf3ff; }
```

* MetricWidget.vue:
```diff
    .subdued {
-       color: #9b9b9b;
+       color: #72777d;
    }
```

* Detail.vue:
```diff
    ui.line.label {
-       color: #9b9b9b!important;
+       color: #54595d!important;
    }
```

* GraphPanel.vue:
```diff
    .graph.panel h2.header .subdued {
-       color: #777;
+       color: #72777d;
    }
```

***

### Issue T185533 - Wikistats Beta: Fix accessibility/markup issues of Wikistats 2.0 (Part 2: Search placeholder)

_Description:_ The WikiStats 2.0 search placeholder did not comply with WCAG AA contrast requirements.  Additionally, the MediaWiki reviewers advocated for the use of their stylelinter.  The stylelinter identified numerous issues that required adjustment.   

_Work Performed:_ Replaced "4 space" indentation with tab indentation as identified by the stylelinter.  Used class hierarchy to identify placeholder rather than unique identifier.  As well as various other slight formatting changes.       

_Changes:_

* TopicExplorer.vue
```diff
@@ -18,7 +18,7 @@
 
         <div class="ui search">
             <div class="ui icon input">
-                <input class="prompt" type="text" v-model="searchDisplay"
+                <input class="prompt" type="search" v-model="searchDisplay"
                     placeholder="Search or Browse questions and pick one to see answers"
                     @blur="onBlur"
                     @keyup.enter="select"
@@ -122,105 +122,122 @@
 
 <style scoped>
 .animateable {
-    transition: all .4s;
+	transition: all 0.4s;
 }
 .slide.transition.container {
-    position: relative;
-    height: 43px;
-    margin: -2px -32px 0 -24px;
+	position: relative;
+	height: 43px;
+	margin: -2px -32px 0 -24px;
 }
 .slide.transition.animateable.container.down {
-    height: 87px;
+	height: 87px;
 }
 .slide.transition.container > div {
-    position: absolute;
+	position: absolute;
 }
 .animateable.topic.searcher {
-    top: -84px;
+	top: -84px;
 }
 .animateable.topic.searcher.down {
-    top: 0;
+	top: 0;
 }
 
-.xui.grey.corner.button, .ui.grey.inverted.segment {
-    background-color: #72777d!important;
+.xui.grey.corner.button,
+.ui.grey.inverted.segment {
+	background-color: #72777d !important;
 }
 .xui.corner.button {
-    display: inline-block;
-    text-align: center!important;
-    line-height: 40px;
-    margin: 0;
-    width: 163px;
-    height: 40px;
-    border-radius: 0 0 3px 0;
-    background-color: #72777d;
-    cursor: pointer;
+	display: inline-block;
+	text-align: center !important;
+	line-height: 40px;
+	margin: 0;
+	width: 163px;
+	height: 40px;
+	border-radius: 0 0 3px 0;
+	background-color: #72777d;
+	cursor: pointer;
 
-    font-family: Lato;
-    font-size: 16px;
-    font-weight: 900;
-    text-align: left;
-    color: #ffffff;
+	font-family: 'Lato';
+	font-size: 16px;
+	font-weight: 900;
+	text-align: left;
+	color: #fff;
 }
 .xui.link {
-    cursor: pointer;
+	cursor: pointer;
 }
 .ui.inverted.segment {
-    width: 100%;
-    height: 84px;
-    padding: 20px 30px;
-    margin: 0;
-    border-radius: 0;
-    font-size: 16px;
-    font-weight: 900;
-    color: #ffffff;
+	width: 100%;
+	height: 84px;
+	padding: 20px 30px;
+	margin: 0;
+	border-radius: 0;
+	font-size: 16px;
+	font-weight: 900;
+	color: #fff;
+}
+
+.ui.search .ui.input input::-webkit-input-placeholder {
+	color: #555;
+}
+
+.ui.search .ui.input input::-ms-input-placeholder {
+	color: #555;
+}
+
+.ui.search .ui.input input::-moz-placeholder {
+	color: #555;
+	opacity: 1;
 }
 
 .ui.search {
-    display: inline-block;
-    width: 79%;
-    margin-left: 10px;
-    margin-right: 6px;
+	display: inline-block;
+	width: 79%;
+	margin-left: 10px;
+	margin-right: 6px;
 }
 .ui.search .ui.input {
-    width: 100%;
+	width: 100%;
 }
 .ui.inverted.segment .ui.search .ui.input input {
-    height: 35px;
-    border-radius: 0.28571429rem;
+	height: 35px;
+	border-radius: 0.28571429rem;
 }
 
-.dropdown.button { width: 70%; margin-left: 10px; }
+.dropdown.button {
+	width: 70%;
+	margin-left: 10px;
+}
 .ui.blue.button {
-    background-color: #3366cc!important;
-    width: 78px;
+	background-color: #36c !important;
+	width: 78px;
 }
 
-@media(max-width: 450px) {
-    .xui.grey.corner.button {
-        width: 100vw;
-        margin-left: -1px;
-        border-radius: 0;
-    }
-    .slide.transition.container {
-        margin: 0;
-    }
-    .xui.link {
-        margin: 0;
-        padding: 0;
-    }
+@media ( max-width: 450px ) {
+	.xui.grey.corner.button {
+		width: 100vw;
+		margin-left: -1px;
+		border-radius: 0;
+	}
+	.slide.transition.container {
+		margin: 0;
+	}
+	.xui.link {
+		margin: 0;
+		padding: 0;
+	}
 }
 
-@media(max-width: 1000px) {
-    .ui.segment.topic.searcher {
-        padding: 10px;
-    }
-    .xui.link {
-        display: inline-block;
-        margin-bottom: 10px;
-    }
-    .topic.searcher .ui.search {
-        width: 97%;
-    }
+@media ( max-width: 1000px ) {
+	.ui.segment.topic.searcher {
+		padding: 10px;
+	}
+	.xui.link {
+		display: inline-block;
+		margin-bottom: 10px;
+	}
+	.topic.searcher .ui.search {
+		width: 97%;
+	}
 }
 </style>
```

***

### Issue T185533 - Wikistats Beta: Fix accessibility/markup issues of Wikistats 2.0 (Part 3: Remaining Concerns)

_Description:_  A number of accessibility/markup issues were identified in the WikiStats2 extension beyond those already mentioned.

_Work Performed:_ 
* Added lang attribute to html element.  
* Added labels to topic search input and Wiki search input.
* Added alt tag for Wikimedia logo.
* Added main ARIA label in App.vue.
* Changed search header to a div.       

_Changes:_

* TopNav.vue:
```diff
@@ -2,7 +2,7 @@
 <div>
     <router-link :to="{project: $store.state.project}">
         <div class="wikititle ui left floated header">
-            <img class="ui mini image" src="../../assets/Wikimedia-logo.svg">
+            <img class="ui mini image" alt="Wikimedia logo" src="../../assets/Wikimedia-logo.svg">
             <span class="text">Wikimedia Statistics</span>
         </div>
     </router-link>
```

* TopicExplorer.vue:
```diff
@@ -18,7 +18,8 @@
 
         <div class="ui search">
             <div class="ui icon input">
-                <input class="prompt" type="text" v-model="searchDisplay"
+				<label for="topicSearch">Search for a Question</label>
+                <input id="topicSearch" class="prompt" type="text" v-model="searchDisplay"
                     placeholder="Search or Browse questions and pick one to see answers"
                     @blur="onBlur"
                     @keyup.enter="select"
@@ -223,4 +224,9 @@
         width: 97%;
     }
 }
+
+label[ for='topicSearch' ]
+{
+	display: none;
+}
 </style>
```

* WikiSelector.vue:
```diff
@@ -2,7 +2,8 @@
 <div>
     <div class="ui search">
         <div class="ui icon input">
-            <input class="prompt" type="text" v-model="inputText"
+			<label for="wikiSearch">Search for a Wiki</label>
+            <input id="wikiSearch" class="prompt" type="text" v-model="inputText"
                 ref="inputBox"
                 :placeholder="placeholder"
                 @click="open"
@@ -344,4 +345,9 @@
         margin-left: 1em;
     }
 }
+
+label[ for='wikiSearch' ]
+{
+	display: none;
+}
 </style>
```

* Dashboard.vue:
```diff
@@ -1,10 +1,10 @@
 <template>
-<section class="widgets">
+<section class="widgets" role="main">
     <div class="ui clearing basic top segment">
         <h2 class="ui left floated header">Monthly overview</h2>
-        <h5 class="ui right floated header wikiselector">
+        <div class="ui right floated header wikiselector">
             <wiki-selector :single="true"></wiki-selector>
-        </h5>
+        </div>
     </div>
     <div class="ui basic area segment"
         v-for="a in areas"
```

* index.html:
```diff
@@ -1,5 +1,5 @@
 <!DOCTYPE html>
-<html>
+<html lang="en">
     <head>
         <meta charset="utf-8">
         <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
```