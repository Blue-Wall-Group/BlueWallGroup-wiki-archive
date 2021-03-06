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
    * Rationale:  Stand-Up meetings are a core component of the Scrum framework.  We are attempting to adhere to the Scrum framework as closely as possible and recognize Stand-Ups as necessary for that effort. 
    * Measurement:  Given that the question prompts are laid out and straightforward, performance on Stand-Ups per Sprint is determined by consistency and attendance.  

* Product Owner 
    * Definition: The Product Owner (PO) is intended to communicate their vision of the product to the the Scrum team.  More specifically, the PO is "responsible for maximizing the value of the product."  In our implementation of Scrum a developer will be chosen each Sprint to serve as the PO in addition to their normal duties. 
    * Rationale: Like the Stand-Ups, the PO is a crucial element of any Scrum implementation.  Without the Product Owner, the team lacks an highly accessible and accountable individual to ensure the team is working on valuable requirements. 
    * Measurement: As each Sprint comes to a close, the team can assess how well the PO performed their duties of: 
      * Clearly expressing Product Backlog items.
      * Ordering the items in the Product Backlog to best achieve goals and missions.
      * Optimizing the value of the work the Development Team performs.
      * Ensuring that the Product Backlog is visible, transparent, and clear to all, and shows what the Scrum Team will work on next.
      * Ensuring the Development Team understands items in the Product Backlog to the level needed.
* Scrum Master 
    * Definition: The Scrum Master (SM) serves as the primary facilitator of the Scrum team. More specifically, the SM is responsible for "making sure a Scrum team lives by the values and practices of Scrum."  As is the case for the Product Owner, the Scrum Master role will rotate amongst team members per Sprint. 
    * Rationale: The SM can be considered a necessary balancing force to the PO: a "Process Owner" who's responsible for the team's performance.   
    * Measurement: The team can assess the SM's overall performance by considering their performance on individual items including: 
      * Ensuring that goals, scope, and product domain are understood by everyone on the Scrum Team as well as possible.
      * Finding techniques for effective Product Backlog management.
      * Helping the Scrum Team understand the need for clear and concise Product Backlog items.
      * Understanding product planning in an empirical environment.
      * Ensuring the Product Owner knows how to arrange the Product Backlog to maximize value.
      * Understanding and practicing agility.
      * Facilitating Scrum events as requested or needed.
* Planning/Review/Retro
    * Definition:  The Sprint Planning session is a time-constrained event (usually 1 hour per Sprint-week) where the Product Owner, Scrum Master, and the Scrum Team work together to define what product backlog items will be completed.  The Sprint Review is conducted at the end of each Sprint and is an event where the team has an opportunity too demo completed features as well as assess whether or not the Sprint Goal (determined in the Sprint Planning) has been successfully completed.  The Sprint Retrospective (Sprint Retro) represents an opportunity for the team to examine their performance and consider whether modified behavior might improve performance.  
    * Rationale: The Sprint Planning, Retrospective, and Review are crucial activities any Scrum team must exercise in order to successfully develop valuable features, constantly improve performance, and ensure consistent Stakeholder awareness of product/feature development.
    * Measurement: Success for Sprint Planning, the Sprint Review, and Sprint retro must be individually measured.
        * Sprint Planning: A successful Sprint Planning session will have produced two key artifacts: a Sprint Goal and a Sprint Backlog.
            * Sprint Goal: A successful Sprint Goal will represent both what the Product Owner and development team want out of the Sprint.  A quality Sprint Goal will be relatively short, in part so that the intention of the Sprint can be concisely conveyed to any curious stakeholders.
            * Sprint Backlog: The Sprint Backlog consists of the Product Backlog items that the team decides to work on during the upcoming Sprint.  A quality Sprint Backlog will consist of items whose completion drives the team towards achieving the Sprint Goal.  Additionally, the Sprint Backlog items will have a list of tasks (as well as estimates per task) that the team determines is necessary for delivery.
        * Sprint Retrospective: 

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
                  criteria for all needed artifacts will be set during sprint planning.
    * Rationale: Acceptance criteria help measured the degree of "done" and correctness of any artifacts produced. 
                 Furthermore, it helps developers create valuable and high quality artifacts.
    * Measurement: The correctness and quality of all artifacts will be measured against the acceptance criteria 
                   established. This evaluation will happen as structured walkthroughs where at least two reviewers asses 
                   the artifact a developer is putting up for acceptance.

* Meet accessibility standards
    * Definition: Ensure that all artifacts produced for users meet accessibility standards defined by MediaWiki, in order 
                  to make the product (MediaWiki) accessible for ALL users.
    * Rationale: One of the goals established in this course is the contribution to an HFOSS project, working to make such 
                 a widely used product as MediaWiki more accessible to all users certainly qualifies as such a 
                 contribution.
    * Measurement: Produced artifacts conform to accessibility standards published my MediaWiki. Verification of this will 
                   take place during structured walkthroughs and with developer tools such as SiteImprove.

* Always have deliverable
    * Definition: Have a potential deliverable ready at all times, that encompasses work being completed in the current 
                  sprint.
    * Rationale: Agile methodologies call for frequent and deliveries of product, as well as prioritizing having working 
                 software that can be delivered at any point of an iteration.
    * Measurement: State of deliverable will be measured half way through current sprint and at the end of current 
                   sprint. State of deliverable will be graded against goal of current sprint, to determine if the 
                   deliverable provides best possible value in terms of what the team wanted to accomplish for the sprint.