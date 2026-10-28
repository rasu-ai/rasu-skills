# Recovery: refusals, conflicts and missing tools

Rasu AI refuses a call rather than doing something half right, and every
refusal says why. Read the reason, fix that cause, and try once more. If the
same call is refused again for the same reason, stop and tell the person in
plain words what is in the way.

## Common refusals and what to do

| The answer says | What it means | What to do |
|---|---|---|
| a 409 conflict, or a stale rev | Someone (often the person, in the studio) changed the record since you read it | Re-read the record, redo your change on the fresh version, send it with the new rev. Never overwrite their edit. |
| consultation-evidence, naming roles | The shot update lacks consults from those specialists | Consult the named roles on the shot, then send the update with all the evidence ids. |
| a code and a path | The fields you sent are malformed (a beat with no seconds, an asset under the wrong key) | Fix exactly that field. Nothing was written. |
| blocked, with missing items | An estimate or compile found the shot cannot generate yet | Fix each missing item, or tell the person what is needed. |
| the plan changed since the quote | The shot moved after you estimated | Estimate again; re-ask if the price changed. |
| a job already running | The same download or generation is in flight | Poll that job with `generation_read (type: gen_job_get)`; do not submit again. |
| not found | Wrong id, or something archived | Look it up by name again; check archived records if the person expects it to exist. |

Craft findings from `engine (type: engine_check)` are advice, never a refusal:
the write already went through. Answer them as the shot method says.

## A tool is missing from your list

Rasu AI's tools can be switched off one by one in some clients, and a
connection only gets the tools its permissions allow. A missing tool looks,
from your side, exactly like a feature Rasu AI does not have. So:

- Never tell the person Rasu AI cannot do something just because the tool
  is not in your list.
- Tell them which tool to turn on (by its name, such as `generate` or
  `seq_shot_update`) and that it is in their client's connector or MCP
  settings for the rasu server.
- If the server's own instructions end with a line saying some tools are not
  offered on this connection and which permission opens them, relay that:
  they need to reconnect and grant that permission.

## Not connected, or signed out

This skill does not connect Rasu AI by itself. If no Rasu AI tools exist
at all, the server has not been added. The person adds it as a remote MCP
server named rasu at `https://api.rasu.ai/mcp`, then approves the
sign-in in the browser window that opens. If calls fail with an
authorization error, ask them to sign in again the same way. Never ask for a
password, API key or token in the chat.

## When you are unsure

Ask one short question rather than guess. A wrong guess on a shared project
costs the person more than the question does.
