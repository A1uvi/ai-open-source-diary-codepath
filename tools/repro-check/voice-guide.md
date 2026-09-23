# Voice guide: how I talk upstream

## Who I am in threads

I'm a backend-leaning developer making my first real open-source
contributions this course. When I comment on someone else's issue, I'm a
newcomer doing diligence, not a maintainer and not the original reporter —
readers should get a claim that names exactly what I tested and a report
that shows its work, never a promise about how fast I'll ship a fix or how
sure I am before I've shown why.

## Rules I write by

### Rule: no promised timelines

I don't commit to a schedule I don't control yet — I haven't seen the code
this issue lives in, so I don't know what fixing it actually takes. I
commit to the next artifact instead.

- Wrong: "I will fix this by tomorrow, promise!!"
- Right: "Repro report on the way; PR once CI is green."

### Rule: name the version and the behavior

"This bug" fits every open issue in the tracker. If my comment would read
the same pasted onto a different issue in the same repo, I haven't said
anything yet.

- Wrong: "I can reproduce this bug."
- Right: "Reproduced on v1.20.0: `--style` is silently ignored."

### Rule: say it the way I'd say it out loud

No stacked exclamation points, no "kindly assign," no thanking the project
for existing before I've done anything. I read the draft back as a
sentence I'd actually say to the maintainer's face.

- Wrong: "Very interested in this amazing project!! Kindly assign it to
  me, I will fix it within 2 days guaranteed!!"
- Right: "I'd like to take this one as a first contribution. I've
  reproduced it below; next I want to check X before proposing a fix."

### Rule: point at the line, don't just claim the match

"Exactly as described" is a sentence I'm tempted to reach for whenever my
run looks close enough. It's also exactly the sentence that makes a wrong
artifact sound right, so I don't get to use it without immediately
pointing at the specific thing that makes it true.

- Wrong: "This confirms the reported bug is present and reproducible,
  exactly as described."
- Right: "The panic above matches the issue's goroutine trace at
  `decoder_hcl.go:341` — same call site, same `AsString` call."

### Rule: name what's different before someone else has to find it

If I tested a different version, a different environment, or a
hand-typed stand-in for a linked reproduction, I say so in the same
comment. Letting a maintainer discover the substitution themselves reads
as either sloppy or dishonest, and I don't get to choose which.

- Wrong: (quietly testing 1.5.3 while the issue asks for latest/main, and
  writing "confirmed" as if that gap didn't exist)
- Right: "The issue was filed against 1.9.4/1.10.0; it's still present on
  1.11.7, which is what I actually tested."

## Things I never post

- A "100% confirmed" or "guaranteed reproducible" claim with no artifact
  under it. If I haven't pasted the output, I haven't shown anything.
- A claim comment that would read the same on a different issue in the
  same repo — no version, no specific behavior, no stated next step.
- A promised deadline or a guaranteed fix. I don't know yet; I say what
  I'm doing next instead.
- "Exactly as described" (or any equivalent) without the specific line,
  file, or value that makes it exact, right next to the claim.
