# nk-landing

![nk-landing](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/social/nk-landing.png)

An agent skill for [Claude Code](https://code.claude.com) and [OpenAI Codex](https://developers.openai.com/codex). Turn a written description of a small business — hours, services, price ranges, the questions customers actually ask — into one self-contained HTML page with a booking door, where every confirmation and every calendar write waits for a member of staff to approve that exact draft.

Part of [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) — agent skills whose scripts were broken on purpose
before release to prove their checks react.

![nk-landing demo: one idea in, a finished page out](https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/nk-landing.gif)

## What it does

- Five invariants: nothing leaves without an approval, and an approval covers only what it names; an approval names the request, the revision, the words and the slot, and what goes out is compared with those; an approval works once; the visitor's side cannot approve; a page with no backend says so on the screen and its outbox says what would have been sent and to whom.
- `assets/starter.html`, a working page — services, hours, prices, the questions people actually ask, a slot picker, a staff desk where drafts wait, an outbox, and three buttons that try the guard on purpose — plus `assets/design-tokens.css`, the token file it shares with the other pages in this family.
- `scripts/make_page.py`: a JSON description becomes one file, tokens inlined, nothing fetched at load. While the description says `demo`, it refuses a phone number outside the reserved 555-01xx range and any e-mail that is not at an example domain.
- The page carries its own type: the Latin subsets of Fraunces, Inter and Space Mono (SIL OFL 1.1, licences in `assets/fonts/`) are inlined, which adds 135,972 bytes to every page. It still fetches nothing at load.
- `scripts/page_check.py`: twelve rules on the file itself — the description is read rather than hard-coded, demo mode is on the screen, no testimonials or star ratings anywhere, every write to the outbox goes through the guard, the guard compares each approval with what goes out and accepts only ones the desk issued, nothing on the customer form approves, labels and a live region, nothing fetched at load, colours from the token file, one file, and a footer that says what is made up.
- `scripts/probe_check.py`: opens the page in Chrome with `?probe=1` and reads the page's own eleven-case attack battery — the comparisons working, not just present. Without Chrome, open `page.html?probe=1` yourself.
- Standard library only (probe_check.py also needs Chrome). The page it writes has no dependencies and no build step.

The full procedure, the boundaries and where the rules came from are in [SKILL.md](SKILL.md).

## How it works

1. Write the description down
2. Keep it fictional while it is a demo
3. Write the page
4. Read the five invariants
5. Check it
6. Try the guard
7. Open it and book something

## Install

Pick one of four ways: three for Claude Code, one for OpenAI Codex. Skills load when a session starts, so open a **new** session after installing.

### 1 · Terminal, one command

```bash
git clone https://github.com/NickkkLian/nk-landing ~/.claude/skills/nk-landing
```

1. Run the command above (for one project only, clone into `.claude/skills/nk-landing` inside that project).
2. Start a new Claude Code session.
3. Check it loaded: type `/nk-landing` — it appears in the slash-command menu. Or just ask for the task; the skill triggers on its own.

### 2 · Claude Code in a terminal session (plugin)

The plugin route goes through the [nickkk-skills](https://github.com/NickkkLian/nickkk-skills) marketplace. Add it once; after that each skill is one command.

```
/plugin marketplace add NickkkLian/nickkk-skills
/plugin install nk-landing@nickkk-skills
```

1. In a Claude Code session, run the first line (once per machine).
2. Run the second line.
3. Start a new session (or run `/reload-plugins`). The skill shows up as `nk-landing:nk-landing`.

Without opening a session, the same two steps work from a shell: `claude plugin marketplace add NickkkLian/nickkk-skills` then `claude plugin install nk-landing@nickkk-skills`.

### 3 · Claude desktop app (Code tab)

**Add the marketplace first — Discover only searches marketplaces you have already added.**

<img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/panel-route.gif" alt="Adding the marketplace and installing a skill in the desktop app" width="640">

<sub>Recorded on 2026-09-16, when the marketplace listed ten skills, all at version 0.1.0; it lists more now. The repository list in this recording shows the recorder's own repositories because a GitHub account is connected; yours will show yours. Type the full name as in step 4.</sub>

1. In the chat box, type `/plugin marketplace` and press Enter (or open **Settings → Customize → Plugins**). The **Plugins** panel opens.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step1-type-plugin-marketplace.png" alt="/plugin marketplace typed in the chat box" width="480">
2. Top right, open **Add ▾** and choose **Add marketplace**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step2-add-menu.png" alt="The Add menu with Add marketplace" width="480">
3. Choose **Add from a repository**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step3-add-from-repository.png" alt="Add marketplace dialog: Add from a repository" width="480">
4. In **URL**, type the full `NickkkLian/nickkk-skills`. At the bottom of the list choose the row **Use "NickkkLian/nickkk-skills"**, then press **Sync**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step4-url-then-sync.png" alt="URL filled in, Sync button" width="480">
5. You land on **Discover**, filtered to the new marketplace (**Filter · 1**). Find **Nk landing** and press **Add**. Installed ones show **✓ Added**.
   <br><img src="https://raw.githubusercontent.com/NickkkLian/nickkk-skills/main/gallery/panel-route/step5-discover-add.png" alt="Discover list with Added and Add buttons" width="480">
6. Close the panel and start a new session.

To try it for one session without installing anything: `claude --plugin-dir ./nk-landing` from a clone.

### 4 · OpenAI Codex CLI

```bash
git clone https://github.com/NickkkLian/nk-landing.git ~/.agents/skills/nk-landing
```

1. Run the command above (for one project only, clone into `.agents/skills/nk-landing` inside that project).
2. Start a new Codex session.
3. Check it loaded, without spending a model call: `codex debug prompt-input | grep -o -- '- nk-landing[a-z0-9:-]*' | sort -u` prints `- nk-landing:nk-landing:`. Codex adds the `nk-landing:` prefix because this repository also carries a Claude Code plugin manifest. Ask for the task and the skill triggers on its own, or type `$` and pick it from the list.

## Compatibility

| Agent | Tested | What was checked |
|---|---|---|
| Claude Code (CLI 2.1.173, macOS) | yes | In a fresh project with an isolated Claude config, inside a macOS sandbox that blocked reading the tester's ~/.claude folder (settings, session history, memory), Desktop, Documents and Downloads, SSH keys and git identity, a plain request that never names the skill triggered it and it ran its bundled script. The route 2 plugin commands were also run from a shell with an isolated config: marketplace add, install, list. The brief was a paragraph about a two-person upholstery workshop and never named the skill. The run loaded it, took the example description with `--init`, rewrote it from the brief, built the page and checked it — eight turns. It used a number in the reserved 555-01xx range, as SKILL.md step 2 asks, and where the brief gave no street address it wrote “full address when you book” rather than inventing one. This run used an earlier version of the skill; afterwards an audit found that the booking guard compared an approval with the request's current draft instead of with the words actually going out. The guard was fixed; the page this run wrote carries the old guard and now reports it (L05/L06), and the same description rebuilt with the current skill (0.1.1) reports 0 findings and passes the page's own eleven-case guard probe. |
| OpenAI Codex CLI (0.155.0-alpha.9.2, gpt-5.6-sol, low reasoning, macOS) | yes | Copied into `~/.agents/skills` of a temporary home, in a fresh project, without the user's Codex config. From the same brief, Codex read SKILL.md and the generator's source, took the example description, rewrote it and built the page, then searched the output for commit(), approve and refuse and read that part of the page before it reported. Like the Claude run, it ran before the guard fix described above: its page carries the old guard and now reports it, and its own description rebuilt with the current skill (0.1.1) reports 0 findings and passes the eleven-case guard probe. |
| Cursor, Gemini CLI | no | Not tested. Their documentation says both read `~/.agents/skills`, the folder route 4 clones into; Gemini CLI asks before it activates a skill. |

Route 4 was checked for this repository: cloned from GitHub into a temporary home's `~/.agents/skills`, it was listed by the step 3 command. This skill's frontmatter uses only name, description, license, compatibility and metadata.

## Verify

```bash
python3 scripts/make_page.py --selftest
python3 scripts/page_check.py --selftest
python3 scripts/probe_check.py --selftest
```

Python 3.9+, standard library only; probe_check.py also needs Google Chrome or Chromium. page_check.py's
rules were each broken on purpose in a sandbox copy — 35 breakages, each turning the sample written for it
red, none by a crash. The page's own 11-case guard battery (`?probe=1`) was checked the same way: 9
breakages of the guard, each turning its own case red. make_page.py and probe_check.py have self-tests
but no break matrix.

## Limits

- **It does not send anything, and it has no backend.** Demo mode is the only mode the skill ships. Wiring it to a real messenger, calendar or database is a person's job, on their own infrastructure.
- **The guard is a demonstration of a rule, not a security control.** It runs in the visitor's own browser, where anything can be edited: whoever can change the page or inject a script into it can get past every check it makes. So "a forged approval is refused" (invariant 4) holds for the page as shipped — approvals made through it and its own code — not against someone who rewrites the page. What the guard is for is to make the rule visible and to define what the real system must enforce on its side.
- **It does not design a brand.** One token file, one layout. If you want a different look, change the tokens.
- **It cannot tell whether the description is true.** Hours, prices and the promise that somebody reads the desk are the owner's to keep.

## License

MIT. Read a script before letting it run in your environment.
