# Step 4: Prove it with tests and write the evidence note

## The secret scan fails on your evidence note

<details><summary>Hint 1</summary>

What did you paste next to a service name such as `api`? A long hexadecimal string after a word like that can look like a key.

</details>

<details><summary>Hint 2</summary>

How does Docker show an image ID in its short form?

</details>

## A break in your code ended up in the candidate image

<details><summary>Hint 1</summary>

Which command did you run while the code was broken? Does it rebuild the image under the tag that's set in your shell?

</details>

<details><summary>Hint 2</summary>

Which command runs only your tests and never builds an image?

</details>

## Your verifier tests need the container engine

<details><summary>Hint 1</summary>

Which part of the verifier compares values, and which part gathers them? Can you call the first with values you choose?

</details>

## A fix after recording means a new candidate

<details><summary>Hint 1</summary>

Is the file you fixed inside the build context? If it is, the recorded image no longer matches. What does the lesson say to do before you cite a passing run?

</details>
