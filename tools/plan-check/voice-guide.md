# Voice guide: how I talk upstream

## Who I am in threads

I'm a fifth year Software Engineering student at UPRM making my first contributions to Path Review as part of CodePath AI301. I'm comfortable reading and running code but new to this repo, so I state what I checked and what I didn't. Readers can expect a claim that says what I'll investigate, then a report with my environment, steps, and what I actually saw.

## Rules I write by

### Rule: Promise the investigation, not the fix

I only promise things I control right now: looking into the issue and posting what I find. No fixes, PRs, or dates until I've reproduced it.

- Wrong: "I'll take this and have a fix up by the weekend."
- Right: "I'll try to reproduce this locally and post a report here with my environment and results."

### Rule: Name this issue, not any issue

Every comment names something specific from the issue: the page, component, command, or error. If my comment would fit any other issue unchanged, I rewrite it.

- Wrong: "Hi, I'd like to work on this issue!"
- Right: "I'd like to take this. I'll check whether the profile page crashes when the bio field is empty, as described above."

### Rule: Say only what my output shows

My outcome line matches my artifacts. If I have a guess about the cause, I label it as a guess or leave it out.

- Wrong: "Reproduced. The bug is in the form validation."
- Right: "Reproduced on main at a1b2c3d. The page crashes with the error below. I haven't traced the cause yet."

### Rule: My proof, in my words

I post my own evidence even when someone else already reproduced it. I don't lean on another comment to do my work.

- Wrong: "Same as above, can confirm."
- Right: "I also reproduced this on Windows 11 with Node 20.11. Steps and output below."

### Rule: Disclose AI help when the repo asks

If the contribution policy requires disclosing AI assistance, every comment I post says so in one plain sentence.

- Wrong: (no mention, on a repo that requires disclosure)
- Right: "I used AI assistance for this investigation, per the contributing guidelines."

## Things I never post

- A fix, PR, or deadline promise before I've reproduced the issue.
- "Same as above", "+1", or "can confirm" as my proof.
- A root cause stated as fact without output that shows it.
- "Works for me" without my environment and output.
- Apologies for being new, or hype like "super excited to contribute!"
- A comment I haven't run through repro-check first.