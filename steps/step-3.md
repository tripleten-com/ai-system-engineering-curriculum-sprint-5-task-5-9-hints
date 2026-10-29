# Step 3: Record the candidate, verify it and show the verifier can fail

## `image_digest` mismatches though nothing changed

<details><summary>Hint 1</summary>

Is the build context the commit, or the files on your disk? Did a checkout or a restore rewrite any of them?

</details>

<details><summary>Hint 2</summary>

On Windows, compare the line endings Git reports for your files with what the commit holds. Which command lists both?

</details>

## `commit_sha` always mismatches

<details><summary>Hint 1</summary>

Which commit holds the record, and which commit does the record describe? Can they ever be the same?

</details>

## The failing case doesn't fail

<details><summary>Hint 1</summary>

Did the containers actually change? A restart keeps each container on the image it already runs. What does a new tag need to take effect?

</details>

## The verifier says nothing is running

<details><summary>Hint 1</summary>

Does the verifier start the stack, or only read what's running? Which tag did you start with?

</details>

## The exited initializer is missing from your container rows

<details><summary>Hint 1</summary>

Does your listing include containers that have exited?

</details>
