# Step 3: Record the candidate, verify it and show the verifier can fail

## The idea

The verifier compares a record with what's running. Two pieces of Git and container knowledge decide whether that comparison is fair.

- **A commit can't name itself:** a commit's id is a hash over everything it contains. A file can't hold the id of the commit it's in, because writing that id would change the hash. So the record always sits in a commit after the one it describes. The fair question is whether the recorded commit is in the branch's history. That's ancestry, and Git answers it directly.
- **A running container keeps its image:** a container is started from one image ID and keeps it until it's recreated. Changing the tag in your shell changes nothing about containers that are already running. Restarting them doesn't help either. Only starting them again through Compose, which builds and recreates as needed, moves them to the image the tag now names.

Then the verifier's structure follows. One part reads the live values: each container's image ID, including containers that have exited, the version the API reports, Git's answer on ancestry, and the versions recomputed from the repository. Another part compares those values with the record, without any stack. The comparing part is what your tests exercise. The reading part is what the passing and failing runs exercise.

> Covered in **CI Gates and Release Verification**.

## What's expected

- You recorded a candidate from a clean tree, committed the record, and checked each value against its source by hand.
- With the candidate's tag set and `poe start` and `poe ready` run, the verifier prints every field row as `match` and one row per first-party container, and exits 0.
- With another tag set and `poe start` run again, `release_tag` and `image_digest` mismatch, the other four match, and the exit code is non-zero.
- Back on the candidate's tag, the verifier passes again, `poe demo` passes and every evaluation case passes.
- You kept both verifier outputs, each with the tag the stack ran with.

## When you're stuck

### `image_digest` mismatches though nothing changed

<details><summary>Hint 1</summary>

Is the build context the commit, or the files on your disk?

</details>

<details><summary>Hint 2</summary>

Did a checkout, a merge or a restore rewrite any files on disk since you recorded?

</details>

<details><summary>Hint 3</summary>

On Windows, compare the line endings Git reports for the working tree with what the commit holds. Which command lists both?

</details>

<details><summary>Hint 4</summary>

Fix the line endings, rebuild under a scratch tag to confirm the ID matches, and only then use the candidate's tag again.

</details>

### `commit_sha` always mismatches

<details><summary>Hint 1</summary>

Which commit holds the record, and which commit does the record describe?

</details>

<details><summary>Hint 2</summary>

Can they ever be the same commit?

</details>

<details><summary>Hint 3</summary>

Ask Git whether the recorded commit is an ancestor of the head, and compare that answer instead.

</details>

### The failing case doesn't fail

<details><summary>Hint 1</summary>

Did the containers actually change when you switched tags?

</details>

<details><summary>Hint 2</summary>

Does a restart move a container to a new image, or keep the one it runs?

</details>

<details><summary>Hint 3</summary>

After every tag change, run `poe start` and `poe ready`, then run the verifier.

</details>

### The verifier says nothing is running

<details><summary>Hint 1</summary>

Does the verifier start the stack, or only read what's running?

</details>

<details><summary>Hint 2</summary>

Which tag did you start the stack with?

</details>

<details><summary>Hint 3</summary>

Start the stack with the candidate's tag first, then verify.

</details>

### The exited initializer is missing from your container rows

<details><summary>Hint 1</summary>

Does your listing include containers that have stopped?

</details>

<details><summary>Hint 2</summary>

Which flag makes the Compose listing show exited containers?

</details>

<details><summary>Hint 3</summary>

List all first-party containers, running or exited, and read each one's image ID.

</details>
