# Step 2: Build the record command and find where each field comes from

## The recorded image ID never matches what `poe start` runs

<details><summary>Hint 1</summary>

How does your record command build the image: the way `poe start` does, or with a plain build of the Dockerfile?

</details>

<details><summary>Hint 2</summary>

Inspect the labels of both images. What does Compose add that a plain build doesn't, and does any of it depend on the project name?

</details>

## Every tag you try is refused

<details><summary>Hint 1</summary>

Image names are shared by every project on your machine's container engine. Which tags already exist there? List them.

</details>

## Committing the record changes the image

<details><summary>Hint 1</summary>

Is the record's directory part of the build context? Read `.dockerignore`.

</details>

## Adding the verifier after recording moves the tag

<details><summary>Hint 1</summary>

Where do the verifier and its `poe` task live? Are those files inside the build context?

</details>

## You can't find the schema head

<details><summary>Hint 1</summary>

Each migration names the revision it follows. Which revision does no other file follow?

</details>

## The configuration digest differs between record and verifier

<details><summary>Hint 1</summary>

Do both use the same files, in the same order, and read the same bytes? Write the rule down once and call the same code from both.

</details>
