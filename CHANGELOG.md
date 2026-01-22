# CHANGELOG

## v6.0.2 - Jan 22, 2026

* Workflow init replaced with new help system

## v6.0.1 - Jan 21, 2026

NOTE: This is in preparation for BMAD's Beta 6 launch. It is a *significant* update and recommended for all BMGD users as it integrates all the new BMAD Beta 6 work done in the past month. Documentation is still incomplete and will come shortly. Thank you for your patience.

* Update of all shared BMM code to current version
  * Brownfield workflow init
  * Greenfield workflow init
  * Project context file is now created as part of both brownfield and greenfield workflow inits
  * quick-flow massively updated (quick-dev, quick-spec updated, quick-prototype removed) to match recent BMM Beta 6 changes
  * All workflows in 4-production changed to match recent BMM Beta 6 changes
  * Retrospective now uses actual BMGD agents, not BMM agents. Now GLaDOS will pat you on the head and Samus will randomly run around the table to work off energy. (Not really, but you get the idea.)
* Adding technical writer agent
* Adding document project workflow (used by technical writer agent and when beginning a brownfield (pre-existing codebase) project). Unmodified for now so will have many sections not typically used by games (such as API documentation)