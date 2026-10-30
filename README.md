Rabia Waseem
MATACS-TH-2026-02 
Time Spent 2.5 hrs

IDEA

The AI reads, the code decides, and a person approves. The model only turns proposal text into structured facts, with a quote for every fact. Scoring, ranking and the pass/fail checks are plain code, so results are repeatable and can be explained to a bidder. A reviewer confirms or overrides the final ranking.


WRITEUP.md	-    Answers to the five questions
PROMPTS.md	-    The extraction prompt and the optional explanation prompt, as I would send them
diagram/process-flow.png	  -   Process flow from upload to ranked output, showing AI steps, rule-based steps and human checkpoints
ranking.json-  Expected ranking of proposals A to E

I did not build the optional prototype.

How I used AI tools

I worked out the process and the ranking on paper first, then used Claude calculate ranking.json scores, checking and giving me opinions about my prompts and writeup. I disagreed with its scoring formulas. It suggested dividing experience by a fixed cap and scaling price and timeline linearly between the ceiling and the best bid; I replaced all of that with simpler formulas where the best eligible bid gets full marks and the others are scaled against it, because that treats every criterion the same way and is easier to explain. I also set the team points (3, 3, 4, 5) myself. Claude then pointed out that numbers such as 14 and 12 came from this sample only, so the tool should use the best eligible value instead.