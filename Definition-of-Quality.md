# Overview 
For this project we define product quality distinctly from process quality.  Both will be tracked as each Sprint progresses.  

The following is a list of behavior and accomplishments which we feel will result in a quality product/process.  Associated with each item is a rationale for its inclusion as well as our method for measuring its degree of success per Sprint.

# Process Quality
* Stand-Ups 
    * Definition: Daily (in our case nightly) concise meeting wherein all team members briefly answer three 
                  questions that keep the team collectively informed on individual team members' commitments.<br/>
                  Questions:  
                      - What did you do today?  
                      - What will you do tomorrow?  
                      - Are there any impediments in your way?
    * Rationale:  
    * Measurement:
* Product Owner - advocate for
    * Definition:
    * Rationale:
    * Measurement:
* Scrum Master - facilitate team progress
    * Definition:
    * Rationale:
    * Measurement:
* Planning/Review/Retro
    * Definition:
    * Rationale:
    * Measurement:
* Acceptance Critera
    * Definition:
    * Rationale:
    * Measurement:
* * 2 Reviewers other than developer
* * Document acceptance tests
* US prioritization (Rank) - documentation.
    * Definition:
    * Rationale:
    * Measurement:
* * Team definition of story point 
* How US fo from BL -> SL
    * Definition:
    * Rationale:
    * Measurement:
* * current: what achieves sprint goal
* Constant deliverable
    * Definition:
    * Rationale:
    * Measurement:

# Product Quality
* Meet coding standards for MW 
    * Definition: All code produced by the team must meet all code standards defined by the MediaWiki project, and the 
                  MediaWiki Accessibility sub-project. Links to coding standards can be found under the "Standards" 
                  section of the "home" page of the Wiki.
    * Rationale: All MediaWiki contributors are expected to adhere to conventions established by the project in order to 
                 maintain and improve the quality and maintainability of the code base.
    * Measurement: No patches submitted by the team during the current sprint get rejected because of violations to the 
                   coding standards.
* Pass test suite & Jenkins acceptance tests. 
    * Definition: Submitted patches should pass initial verification tests as well as post-merge tests, administered with 
                  Jenkins on Gerrit.
    * Rationale: Test suites run by Jenkins are the main way to prove submitted code behave correctly, and can be 
                 integrated into existing code base. Thus, all submitted patches must pass all test suites in order to be 
                 merged into code base.
    * Measurement: No submitted patches should get rejected because they fail to pass acceptance and integration tests.

* Acceptance criteria
    * Definition: All accepted artifacts must meet or exceed previously set and documented acceptance criteria. Acceptance 
                  criteria for all artifacts needed to be produced will be set during sprint planning.
    * Rationale: Acceptance criteria help measured the degree of "done" and correctness of any artifacts produced. 
                 Furthermore, it helps developers create valuable and high quality artifacts.
    * Measurement: The correctness and quality of all artifacts will be measured against the acceptance criteria 
                   established. This evaluation will happen as structured walkthrough where at least two reviewers asses 
                   the artifact a developer is putting for acceptance.

* Meet accessibility standards
    * Definition: Ensure that all artifact produced for users meet accessibility standards defined by MediaWiki, in order 
                  to make this product (MediaWiki) accessible for ALL users.
    * Rationale: One of the goals established in this course is the contribution to an HFOSS project, working to make such 
                 a widely used product as MediaWiki more accessible to all users certainly qualifies as such a 
                 contribution.
    * Measurement: Produced artifacts conform to accessibility standards published my MediaWiki. Verification of this will 
                   take place during structured walkthroughs and with developer tools such as SiteImprove.

* Always have deliverable
    * Definition: Have a potential deliverable ready at all times, that encompasses work being completed in the current 
                  sprint.
    * Rationale: Agile methodologies call for frequent and deliveries of product, as well as prioritizing having working 
                 software that can be delivered.
    * Measurement: State of deliverable will be measured half way through current sprint and at the end of current of 
                   sprint. State of deliverable will be graded against goal of current sprint, to determine if the 
                   deliverable provides best possible value in terms of what the team wanted to accomplish for the sprint.