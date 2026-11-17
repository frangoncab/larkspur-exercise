# Client review: Larkspur disruption-care agent

**To:** Room 22  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-17 11:07

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

**1. agent.py search_alternatives and the committed trace: the description was rewritten, yet the run in readout-trace.json never calls it.**

The diff replaces the placeholder description "search" with a 499-character version and adds a description to the pnr property. readout-trace.json lists the tools called in order as lookup_booking, reopen_stats. That is 3 API turns and 2 tool calls, with no get_flight_status, search_alternatives, check_policy or hold_seat.

Run python3 run.py --all --trace and paste the tool-call sequence for each booking, including which PNRs reach search_alternatives.

**2. agent.py run_agent(): the answer variable is assigned only inside the while loop, after the second create() call.**

The diff moves answer = text_of(response) below turns += 1 and deletes the old assignment above the second create(). When the first response has stop_reason other than tool_use, the loop never runs and run_agent returns the initial empty string. Any booking that needs no tool call returns "" to the customer. A larger model does not change that return path.

Run python3 run.py --all --trace and paste the final answer text for every PNR that made zero tool calls.

**3. agent.py reopen_stats: the tool reads a sample the code itself caveats as anecdotal and cannot match the pnr it is given.**

reopen_stats reads data/americas/transcripts_sample.jsonl and builds rates per intent_label. The returned caveat string says one or two conversations per issue type, "Not specific to this booking", and pnr is echoed only. readout-trace.json shows this tool was the second call in the one recorded run. Any reopen rate the agent quotes to a customer rests on a handful of rows, and the model size does not change those counts.

Paste the by_issue_type conversations counts that reopen_stats returned in the readout-trace.json run.

**4. PITCH.md and the repository: Number reads "one more tool", and there is no evals/cases.json and no bench output.**

The Number line gives a count of tools, with no latency, token, cost or pass-rate figure. The only wire run is readout-trace.json: 10697 tokens in, 783 out, 10.3s, with 0 cache read and 0 written. cache_control appears nowhere in agent.py and TONE_ADDENDUM is still 0 characters. Priya cannot compare any model against a baseline that has not been measured.

Run python3 bench.py --label before --stage 1 --runs 3, then after your change python3 bench.py --label after --stage 1 --runs 3, and paste the python3 bench.py --compare before after output.

**5. PITCH.md Lever says "All of them" and Still broken says the agent cannot rebook, while confirm_rebooking and hold_seat are in the 11-tool list.**

PITCH.md picks cost, speed and intelligence together. The diff touches no code aimed at cost or speed: cache_control is absent, streaming is absent, and the loop ceiling is still MAX_TOOL_CALLS = 8. The hold_seat description is 73 characters and send_confirmation is 71, the two shortest of the 11 schemas. The gates banked in readout.html are 1.2, 1.3, 1.4, and 2.1, 2.2, 3.1, 4.1 are not banked.

Run python3 verify.py 2.1 and python3 verify.py 2.2 and paste both outputs.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (357 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
