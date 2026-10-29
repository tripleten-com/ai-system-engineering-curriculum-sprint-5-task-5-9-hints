# Step 4: Prove it with tests and write the evidence note

## The idea

Two claims need tests:

- **The record:** it has exactly six fields and refuses anything else.
- **The verifier:** it reports a mismatch when a recorded value differs from the observed one.

Both are testable without a stack, because of how you built them in Step 3. The comparing function takes values you choose. The gathering code sits behind an interface that a test can replace. Your tests must pass on a machine with no containers running.

This Task has a trap the earlier ones didn't. `poe verify` starts the stack, and starting the stack builds the image under whatever tag is set in your shell. Break your code for a test run with the candidate's tag set, run `poe verify`, and the candidate's tag now points at an image built from broken code. So the break-and-restore loop uses only the command that runs your tests, which builds nothing. And once you've recorded, any fix to a file inside the build context means a new candidate under a new tag.

One more trap is in the evidence note. A long hexadecimal image ID written next to a word like `api` looks like a key to the secret scan. In the note, write image IDs in Docker's short form.

> Covered in **CI Gates and Release Verification**.

## What's expected

- The record tests:
  - six fields load
  - a missing field, a seventh field and an empty value are each rejected
  - the record command refuses a dirty tree and an existing tag
- The verifier tests:
  - when every observed value agrees with the record, every row reads `match` and the verdict is a pass
  - changing one observed value, the image ID first, makes that row `mismatch` and the result a failure
- The tests pass with no stack running.
- You broke each group once and saw a test fail, without rebuilding the candidate's image.
- Your note gives both verifier outputs with their tags, and every image ID in its short form.
- `poe verify` and `poe demo` pass after your last edit, under the candidate's tag only if nothing in the build changed since you recorded.

## When you're stuck

### Your verifier tests need the container engine

<details><summary>Move 1</summary>

Which part of the verifier compares values, and which part gathers them?

</details>

<details><summary>Move 2</summary>

Can you call the comparing part with a record and observed values you build in the test?

</details>

<details><summary>Move 3</summary>

Put the calls to Git, Compose and the container engine behind one small interface, and give the tests a stand-in that returns fixed answers.

</details>

### A break in your code ended up in the candidate image

<details><summary>Move 1</summary>

Which command did you run while the code was broken?

</details>

<details><summary>Move 2</summary>

Does that command build an image, and under which tag?

</details>

<details><summary>Move 3</summary>

Use the command that runs only your tests for every break. If the candidate's image already moved, record a new candidate under a new tag.

</details>

### The secret scan fails on your evidence note

<details><summary>Move 1</summary>

What did you paste next to a service name such as `api`?

</details>

<details><summary>Move 2</summary>

How does Docker show an image ID in its short form?

</details>

<details><summary>Move 3</summary>

Write short IDs in the note, and keep the full ones in the record and the verifier's own output.

</details>

### A fix after recording means a new candidate

<details><summary>Move 1</summary>

Is the file you fixed inside the build context?

</details>

<details><summary>Move 2</summary>

If it is, does the recorded image still match what the next build produces?

</details>

<details><summary>Move 3</summary>

Commit the fix, record a new candidate under a new tag, and repeat the passing run from Step 3 before you cite it.

</details>
