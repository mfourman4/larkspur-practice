# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A disruption-care chat agent for Larkspur. It handles all five test ticket types, and we added two read-only tools served from a separate MCP server: next_available_day (the earliest day with an open seat) and fare_rules (quotes the Handbook). We also gave it a written rule for abusive or threatening customers: one calm boundary message, no entitlements, and the case handed to a person with the customer's own words quoted.
Does: Looks up the customer's booking, checks their flight's live status, works out what Larkspur owes them, finds rebooking options and holds a seat for 15 minutes. The customer confirms by clicking. If a customer is abusive or threatens to sue or post publicly, it hands the case to a person instead of arguing.
Number: On our harder test tickets (3 runs on each of 5 tickets), the agent passed 12 of 15 hard checks on which tools it called before the tone rule and 15 of 15 after, and the abuse-handling hard gate went from failing to passing.
Safety check: The agent can hold a seat but cannot confirm a rebooking. Confirming needs a token that only the customer's own click produces, so "the customer said yes" in chat is not enough.
Next: Cut cost. The tool list is re-sent on every turn and is roughly two-thirds of the input tokens, and no turn uses prompt caching, so we will try caching it and measure with a second before and after.
Still broken: Internal policy reference codes (like UNC-B0) still reach customers, and our plain-language test failed in every full run. On H4MC7Y the policy check says no hotel while the Handbook says an overnight cancellation within our control qualifies for one.
Lever: intelligence
Caveat: The tone rule makes every request longer, so an ordinary ticket costs about 5% more in model cost ($0.0687 to $0.0722 per contact) while an abusive one costs less because it ends in two turns. The 12 of 15 and 15 of 15 come from 3 runs per ticket, so small differences are noise. Our own 10 test cases went from 5 passing to 9 passing in one run each, and one of the tone cases had its expectation reworded to stop asking for proof the system cannot show, so that pass is weaker evidence than the rest. None of this says anything about customers who are upset without being abusive.

## Priya asked

Costs: About $0.07 of model cost per contact: $0.0687 before the tone rule and $0.0722 after (17,825 and 19,243 input tokens, about 1,000 output tokens), 3 runs on each of 5 tickets, at Sonnet list prices with no caching. That is model cost only, not Larkspur's loaded cost. The tool list is re-sent on every turn and makes up roughly two-thirds of the input tokens.
Wrong: When the agent is wrong it can only hold a seat, which is reversible, and it cannot confirm. The two known wrong answers are in "Still broken" above. Of our 10 test cases, 9 pass and the one that fails is the reference-code case.
Runs it: A Python program calling the Claude Messages API with 11 tools. Anything the Handbook reserves for people goes to a person.
Left out: What the Handbook says chat will not do: refunds, fare-difference collection, hotel approval, other-airline rebooking, groups of 10 or more, unaccompanied minors, partner-operated flights, pets in cabin and medical requests on a new flight, and baggage tracing.
