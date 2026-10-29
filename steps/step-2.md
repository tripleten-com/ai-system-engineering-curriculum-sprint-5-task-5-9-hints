# Step 2: Build the record command and find where each field comes from

## The idea

A release candidate record answers "which build is this, and where did it come from?" without anyone's memory. It has six fields, and each one is read from its source by a command, never typed:

- **The tag:** what you call the candidate.
- **The commit:** what it was built from.
- **The image ID:** what the build produced.
- **The schema head:** which database migrations it expects.
- **A digest over the configuration files:** which configuration it runs with.
- **The evaluation set's version:** which evaluation it should pass.

A value typed by hand is a belief. A value read from its source can be checked again later.

The record command is careful in a fixed order. It refuses first: a tree with uncommitted changes, which has no commit to record, and a tag that already names an image or a record. Then it builds the image the same way `poe start` does, because Compose adds its own labels to the images it builds, and a plain build gives a different ID. Then it reads each field, and writes six fields and nothing else.

Two placements decide whether the record stays true:

- **The record's directory is outside the build context:** otherwise committing the record changes the next build, and the recorded ID points at an image no tag names.
- **The verifier is written before you record:** it lives inside the build context too, so adding it later would change the image.

> Covered in **The Platform's Data Layer**.

## What's expected

- You wrote down the source of each of the six fields, and the rule for the configuration digest.
- Your record command refuses a dirty tree, an existing image tag, the default tag, and a tag already recorded.
- It builds through Compose with the tag set, then reads the six fields at the commit it built.
- The record holds exactly six fields, in a format the repository already reads.
- The record's directory is in `.dockerignore`.
- The verifier and both `poe` tasks are written and committed before you record.

## When you're stuck

### The recorded image ID never matches what `poe start` runs

<details><summary>Move 1</summary>

How does your record command build the image: the way `poe start` does, or with a plain build of the Dockerfile?

</details>

<details><summary>Move 2</summary>

Inspect the labels of both images. What does Compose add that a plain build doesn't?

</details>

<details><summary>Move 3</summary>

Does any of those labels depend on the Compose project name?

</details>

<details><summary>Move 4</summary>

The approach: build through Compose, for the one service that has the build, with the tag set in the environment, under the same project name `poe start` uses. Then read the ID of the tagged image.

</details>

### Every tag you try is refused

<details><summary>Move 1</summary>

Image names are shared by every project on your machine's container engine. Which tags already exist there? List them.

</details>

<details><summary>Move 2</summary>

Is the tag you're trying the default tag from `compose.yaml`?

</details>

<details><summary>Move 3</summary>

Choose a tag form that can't collide with an existing image and is never reused, and write the form down.

</details>

### Committing the record changes the image

<details><summary>Move 1</summary>

Is the record's directory part of the build context?

</details>

<details><summary>Move 2</summary>

Read `.dockerignore`. Which directories does the build never see?

</details>

<details><summary>Move 3</summary>

Add the record's directory there, commit that change, and record a new candidate.

</details>

### Adding the verifier after recording moves the tag

<details><summary>Move 1</summary>

Where do the verifier and its `poe` task live? Are those files inside the build context?

</details>

<details><summary>Move 2</summary>

What happens to the image when a file inside the build context changes?

</details>

<details><summary>Move 3</summary>

Write the record command, the verifier and both tasks, commit them, and only then record.

</details>

### You can't find the schema head

<details><summary>Move 1</summary>

Open `migrations/versions/`. What does each file name as its revision, and as the revision it follows?

</details>

<details><summary>Move 2</summary>

Which revision does no other file follow?

</details>

<details><summary>Move 3</summary>

Read the files the way your command will, and check your answer against the newest migration by hand.

</details>

### The configuration digest differs between the record and the verifier

<details><summary>Move 1</summary>

Do both use the same files, in the same order?

</details>

<details><summary>Move 2</summary>

Do both read the same bytes? A checkout can change line endings on disk.

</details>

<details><summary>Move 3</summary>

Write the rule down once and call the same code from both. Then decide which bytes the rule reads, knowing a checkout can change files on disk, and defend that choice in your note.

</details>
