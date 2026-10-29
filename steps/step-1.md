# Step 1: Run every service from one application image

## Services show the same tag but different image IDs

<details><summary>Hint 1</summary>

How many of your services have a `build` block? Each one is a separate build.

</details>

<details><summary>Hint 2</summary>

If only one service builds, how do the others get that image instead of pulling or keeping an older one?

</details>

## A container runs code that isn't in the image

<details><summary>Hint 1</summary>

Read each service's volumes. Does any of them mount your source over the code the image carries?

</details>

<details><summary>Hint 2</summary>

Which one mount may stay, because the image never carries that data?

</details>

## A container starts something when it has no command

<details><summary>Hint 1</summary>

Does your Dockerfile say nothing about a command, or does it say "no command"? What does the base image do in the first case?

</details>

## A service fails at import time

<details><summary>Hint 1</summary>

Which dependency group holds the package it can't import? Does your one image install that group?

</details>

## The API won't start after the change

<details><summary>Hint 1</summary>

What runs before the API, and what command did it have before? Read `docker compose ps -a`, including exited containers.

</details>

## An authoring check crashes on a missing Dockerfile

<details><summary>Hint 1</summary>

Which file does the error name? Is that a check you run yourself, and does it come from the platform or from your own code?

</details>

<details><summary>Hint 2</summary>

Note what the check expects and why it no longer holds. If it's a supplied check you aren't allowed to change, say so in your evidence note.

</details>

## You're not sure which Dockerfiles to delete

<details><summary>Hint 1</summary>

After your change, which Dockerfiles does any service still build from? Search `compose.yaml` for each file name.

</details>
