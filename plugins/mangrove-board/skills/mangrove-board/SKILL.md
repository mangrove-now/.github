---
name: mangrove-board
description: Create, triage and move tickets on the mangrove-freelance GitHub board (org mangrove-now, project 1). Use when the user asks to create a ticket/issue, update or move one, set its status/priority/size, review the backlog, or when finishing work tied to an issue in a mangrove-now repo, so the ticket reflects the real state of the work.
---

# mangrove-board

All freelance tickets live on one GitHub Project: **mangrove-freelance**, https://github.com/orgs/mangrove-now/projects/1.
Each ticket is an issue in the client's repo (`mangrove-now/<repo>`). The full human guide is `BOARD.md` in `mangrove-now/.github`.

## Writing a ticket

- **Language**: Spanish, clean and consistent, even if the request came in English or with typos.
- **Title**: verb first, sentence case, no `[web]`/`[app]` prefixes, no trailing period. *Agregar…*, *Corregir…*, *Mostrar…*, *Permitir…*, *Investigar…*.
- **Body**:
  ```
  ## Contexto
  <qué pasa o qué se necesita, y para quién>

  ## Qué hay que hacer
  - [ ] <cada cosa a entregar>

  ## Referencias
  <capturas, ejemplos, tickets relacionados>
  ```
  Use only what the user or the code tells you; never invent requirements. If there is no detail yet, write `_Sin detalle todavía. Completar antes de pasar a Todo._` under Contexto.
- **Type**: Bug (something broken), Feature (new capability), Task (tech work, research, maintenance).
- **Labels**: one area label from the repo's own set. List them with `gh label list -R mangrove-now/<repo>`; never create new ones without asking.
- **Never** put passwords, tokens or client logins in a ticket. If you find one, tell the user: edit history keeps it, so it must also be deleted from the issue's history in the UI and rotated.

## Status flow

| Status | When |
|---|---|
| Backlog | New and not prioritized (default for new tickets) |
| Todo | Prioritized, body complete, Size set |
| In Progress | You or someone started the work |
| In Review | PR open, waiting for review |
| Deployed | In production; closed issues land here. For mobile apps, merged is not in production: move it here once the release is published |
| Validated | The client saw it and approved it (moved by hand, ask the user) |

Keep the board honest: when you start work on an issue move it to **In Progress**; when you open its PR move it to **In Review** and put `Closes #<n>` in the PR body; when it ships, close the issue and make sure it is **Deployed**; once the client approves it, move it to **Validated**. Ask before closing an issue the user did not ask to close.

## IDs

```
PROJECT_ID = PVT_kwDOEQO43c4BiY7m

Status   PVTSSF_lADOEQO43c4BiY7mzhhRK_k
  Backlog db4be10f · Todo f75ad846 · In Progress 47fc9ee4 · In Review 64bc7190 · Deployed 535c0267 · Validated e7f6108c
Priority PVTSSF_lADOEQO43c4BiY7mzhmOf9A
  Urgent 56e43212 · High 02f5aeaa · Medium 399e0a96 · Low 71d20546
Size     PVTSSF_lADOEQO43c4BiY7mzhmOf9E
  XS 94445fd0 · S 9dab75e3 · M d0b55498 · L 5fa750ab · XL 35ac9580
Target   PVTF_lADOEQO43c4BiY7mzhmOf9I   (date, YYYY-MM-DD)

Issue types: Bug IT_kwDOEQO43c4CAGp3 · Feature IT_kwDOEQO43c4CAGp4 · Task IT_kwDOEQO43c4CAGp2
```

If a call fails with an unknown ID, the board was reconfigured: re-read them with
`gh project field-list 1 --owner mangrove-now --format json` and tell the user this file needs updating.

## Recipes

Requires `gh` with the `project` and `repo` scopes.

**Create a ticket and put it on the board**
```bash
URL=$(gh issue create -R mangrove-now/<repo> --title "<título>" --body-file body.md --label <area>)
ISSUE_ID=$(gh issue view "$URL" --json id --jq .id)
gh api graphql -f query='mutation($i:ID!,$t:ID!){updateIssueIssueType(input:{issueId:$i,issueTypeId:$t}){issue{number}}}' \
  -f i="$ISSUE_ID" -f t=<issue-type-id>
ITEM_ID=$(gh project item-add 1 --owner mangrove-now --url "$URL" --format json --jq .id)
```

**Find the board item of an existing issue**
```bash
gh api graphql -f query='query($o:String!,$r:String!,$n:Int!){repository(owner:$o,name:$r){issue(number:$n){id projectItems(first:10){nodes{id project{number}}}}}}' \
  -f o=mangrove-now -f r=<repo> -F n=<number> \
  --jq '.data.repository.issue.projectItems.nodes[] | select(.project.number==1) | .id'
```

**Set a field**
```bash
gh project item-edit --project-id PVT_kwDOEQO43c4BiY7m --id "$ITEM_ID" \
  --field-id <field-id> --single-select-option-id <option-id>      # Status / Priority / Size
gh project item-edit --project-id PVT_kwDOEQO43c4BiY7m --id "$ITEM_ID" \
  --field-id PVTF_lADOEQO43c4BiY7mzhmOf9I --date 2026-11-30          # Target
```

**Review what's open**
```bash
gh project item-list 1 --owner mangrove-now --format json --limit 500 \
  --jq '.items[] | select(.status != "Deployed" and .status != "Validated") | "\(.status)\t\(.priority // "-")\t\(.repository)#\(.content.number)\t\(.title)"'
```

After changing tickets, tell the user what moved in one short list (`#285 → In Review`), with the board link.
