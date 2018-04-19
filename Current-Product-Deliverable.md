# MediaWiki Accessibility Product Deliverable

#### The Blue Wall Group:  
    Jesse Buck  
    Daniel Collier  
    Michael Cornacchio  
    Hunter Hobbs  
    Jaziel Pauda  

#### Abstract:
The following document is a summary of the improvements we (Blue Wall Group) have made to the MediaWiki free open source software project. Our improvements addressed accessibility concerns existing within the MediaWiki core and its extensions. Three of our contributions have been integrated into the current version of the MediaWiki software. Furthermore, we have contributed four additional changes that are awaiting acceptance from project reviewers. These contributions were part of our senior capstone project during the Spring 2018 semester at Metropolitan State University of Denver.               


#### Contents:
- [Issue T161612 - Buttons in MMV are not really buttons and are thus not semantic](#issue-t161612---buttons-in-mmv-are-not-really-buttons-and-are-thus-not-semantic)
- [Addition of label to input field in Visual Editor Categories page](#addition-of-label-to-input-field-in-visual-editor-categories-page)
- [Issue T175937 - Flow could use article tags](#issue-t175937---flow-could-use-article-tags)
- [Issue T156450 - QuizGame Special:QuestionGameHome should order headings correctly for semantics and accessibility](#issue-t156450---quizgame-specialquestiongamehome-should-order-headings-correctly-for-semantics-and-accessibility)
- [Issue T185533 - Wikistats Beta: Fix accessibility/markup issues of Wikistats 2.0 (Part 1: .subdued classes and footer area)](#issue-t185533---wikistats-beta-fix-accessibilitymarkup-issues-of-wikistats-20-part-1-subdued-classes-and-footer-area)
- [Issue T185533 - Wikistats Beta: Fix accessibility/markup issues of Wikistats 2.0 (Part 2: Search placeholder)](#issue-t185533---wikistats-beta-fix-accessibilitymarkup-issues-of-wikistats-20-part-2-search-placeholder)
- [Issue T185533 - Wikistats Beta: Fix accessibility/markup issues of Wikistats 2.0 (Part 3: Remaining Concerns)](#issue-t185533---wikistats-beta-fix-accessibilitymarkup-issues-of-wikistats-20-part-3-remaining-concerns)

***

### Issue T161612 - Buttons in MMV are not really buttons and are thus not semantic 

#### Contributors:
* Hunter Hobbs
* Jaziel Pauda
* Jesse Buck

#### Description:
Throughout the codebase of the MultiMedia Viewer extension, there are button-like interface elements that are tagged as `<div>` and `<a>` elements. This is not semantic and thus all button-like elements in MultiMedia Viewer need to be converted from `<div>` and `<a>` elements to `<button>` elements. As is, a screen reader will not receive any semantic content about the interactive element to provide to the user.

#### Work performed:
We thoroughly inspected the codebase for all interactive button-like elements that were improperly tagged. The `<div>` in `permission.js` was easily converted to a `<button>`. The `copyButton` in `download.pane.js` and `reuse.embed.js` needed a class (`bootstrap.less`) to offset some labeling text so that it wouldn't interfere with the UI for users without accessibility needs. A message library was used for the label language translation. Some of the associated CSS files needed to be edited to keep the button-like elements styling the same as it was prior to converting the tags. In addition to converting the elements, there was an `<a>`-button-like element in `StripeButtons.js` that was a link. This was not converted because it is a link and the rationale is that `<a>`-link elements are used to go from `page1` to `page2` while buttons are used as an interactive interface within the page. A `tabindex` attribute was added to the `StripeButton.js` `<a>` elements to make it focusable by the keyboard for accessibility needs.

#### Outcome:
This issue spanned nearly the whole semester. There was a total of 13 patchsets developed to resolve this issue. It was merged and deployed on April 16, 2018.

#### Changes:
* resources/mmv/mmv.bootstrap.less:
```diff
@@ -58,3 +58,12 @@
 		}
 	}
 }
+
+.mw-mmv-button {
+	background-color: transparent;
+	min-width: 0;
+	border: 0;
+	padding: 0;
+	overflow-x: hidden;
+	text-indent: -9999em;
+}
```

* resources/mmv/ui/mmv.ui.download.pane.js:
```diff
@@ -198,8 +198,8 @@
 			)
 			.appendTo( this.$attributionSection );
 		this.attributionInput = attributionInput;
-		this.$attributionCopy = this.$copyButton = $( '<a>' )
-			.addClass( 'mw-mmv-dialog-copy' )
+		this.$attributionCopy = this.$copyButton = $( '<button>' )
+			.addClass( 'mw-ui-button mw-mmv-button mw-mmv-dialog-copy' )
 			.click( function () {
 				// Select the text, and then try to copy the text.
 				// If the copy fails or is not supported, continue as if nothing had happened.
@@ -216,6 +216,7 @@
 				}
 			} )
 			.prop( 'title', mw.msg( 'multimediaviewer-download-attribution-copy' ) )
+			.text( mw.msg( 'multimediaviewer-download-attribution-copy' ) )
 			.tipsy( {
 				delayIn: mw.config.get( 'wgMultimediaViewer' ).tooltipDelay,
 				gravity: this.correctEW( 'se' )
```

* resources/mmv/ui/mmv.ui.permission.js:
```diff
@@ -96,7 +96,7 @@
 		 * "Close" button (does not actually close the box, just makes it smaller).
 		 * @property {jQuery}
 		 */
-		this.$close = $( '<div>' )
+		this.$close = $( '<button>' )
 			.addClass( 'mw-mmv-permission-close' )
 			.on( 'click', function () {
 				permission.shrink();
```

* resources/mmv/ui/mmv.ui.reuse.embed.js:
```diff
@@ -141,8 +141,8 @@
 			mw.mmv.actionLogger.log( 'embed-wikitext-copied' );
 		} );
 
-		this.$copyButton = $( '<a>' )
-			.addClass( 'mw-mmv-dialog-copy' )
+		this.$copyButton = $( '<button>' )
+			.addClass( 'mw-mmv-button mw-mmv-dialog-copy' )
 			.click( function () {
 				// Select the text, and then try to copy the text.
 				// If the copy fails or is not supported, continue as if nothing had happened.
@@ -159,6 +159,7 @@
 				}
 			} )
 			.prop( 'title', mw.msg( 'multimediaviewer-reuse-copy-embed' ) )
+			.text( mw.msg( 'multimediaviewer-reuse-copy-embed' ) )
 			.tipsy( {
 				delayIn: mw.config.get( 'wgMultimediaViewer' ).tooltipDelay,
 				gravity: this.correctEW( 'se' )
```

* resources/mmv/ui/mmv.ui.reuse.embed.less
```diff
@@ -37,10 +37,9 @@
 
 	.mw-mmv-dialog-copy {
 		float: right;
-		width: 1em;
-		height: 1em;
-		margin: 10px 0.5em 20px 0;
-		padding: 0 0 0 5px;
+		width: 1.5em;
+		height: 1.5em;
+		margin: 10px 0.75em 20px 0.75em;
 	}
 }
 ```

* resources/mmv/ui/mmv.ui.reuse.share.js:
```diff
@@ -67,8 +67,8 @@
 				mw.mmv.actionLogger.log( 'share-page' );
 			} );
 
-		this.$copyButton = $( '<a>' )
-			.addClass( 'mw-mmv-dialog-copy' )
+		this.$copyButton = $( '<button>' )
+			.addClass( 'mw-mmv-button mw-mmv-dialog-copy' )
 			.click( function () {
 				// Select the text, and then try to copy the text.
 				// If the copy fails or is not supported, continue as if nothing had happened.
@@ -85,6 +85,7 @@
 				}
 			} )
 			.prop( 'title', mw.msg( 'multimediaviewer-reuse-copy-share' ) )
+			.text( mw.msg( 'multimediaviewer-reuse-copy-share' ) )
 			.tipsy( {
 				delayIn: mw.config.get( 'wgMultimediaViewer' ).tooltipDelay,
 				gravity: this.correctEW( 'se' )
```

* resources/mmv/ui/mmv.ui.reuse.share.less:
```diff
@@ -18,13 +18,12 @@
 	.mw-mmv-dialog-copy {
 		// style rules based on .mw-mmv-share-page-link
 		float: right;
-		width: 1.5em;
-		height: 1.5em;
+		width: 2em;
+		height: 2em;
 		// position approximately to the middle - probably fragile but couldn't find a better way as
 		// the height of OOUI input widget has both em and px parts and not possible to calculate
 		// exactly
-		margin: 8px 0.5em 8px 0;
-		padding: 0 0 0 5px;
+		margin: 8px 0.5em;
 	}
 }
 ```

* resources/mmv/ui/mmv.ui.stripeButtons.js:
```diff
@@ -59,7 +59,8 @@
 		$button = $( '<a>' )
 			.addClass( 'mw-mmv-stripe-button empty ' + cssClass )
 			// elements are right-floated so we use prepend instead of append to keep the order
-			.prependTo( this.$buttonContainer );
+			.prependTo( this.$buttonContainer )
+			.attr( 'tabindex', '0' );
 
 		return $button;
 	};
```

[Issue T161612 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T161612)

[Issue T161612 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/408577/)

***
### Addition of label to input field in Visual Editor Categories page

#### Contributors:
* Jaziel Pauda
* Hunter Hobbs

#### Status:
This contribution was accepted and merged on 4/17/2018 and was deployed on 4/24/2018.

#### Results:
* Before adding category label:
![](https://i.imgur.com/xEQpOnm.png) 

* After adding category label:
![](https://i.imgur.com/uxBiVlM.png)

#### Description:
This issue revolved the improvement of the "Categories" dialog page in the MediaWiki Visual Editor extension. Specifically, it involved the addition of a 'label' element for the top input field of the this page. This addition was necessary because the previous version of this page relied on user solely on input field placeholder text to inform the user of information necessary to understand the interface and fulfill user tasks. Furthermore, this addition was necessary to improve the experience of user accessing the interface with a screen reader as well as users with visual and cognitive impairments.

#### Work performed:
The work performed on this contribution began with the addition of the new label name into the messages namespace of the Visual Editor extension. This was accomplished by adding the name of the label into the 'extension.json' file of the extension. After this, the newly added message was defined by associating the message with the English text that would be displayed to the user in the 'en.json' file of the code base. The final step in adding this new label to the messages namespace of the extension was adding a description of the new message in the 'qqq.json' file that described the purpose of the label as well as the information it conveyed to the user so that this new label could be translated accurately into the different languages MediaWiki supports. The final step in the development of this contribution was utilizing MediaWiki OO JS UI library to add a new FieldLayout to the 've.ui.MWCategoriesPage.js' file that included the category input widget as well as the newly created label for that input widget.

#### Changes:

* extension.json:
```diff
@@ -1693,6 +1693,7 @@
 				"visualeditor-categories-tool",
 				"visualeditor-dialog-meta-advancedsettings-label",
 				"visualeditor-dialog-meta-advancedsettings-section",
+				"visualeditor-dialog-meta-categories-addcategory-label",
 				"visualeditor-dialog-meta-categories-category",
 				"visualeditor-dialog-meta-categories-data-label",
 				"visualeditor-dialog-meta-categories-defaultsort-help",
```

* i18n/ve-mw/en.json:
```diff
@@ -151,6 +151,7 @@
 	"visualeditor-dialog-media-upload": "Upload",
 	"visualeditor-dialog-meta-advancedsettings-label": "Advanced settings",
 	"visualeditor-dialog-meta-advancedsettings-section": "Advanced settings",
+	"visualeditor-dialog-meta-categories-addcategory-label": "Add a category to this page",
 	"visualeditor-dialog-meta-categories-category": "Category",
 	"visualeditor-dialog-meta-categories-data-label": "Categories",
 	"visualeditor-dialog-meta-categories-defaultsort-help": "You can override how this page is sorted when displayed within a category by setting a different index to sort with instead. This is often used to make pages about people show by last name, but be named with their first name shown first.",
```

* i18n/ve-mw/qqq.json:
```diff
@@ -165,6 +165,7 @@
 	"visualeditor-dialog-media-upload": "Label for the upload button\n{{Identical|Upload}}",
 	"visualeditor-dialog-meta-advancedsettings-label": "Title for the advanced settings dialog section.\n{{Identical|Advanced settings}}",
 	"visualeditor-dialog-meta-advancedsettings-section": "Label for the advanced settings dialog section.\n{{Identical|Advanced settings}}",
+	"visualeditor-dialog-meta-categories-addcategory-label": "Label for field that adds a category to the page",
 	"visualeditor-dialog-meta-categories-category": "Title of popup for editing category options.\n{{Identical|Category}}",
 	"visualeditor-dialog-meta-categories-data-label": "Label for the categories sub-section.\n{{Identical|Category}}",
 	"visualeditor-dialog-meta-categories-defaultsort-help": "Message displayed as contextual help about the <nowiki>{{DEFAULTSORT:…}}</nowiki> control to editors in the page categories panel.",
```

* modules/ve-mw/ui/pages/ve.ui.MWCategoriesPage.js:
```diff
@@ -31,13 +31,25 @@
 		label: ve.msg( 'visualeditor-dialog-meta-categories-data-label' ),
 		icon: 'tag'
 	} );
+
 	this.categoryOptionsFieldset = new OO.ui.FieldsetLayout( {
 		label: ve.msg( 'visualeditor-dialog-meta-categories-options' ),
 		icon: 'advanced'
 	} );
+
 	this.categoryWidget = new ve.ui.MWCategoryWidget( {
 		$overlay: config.$overlay
 	} );
+
+	this.addCategory = new OO.ui.FieldLayout(
+		this.categoryWidget,
+		{
+			$overlay: config.$overlay,
+			align: 'top',
+			label: ve.msg( 'visualeditor-dialog-meta-categories-addcategory-label' )
+		}
+	);
+
 	this.defaultSortInput = new OO.ui.TextInputWidget( {
 		placeholder: this.fallbackDefaultSortKey
 	} );
@@ -64,7 +76,7 @@
 	} );
 
 	// Initialization
-	this.categoriesFieldset.$element.append( this.categoryWidget.$element );
+	this.categoriesFieldset.addItems( [ this.addCategory ] );
 	this.categoryOptionsFieldset.addItems( [ this.defaultSort ] );
 	this.$element.append( this.categoriesFieldset.$element, this.categoryOptionsFieldset.$element );
 };
```
#### Links:
[Phabricator issue ticket](https://phabricator.wikimedia.org/T185533)

[Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/426139/)

[Overview of merge on Phabricator](https://google.com)

***

### Correcting 

#### Contributors:
* Jaziel Pauda

#### Description:
Within the MediaWiki Flow extension, several `<div>` elements needed to be changed into `<article>` elements in order to better follow semantic HTML conventions, specifically W3 and MDN html standards.

#### Work performed:
The work started with verifying the issue author was correct that the `<div>` tags specified should become `<article>` tags by going over W3 and MDN standards. After this, the `<div>` tags that needed to be changed were identified by searching for the tags that had the CSS classes "flow-post-content" and "mw-parser-output". Then, the identified tags were changed to `<article>` tags, templates were compiled, and finally the changes were tested using PHPUnit.

#### Changes:

* handlebars/compiled/flow_block_topic_moderate_post.handlebars.php:
```diff
@@ -159,9 +159,9 @@
 '.$sp.''.((LCRun3::ifvar($cx, ((isset($in['isModerated']) && is_array($in)) ? $in['isModerated'] : null))) ? '		<div class="flow-moderated-post-content">
 '.$sp.''.LCRun3::p($cx, 'flow_post_moderation_state', array(array($in),array()), '			').'		</div>
 '.$sp.'' : '').'
-'.$sp.'	<div class="flow-post-content mw-parser-output">
+'.$sp.'	<article class="flow-post-content mw-parser-output">
 '.$sp.'		'.LCRun3::ch($cx, 'escapeContent', array(array(((isset($in['content']['format']) && is_array($in['content'])) ? $in['content']['format'] : null),((isset($in['content']['content']) && is_array($in['content'])) ? $in['content']['content'] : null)),array()), 'encq').'
-'.$sp.'	</div>
+'.$sp.'	</article>
 '.$sp.'
 '.$sp.''.LCRun3::p($cx, 'flow_post_meta_actions', array(array($in),array()), '	').''.LCRun3::p($cx, 'flow_post_actions', array(array($in),array()), '	').'</div>
 ';},'flow_anon_warning' => function ($cx, $in, $sp) {return ''.$sp.'<div class="flow-anon-warning">
```

* handlebars/compiled/flow_block_topic_moderate_topic.handlebars.php:
```diff
@@ -159,9 +159,9 @@
 '.$sp.''.((LCRun3::ifvar($cx, ((isset($in['isModerated']) && is_array($in)) ? $in['isModerated'] : null))) ? '		<div class="flow-moderated-post-content">
 '.$sp.''.LCRun3::p($cx, 'flow_post_moderation_state', array(array($in),array()), '			').'		</div>
 '.$sp.'' : '').'
-'.$sp.'	<div class="flow-post-content mw-parser-output">
+'.$sp.'	<article class="flow-post-content mw-parser-output">
 '.$sp.'		'.LCRun3::ch($cx, 'escapeContent', array(array(((isset($in['content']['format']) && is_array($in['content'])) ? $in['content']['format'] : null),((isset($in['content']['content']) && is_array($in['content'])) ? $in['content']['content'] : null)),array()), 'encq').'
-'.$sp.'	</div>
+'.$sp.'	</article>
 '.$sp.'
 '.$sp.''.LCRun3::p($cx, 'flow_post_meta_actions', array(array($in),array()), '	').''.LCRun3::p($cx, 'flow_post_actions', array(array($in),array()), '	').'</div>
 ';},'flow_anon_warning' => function ($cx, $in, $sp) {return ''.$sp.'<div class="flow-anon-warning">
```

* handlebars/compiled/flow_post.handlebars.php:
```diff
@@ -137,9 +137,9 @@
 '.$sp.''.((LCRun3::ifvar($cx, ((isset($in['isModerated']) && is_array($in)) ? $in['isModerated'] : null))) ? '		<div class="flow-moderated-post-content">
 '.$sp.''.LCRun3::p($cx, 'flow_post_moderation_state', array(array($in),array()), '			').'		</div>
 '.$sp.'' : '').'
-'.$sp.'	<div class="flow-post-content mw-parser-output">
+'.$sp.'	<article class="flow-post-content mw-parser-output">
 '.$sp.'		'.LCRun3::ch($cx, 'escapeContent', array(array(((isset($in['content']['format']) && is_array($in['content'])) ? $in['content']['format'] : null),((isset($in['content']['content']) && is_array($in['content'])) ? $in['content']['content'] : null)),array()), 'encq').'
-'.$sp.'	</div>
+'.$sp.'	</article>
 '.$sp.'
 '.$sp.''.LCRun3::p($cx, 'flow_post_meta_actions', array(array($in),array()), '	').''.LCRun3::p($cx, 'flow_post_actions', array(array($in),array()), '	').'</div>
 ';},'flow_anon_warning' => function ($cx, $in, $sp) {return ''.$sp.'<div class="flow-anon-warning">
```

* handlebars/flow_post_inner.partial.handlebars:
```diff
@@ -11,9 +11,9 @@
 		</div>
 	{{/if}}
 
-	<div class="flow-post-content mw-parser-output">
+	<article class="flow-post-content mw-parser-output">
 		{{escapeContent content.format content.content}}
-	</div>
+	</article>
 
 	{{> flow_post_meta_actions}}
 	{{> flow_post_actions}}
```

[Phabricator issue ticket](https://phabricator.wikimedia.org/T175937)

[Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/413091/)

[Overview of merge on Phabricator](https://phabricator.wikimedia.org/rEFLWa703662e1c62c251ad1e83bf0b84715dfb5ff437)

***

### Issue T156450 - QuizGame Special:QuestionGameHome should order headings correctly for semantics and accessibility 

#### Contributors:
* Jaziel Pauda

#### Description:
In the QuizGame extension, several `<h1>` elements were present on a single page of the extension. For accessibility and semantics reasons, all of these `<h1>` tags needed to be changed into `<h2>`'s.

#### Work performed:
The work started with determining if having multiple `<h2>` tags in one page was appropriate. After this, the specified heading tags were changed to `<h2>` tags and CSS classes with selectors for the `<h1>` tags were removed from the code base.

#### Changes:

* includes/specials/SpecialQuizGameHome.php:
```diff
@@ -418,10 +418,10 @@
 						$this->msg( 'quizgame-admin-back' )->text() . '</a>
 				</div>
 
-				<h1>' . $this->msg( 'quizgame-admin-flagged' )->text() . "</h1>
+				<h2>' . $this->msg( 'quizgame-admin-flagged' )->text() . "</h2>
 				{$flaggedQuestions}
 
-				<h1>" . $this->msg( 'quizgame-admin-protected' )->text() . "</h1>
+				<h2>" . $this->msg( 'quizgame-admin-protected' )->text() . "</h2>
 				{$protectedQuestions}
 
 			</div>";
@@ -671,7 +671,7 @@
 						'">
 
 						<div class="credit-box" id="creditBox">
-							<h1>' . $this->msg( 'quizgame-submitted-by' )->text() . "</h1>
+							<h2>' . $this->msg( 'quizgame-submitted-by' )->text() . "</h2>
 
 							<div id=\"submitted-by-image\" class=\"submitted-by-image\">
 							<a href=\"{$user_title->getFullURL()}\">
@@ -702,13 +702,13 @@
 
 						<div class=\"ajax-messages\" id=\"ajax-messages\" style=\"margin:20px 0px 15px 0px;\"></div>
 
-						<h1>" . $this->msg( 'quizgame-question' )->text() . "</h1>
+						<h2>" . $this->msg( 'quizgame-question' )->text() . "</h2>
 						<input name=\"quizgame-question\" id=\"quizgame-question\" type=\"text\" value=\"" .
 							htmlspecialchars( $question['text'], ENT_QUOTES ) . "\" size=\"64\" />
-						<h1>" . $this->msg( 'quizgame-answers' )->text() . "</h1>
+						<h2>" . $this->msg( 'quizgame-answers' )->text() . "</h2>
 						<div style=\"margin:10px 0px;\">" . $this->msg( 'quizgame-correct-answer-checked' )->text() . "</div>
 						{$quizOptions}
-						<h1>" . $this->msg( 'quizgame-picture' )->text() . "</h1>
+						<h2>" . $this->msg( 'quizgame-picture' )->text() . "</h2>
 						<div class=\"quizgame-edit-picture\" id=\"quizgame-edit-picture\">
 							{$pictag}
 						</div>
@@ -1170,7 +1170,7 @@
 						"</a>
 					</div>
 					<div class=\"credit-box\" id=\"creditBox\">
-						<h1>" . $this->msg( 'quizgame-submitted-by' )->text() . "</h1>
+						<h2>" . $this->msg( 'quizgame-submitted-by' )->text() . "</h2>
 
 						<div id=\"submitted-by-image\" class=\"submitted-by-image\">
 							<a href=\"{$user_title->getFullURL()}\">
@@ -1363,9 +1363,9 @@
 					htmlspecialchars( $this->getPageTitle()->getFullURL( 'questionGameAction=createGame' ) ) . '">
 				<div id="quiz-game-errors" style="color:red"></div>
 
-				<h1>' . $this->msg( 'quizgame-create-write-question' )->text() . '</h1>
+				<h2>' . $this->msg( 'quizgame-create-write-question' )->text() . '</h2>
 				<input name="quizgame-question" id="quizgame-question" type="text" value="" size="64" />
-				<h1 class="write-answer">' . $this->msg( 'quizgame-create-write-answers' )->text() . '</h1>
+				<h2 class="write-answer">' . $this->msg( 'quizgame-create-write-answers' )->text() . '</h2>
 				<span style="margin-top:10px;">' . $this->msg( 'quizgame-create-check-correct' )->text() . '</span>
 				<span style="display:none;" id="this-is-the-welcome-page"></span>';
 		// the span#this-is-the-welcome-page element is an epic hack for JS
@@ -1389,8 +1389,8 @@
 
 			'</form>
 
-			<h1 style="margin-top:20px">' .
-				$this->msg( 'quizgame-create-add-picture' )->text() . '</h1>
+			<h2 style="margin-top:20px">' .
+				$this->msg( 'quizgame-create-add-picture' )->text() . '</h2>
 			<div id="quizgame-picture-upload">
 
 				<div id="real-form">
```

* resources/css/questiongame.css:
```diff
@@ -28,14 +28,6 @@
 	padding: 0 0 0 2px;
 }
 
-.quizgame-edit-question h1 {
-	font-size: 16px;
-	font-weight: bold;
-	border-bottom: none;
-	color: #333;
-	margin: 20px 0 10px 0 !important;
-}
-
 .quizgame-picture img {
 	border: 1px solid #dcdcdc;
 	padding: 3px;
@@ -103,13 +95,6 @@
 	padding: 10px 0 0 0;
 }
 
-.quizgame-admin h1 {
-	font-size: 18px;
-	color: #333;
-	font-weight: bold;
-	margin: 0 0 20px 0 !important;
-}
-
 .quizgame-admin-top-links {
 	margin: -10px 0 20px 0;
 }
@@ -124,13 +109,6 @@
 	margin: 0 0 20px 0;
 	padding: 0 0 20px 0;
 	width: 500px;
-}
-
-.quizgame-flagged-item h1 {
-	font-size: 14px;
-	font-weight: bold;
-	color: #333;
-	margin: 0 0 10px 0 !important;
 }
 
 .quizgame-flagged-picture img {
@@ -253,15 +231,6 @@
 	margin: 8px 0 0;
 	padding: 10px;
 	padding-bottom: 60px;
-}
-
-.credit-box h1 {
-	border-bottom: none;
-	color: #333;
-	font-size: 16px;
-	font-weight: bold;
-	padding: 0;
-	margin: 0 0 10px !important;
 }
 
 .last-game {
@@ -406,27 +375,6 @@
 	border: 1px solid #dcdcdc;
 	padding: 9px;
 	width: 500px;
-}
-
-.create-message h1 {
-	font-size: 22px;
-	border-bottom: none;
-	font-weight: bold;
-	margin: 0 0 10px !important;
-}
-
-.quizgame-create-form h1 {
-	color: #333;
-	font-size: 16px;
-	font-weight: bold;
-	border-bottom: none;
-	margin: 20px 0 15px 0 !important;
-}
-
-h1.write-answer {
-	color: #333;
-	font-size: 16px;
-	margin: 20px 0 10px 0 !important;
 }
 
 .quizgame-answer-number {
```

[Issue T156450 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T156450)

[Issue T156450 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/416887/)

***

### Issue T185533 - Wikistats Beta: Fix accessibility/markup issues of Wikistats 2.0 (Part 1: .subdued classes and footer area) 

#### Contributors:
* Jesse Buck

#### Description: 

#### Work performed:
Changed several elements to use colors from the WikimediaUI Base color palette. Changed text using the "subdued" class, the color of text and links within the footer area, and the background color of the footer area. Although not explicitly mentioned in T185533, the metrics buttons within the Detail view was also changed.

#### Changes:
* src/App.vue
```diff
@@ -138,8 +138,8 @@
     border-bottom: none;
 }
 .ui.attached.footer.segment {
-    background-color: #3B3B3B;
-    color: #AAAAAA;
+    background-color: #54595d;
+    color: #fff;
     border: none;
 }
 ```
 
* src/components/BottomFooter.vue:
```diff
@@ -32,6 +32,9 @@
 </script>
 
 <style scoped>
-a, a:visited { color: #888; }
+a, a:visited {
+    color: #eaf3ff;
+    border-bottom: 1px dashed #eaf3ff;
+}
 .ui.centered { font-size: 1.1em; }
 </style>
```

* src/components/dashboard/MetricWidget.vue:
```diff
@@ -322,6 +322,6 @@
     margin: 4px 2px 2px 2px;
 }
 .subdued {
-    color: #9b9b9b;
+    color: #72777d;
 }
 </style>
 ```

* src/components/detail/Detail.vue:
```diff
@@ -240,7 +240,7 @@
     border: solid 2px #cdcdcd!important;
     font-size: 13px;
     font-weight: 500;
-    color: #9b9b9b!important;
+    color: #54595d!important;
     padding: 5px 9px;
     cursor: pointer;
 }
```

* src/components/detail/GraphPanel.vue:
```diff
@@ -201,7 +201,6 @@
 .graph.panel h2.header .subdued {
     margin-left: 4px;
     font-size: 18px;
-    color: #777;
     font-weight: 300;
 }
 .graph.panel .ui.right.floated.buttons {
```

[Issue T185533 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T185533)

[Issue T185533 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/419958/)


***

### Resolving WikiStats2 searchbar placeholder contrast and linting issues. 

#### Contributors:
* Michael Cornacchio  

#### Status:  

#### Results:  

#### Description:
The WikiStats 2.0 search placeholder did not comply with WCAG AA contrast requirements.  Additionally, the MediaWiki reviewers advocated for the use of their stylelinter.  The stylelinter identified numerous issues that required adjustment.
 
#### Work performed:
Replaced "4 space" indentation with tab indentation as identified by the stylelinter.  Used class hierarchy to identify placeholder rather than unique identifier.  As well as various other slight formatting changes.
 
#### Code Changes:
* src/components/TopicExplorer.vue:
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
#### Links: 

_Results:_  
* Before contrast modification:
![](https://i.imgur.com/yf7RVhO.png)  
 
* After contrast modification:
![](https://i.imgur.com/ztt7vnJ.png)

***

### Correcting minor markup issues in WikiStats2

#### Contributors:
* Jesse Buck
* Michael Cornacchio

#### Description:  A number of accessibility/markup issues were identified in the WikiStats2 extension beyond those already mentioned.

#### Work performed: 
* Added lang attribute to html element.  
* Added labels to topic search input and Wiki search input.
* Added alt tag for Wikimedia logo.
* Added main ARIA label in App.vue.
* Changed search header to a div.       

#### Changes:

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

[Issue T185533 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T185533)

[Issue T185533 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/426848/)
***

#### Glossary:  
_ARIA:_  
_Accessibility:_  
_Attribute:_  
_CSS:_  
_Codebase:_  
_Element:_  
_Focusable:_  
_Gerrit:_  
_HTML:_  
_JS:_  
_OO:_  
_MMV:_  
_Markup:_  
_PHP Unit:_  
_Patch Set:_  
_Placeholder:_  
_Semantic:_  
_Tags:_  
_UI:_  
_W3:_  
_WCAAG:_  
_Widget:_  
_Workboard:_  
