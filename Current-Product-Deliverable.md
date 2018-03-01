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

_Description:_ This issue focused on a MediaWiki extensions called Multimedia Viewer. Some of the buttons that are part of the multimedia viewer were tagged in the HTML as `<div>`s or `<a>`s. The result of this is that if the button does not have a label attribute, then a screen reader does not have any textual information to provide content to the button for the user.

_Work performed:_ We went through each JavaScript file in the Multimedia Viewer directory and looked for button elements that were tagged improperly. In some cases, the `<a>` tags were able to just be replaced with `<button>` tags. In the case of the copyButton, we added an aria-label attribute with a value of “copy text” that was output via the MW-messages API.  We needed to fix some of the button styling in the associated CSS files as well. 

This issue is not completely resolved yet. We are currently working on patchset 5. The history of our changes and patchsets, as well as the maintainer review and continuous integration test results can be view on the Gerrit web interface.

[Issue T161612 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T161612)

[Issue T161612 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/408577/)

***

### Issue T175937 - Flow could use article tags

_Description:_ Within the MediaWiki Flow extension, several `<div>` elements needed to be changed into `<article>` elements in order to better follow semantic HTML conventions.

_Work Performed:_

[Issue T175937 MediaWiki Phabricator Work board link](https://phabricator.wikimedia.org/T175937)

[Issue T175937 Gerrit Patch Submission Review and Code Diff link](https://gerrit.wikimedia.org/r/#/c/413091/)

[Issue T175937 MediaWiki Phabricator merged commit](https://phabricator.wikimedia.org/rEFLWa703662e1c62c251ad1e83bf0b84715dfb5ff437)

