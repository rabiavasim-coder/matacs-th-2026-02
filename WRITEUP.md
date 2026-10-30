1. AI vs. rules. 
AI acts as  a document extractor. It reads every proposal which may be in varied formats and then extracts the required information based on the given JSON format. These include  including contractor experience, pricing, timelines, warranties, licences, insurance and team details as this is the information on which the later scores will be evaluated. It will also interpret safety/monitoring plans and detect any security flags,  identify or highlight the missing information in the proposal, mark page number for each value filled in so human can track down relevant / supporting information if required later for reference. All the data is consolidated into single JSON file instead of giving multiple prompts and managing multiple  JSON files.
Another AI prompt is given to give a valid reason based on the scores explaining the why of rejection and state any missing information in the proposal which lead to explaining each position, including anything missing from the proposal. It is for the team to re verify or explain the position.
Python Code can be used for pass/fail checks, proposed budget, timeline, workforce are within the requirement, weighted score calculations and rankings. Also if any information is missing it is scored 0 as it is an error at the submitters end to not provide complete information.

2. Consistency and explainability.
Given the mandatory and pass/fail criterion, the scoring engine will apply fixed formulas and assign defied weights. Given the percentages of each extracted value from the document a total evaluation score of 100 is used. Experience (30): 
-qualifying installations divided by the highest eligible bidder's count, multiplied by 30.

-Price (25): lowest eligible price divided by bidder's price, multiplied by 25.

-Timeline (15): fastest stated eligible completion time divided by bidder's completion time, multiplied by 15. Missing timeline receives zero.

-Warranty (15): 7.5 points each for panel and workmanship warranties, normalized against 25 and 12 years respectively.

-Team and approach (15): named lead (3), defined crew (3), safety plan (4), and monitoring arrangements (5).
Results:
For repeatability, as the criterion is fixed and scores are mathematically calculated,Scoring is pure arithmetic, so it gives identical results for identical facts. Model output can vary slightly, so the extracted facts are saved on the first run and every re-run reads that saved copy. Validation and human review cover anything uncertain. Reviewers would see the overall score, individual criterion scores, supporting evidence and reasons for each ranking. Missing weighted information would receive zero points and be clearly identified.
B is ineligible because the bidder does not demonstrate the mandatory electrical contractor licence. D receives zero for its missing timeline but remains eligible for ranking because its mandatory requirements are satisfied.

3. Humans in the Loop

A person is involved at four points:

Review queue. Anything that failed validation, had an ambiguous date, or a quote that didn't match the source waits for a person to fix or confirm it.
Flagged proposals. These are held out of the ranking until someone looks. Proposal C goes here.
Ineligible proposals. The reviewer can see why, with the quote, and reinstate one with a written note if the extraction was wrong.
Final ranking. The reviewer confirms or overrides any position, and an override needs a reason. The tool never awards anything.

The audit log keeps, for every run. file hash, criteria version, prompt version, model name, extracted facts with quotes, gate results, scores, flags, who confirmed or overrode what, their reason, and timestamps.

4. Changing criteria
Whenever criteria or weights are edited, the system would create a new numbered version of the configuration. Each evaluation would retain the criteria version used to generate its score.

Existing results would remain unchanged under their original version. If the administrator wants all proposals evaluated using updated criteria, the system would recalculate all proposals using the new version while retaining the previous results for comparison and auditing.This maintains historical consistency and ensures every evaluation is traceable.

5. Risks

Wrong extraction. Every fact needs a quote, and code checks that the quote exists in the source. Mismatches become null and go to review. Scanned PDFs with poor text go to the queue instead of being guessed.

Hallucination/Made-up facts. Missing information is null by rule, never filled in. A missing schedule, as in D, scores zero for timeline and is flagged, not assumed. Every extracted value should have supporting evidence and a page reference. Unsupported information would be marked as missing rather than assumed.

Prompt Injection/ Manipulation. Proposal C hides an instruction aimed at AI reviewers. The prompt treats proposal text as data, the model reports the attempt, and code flags it. Manipulation can only change the facts the model reports, and every fact needs a quote and page that code verifies, so a false claim has nothing to point to. Any security flag also holds the proposal for a human. I chose flagged over ineligible because the real content is fine and a human should judge whether the bidder wrote it. Its legitimate content would score about 76, which the reviewer can see.

Gaming the gates. Proposal B is the cheapest, but its licence belongs to a subcontractor yet to be named. The brief says the bidder must hold it, so B is ineligible. Price never rescues a mandatory failure.

Bias. The scoring formulas never see the company name, size or location, only the extracted facts. Reviewers see the full breakdown, so skewed results are visible. I would also run known test proposals after any prompt or model change.

Cost. One extraction call per proposal, cached by file hash, with no AI in scoring or re-scoring. A small model is enough because the task is reading, not reasoning.
