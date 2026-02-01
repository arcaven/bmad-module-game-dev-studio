# CHANGELOG

## v0.1.5 - Feb 1, 2026

* Improve module-help.csv descriptions with "Use when..." clauses for better LLM comprehension
* Update all 26 workflow descriptions with '[Action]. Use [when].' pattern
* Move Correct Course workflow from anytime to 4-production phase (sequence 55)
* Remove sequence numbers from all anytime items
* Change "AI agent" to "chosen agentic tool" for vendor neutrality
* Properly order anytime items at top before phased workflows

## v0.1.4 - Jan 27, 2026

* The architecture creation process has been revamped and now has significantly more content relevant to game development. When creating your architecture document, you'll now make decisions on such things as rendering pipelines, physics systems, anti-cheat libraries, dialogue systems, and more. You'll be prompted for common starter templates and useful MCPs that interact with game engines.
* Fixed bug in multiple workflows that were using BMM's user_skill_level instead of GDS' game_dev_experience.

## v0.1.3 - Jan 26, 2026

* Help system update

## v0.1.2 - Jan 22, 2026

* Workflow init replaced with new help system
* Changing all references of BMGD to GDS

## v0.1.1 - Jan 21, 2026

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