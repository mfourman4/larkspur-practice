# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A disruption-care chat agent for Larkspur. It handles all five test ticket types, and we added two read-only tools: next_available_day (the earliest day with an open seat) and fare_rules (quotes the Handbook).
Does: Looks up the customer's booking, checks their flight's live status, works out what Larkspur owes them, finds rebooking options and holds a seat for 15 minutes. The customer confirms by clicking.
Number: The five test tickets cost about $0.36 in model cost at Sonnet list prices, about $0.07 per ticket. The tool list alone (11 tools, 2,911 tokens) costs about $0.009 on every turn.
Safety check: The agent can hold a seat but cannot confirm a rebooking. Confirming needs a token that only the customer's own click produces, so "the customer said yes" in chat is not enough.
Next: Build the tone gate. Track and optimize cost.
Still broken: The abusive-message ticket (R8KD3F) gets a calm, normal answer with no tone handling, and on H4MC7Y the policy check says no hotel while the Handbook says an overnight cancellation within our control qualifies for one.
Lever: cost

## Priya asked

Costs: About $0.07 of model cost per ticket (89,601 input and 5,859 output tokens across five tickets, at Sonnet list prices, no caching). That is model cost only, not Larkspur's loaded cost. The tool list is re-sent on every turn and makes up roughly two-thirds of the input tokens.
Wrong: When the agent is wrong it can only hold a seat, which is reversible, and it cannot confirm. The two known wrong answers are in "Still broken" above.
Runs it: A Python program calling the Claude Messages API with 11 tools. Anything the Handbook reserves for people goes to a person.
Left out: What the Handbook says chat will not do: refunds, fare-difference collection, hotel approval, other-airline rebooking, groups of 10 or more, unaccompanied minors, partner-operated flights, pets in cabin and medical requests on a new flight, and baggage tracing.
