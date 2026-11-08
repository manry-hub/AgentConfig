I have a bug. Do NOT write any fix yet.

Error / unexpected behaviour: [PASTE THE ERROR]
What I expected: [DESCRIBE]
What actually happens: [DESCRIBE]
Relevant code: [PASTE OR POINT TO FILES]
What I already tried: [LIST]

Step 1: Restate the problem in your own words so I know we agree.
Step 2: Trace the failure path structurally — use `search_graph` to locate involved symbols and `trace_path` (inbound/outbound) to map the execution flow and callers before reading broad files or shotgun grepping.
Step 3: List the 3-5 most likely root causes, ranked by probability,
each with your reasoning.
Step 4: For each cause, give me the single fastest way to confirm or
eliminate it — a log line, a check, a one-line test.
Step 5: Stop and wait for my results.
Step 6: Only once we've confirmed the cause, write the minimal fix,
explain why it works, and tell me exactly what to test to verify.

Don't shotgun changes. Don't refactor unrelated code.
Don't fix things I didn't ask about.
