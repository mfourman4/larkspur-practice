# Client review: Larkspur disruption-care agent

**To:** Room 1  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-09 13:08

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py run_agent(): the loop appends response.content and returns text_of(response) from the final turn, with MAX_TOOL_CALLS at 8.**

Your diff replaced the string-only assistant message with the full response.content and moved the return to the last response. The committed readout-trace.json shows 4 API turns and 5 tool calls, so the loop continued through lookup_booking, get_flight_status, next_available_day, check_policy and search_alternatives. You can claim to a sponsor that the loop runs multi-step and returns the final answer.

Run python3 verify.py 1.2 and paste its output.

**2. agent.py build_tools(): hold_seat is 73 characters and send_confirmation is 71, next to check_policy at 445.**

hold_seat reads "Place a 15-minute hold on one alternative. Reversible. It simply expires." and send_confirmation reads "Send the customer a written confirmation of what was just done. Benign." Neither says when to call it or what precedes it. Your readout-trace.json run never called either tool, and no run in this repo shows where in the sequence they fire.

Run python3 run.py --all --trace and paste the tool order for every ticket that reaches a seat hold.

**3. PITCH.md says R8KD3F gets a calm answer and H4MC7Y gets no hotel, but TONE_ADDENDUM is still 0 characters and check_policy is unchanged in your diff.**

Your diff touches search_alternatives, the date format and the loop, and leaves check_policy at 445 characters. The H4MC7Y no-hotel result comes out of the policy table behind check_policy and the model reads it. A larger model reading the same policy row returns the same row. Priya's question about using the best model cannot be answered with a model name until these two cases are tied to a cause.

Run python3 run.py H4MC7Y --trace and paste the check_policy result block with its policy_row_id.

**4. No evals/cases.json exists, and support/review.py differs from the shipped pack.**

The repository has no eval cases, so nothing yet proves the agent behaves on any ticket beyond the five you ran by hand. The modified support/review.py is a given file you were asked not to edit. Any number that passes through it needs the reader to know what it changed.

Paste the git diff of support/review.py and run python3 eval_harness.py once you have written cases.

**5. PITCH.md cost lever: the $0.07 per ticket and the 11-tool, 2,911-token figure are in prose, and no bench-after.json shows a before and after.**

readout-trace.json shows 17626 in, 995 out, and prompt caching at 0 read, 0 written. agent.py has no cache_control anywhere, and the 2,911-token tool list goes out in full on every one of the 4 turns of that run. No second run exists to show a change in tokens or dollars.

Run python3 run.py --tool-tax and paste its output, then python3 bench.py --compare on a before and after label.

## Your four answers

Four of the lines in your PITCH.md are answers to me rather than to your
verifier, and somebody on your side wrote them between our sessions. I read
those beside the code, not instead of it. Where an answer is carrying a number,
I have said whether the repository backs it.

| My question | Your answer | My read |
| --- | --- | --- |
| **What it costs** | About $0.07 of model cost per ticket (89,601 input and 5,859 output tokens across five tickets, at Sonnet list prices, no caching). That is model cost only, not Larkspur's loaded cost. The tool list is re-sent on every turn and makes up roughly two-thirds of the input tokens. | **Thin.** Your 89,601 input and 5,859 output tokens over five tickets, and the "roughly two-thirds" tool-list share, have no run behind them in the material. Only the single-ticket readout-trace.json (17626 in, 995 out) is committed, so paste the python3 run.py --tool-tax output or a bench-after.json compare to back it. |
| **When it is wrong** | When the agent is wrong it can only hold a seat, which is reversible, and it cannot confirm. The two known wrong answers are in "Still broken" above. | **Thin.** You name hold-only as the bound and point at two known failures, which is specific, but it gives no rate and no denominator across tickets. evals/cases.json does not exist, so nothing counts how often the policy answer or the tone handling goes wrong. |
| **Who runs it** | A Python program calling the Claude Messages API with 11 tools. Anything the Handbook reserves for people goes to a person. | **Thin.** "A Python program calling the Claude Messages API with 11 tools" names no owner at Larkspur, and the scan finds 9 shipped schemas with EXTRA_TOOLS at 0 and no LOCAL_TOOLS executors. The two added tools, next_available_day and fare_rules, are not visible in agent.py, so the 11 cannot be checked here. |
| **What you left out** | What the Handbook says chat will not do: refunds, fare-difference collection, hotel approval, other-airline rebooking, groups of 10 or more, unaccompanied minors, partner-operated flights, pets in cabin and medical requests on a new flight, and baggage tracing. | **Answered.** The list of refunds, groups of 10 or more, unaccompanied minors, partner-operated flights and the rest matches the scope that escalate_to_human lists. The Handbook itself is not in the material, so spot-check one item against it. |

All four answered. Bring the artifact behind each one to our next meeting, not the sentence.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (231 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- `PITCH.md (4 of 4 answers to Priya)`
- given files that differ from the shipped pack: `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
