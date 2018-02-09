## Contribution/collaboration organization
- A repository is shared among the team.
- The team’s repository is kept up-to-date with the main MediaWiki/extension repository.

## Contribution/collaboration workflow
- Individuals clone the team’s repository
- Individuals push and pull changes from shared repository to share in-progress work.
- Individuals pull changes from the main repository as a means to update the shared repository, or before submitting changes to the main repository.
- Individuals submit changes to the main repository using the Gerrit “git review” process. (See: Contribution/collaboration steps)

## Initial setup 
(Assuming a somewhat standard environment, such as that provided by the MediaWiki Vagrant setup):
- Set up Gerrit and Git Review as described here: https://www.mediawiki.org/wiki/Gerrit/Tutorial, with a few minor changes:
-- Use the “upstream” remote to refer to the main MediaWiki/extension repository, rather than “origin”. As such, any references to “origin” should probably be replaced with “upstream”
-- Most importantly, instead of “git config --global gitreview.remote origin”, use “git config --global gitreview.remote upstream”.

- If “git remote -v” already shows “origin” as pointing to the main repository (such as in the Vagrant setup), use “git remote rename origin upstream”. 
-- Otherwise, use “git remote add upstream <main url>”, where <main url> is the address for the main MediaWiki/extension repository.

- Use “git remote add origin <team url>” to add the team’s repository as the new origin, where <team url> is the address for the team’s shared repository.
Note: Some of this process may need to be repeated when switching to work on a new MediaWiki extension, or to/from the core MediaWiki module.

 
If properly set up, you should get matching output with the given commands:

`git remote -v`

     origin     https://github.com/< >.git (fetch)

     origin     https://github.com/< >.git (push)

     upstream	https://gerrit.wikimedia.org/< >.git (fetch)

     upstream	https://gerrit.wikimedia.org< >.git (push)`


`git config --global --list`

     gitreview.username=<username>

     gitreview.remote=upstream

     user.email=<email>

     user.name=<full name>`


## Contribution/collaboration steps
(W.I.P.)
- Create or check out a branch.
- Push/pull commits for that branch to the shared repository.
- “Squash” changes into a single commit.
- Rebase on upstream.
- Submit to Gerrit for review

See (Using “upstream” rather than “origin”):
https://www.mediawiki.org/wiki/Gerrit/Tutorial#How_to_submit_a_patch

https://www.mediawiki.org/wiki/Gerrit/Tutorial#Other_common_situations 


