# Step 1: Run every service from one application image

## The idea

The lesson asks for one image and proves it by the image ID. Three facts about images make that work.

- **What an image ID covers:** the ID is a hash over the image's configuration and every layer's content. Change one byte of a copied file, one installed package, one label or the default command, and the ID changes. Two images with the same ID are the same bytes. A tag is only a name pointing at one ID, and anyone can move it.
- **Compose adds labels:** when Compose builds an image, it stamps labels into its configuration, including the project name. So the same Dockerfile built by hand, or under another project name, gives a different ID from the one `poe start` builds. That's why everything that records or compares IDs has to build the way `poe start` builds.
- **Building and pulling:** a service with a build entry gets built under its tag. A service with only an image name looks for that tag, and may try to pull it from a registry or keep an older image under the same name. Marking a service never to pull tells Compose to use exactly the local image that one build produced.

Put together: build once, point every first-party service at that one tag, never pull, and then the ID you read from any container tells you what all of them run.

> Covered in **Registries, Tags, and Image Hygiene**, in **Releasing Containers Safely**.

## What's expected

- One Dockerfile installs every group the five first-party services need, keeps the build argument and the unprivileged user, and declares no default command.
- In `compose.yaml`, one service has the build. The other four first-party services use the same image with `pull_policy: never` and their own command.
- No first-party service mounts your source code. The read-only scripts mount may stay.
- Under a tag you use only for this check, `docker compose ps -a` shows one image name and tag for all five services, the exited initializer included.
- `GET /version` from the API reports that tag, and `poe demo` passes.
- The old Dockerfiles are gone, and nothing still references them.

## When you're stuck

### Services show the same tag but different image IDs

<details><summary>Hint 1</summary>

How many of your services have a `build` block?

</details>

<details><summary>Hint 2</summary>

What does Compose do when several services build to the same tag?

</details>

<details><summary>Hint 3</summary>

If only one service builds, how do the others get exactly that image instead of pulling or keeping an older one?

</details>

<details><summary>Hint 4</summary>

The approach: keep the build on one service, give the other four the same image name and tag, mark them never to pull, and run `poe start` again.

</details>

### A container runs code that isn't in the image

<details><summary>Hint 1</summary>

Read each first-party service's volumes in the resolved configuration.

</details>

<details><summary>Hint 2</summary>

Does any of them mount your source over the code the image carries?

</details>

<details><summary>Hint 3</summary>

Remove source mounts from every first-party service. Keep only the read-only scripts mount, because the image never carries that data.

</details>

### A container starts something when it has no command

<details><summary>Hint 1</summary>

Does your Dockerfile say nothing about a command, or does it explicitly say there's none?

</details>

<details><summary>Hint 2</summary>

What does the base image run by default when nothing overrides it?

</details>

<details><summary>Hint 3</summary>

Declare an empty command in the Dockerfile, so only a service's own command can start anything.

</details>

### A service fails at import time

<details><summary>Hint 1</summary>

Read the failing service's log. Which package can't it import?

</details>

<details><summary>Hint 2</summary>

Which dependency group holds that package? Does your one image install it?

</details>

<details><summary>Hint 3</summary>

Install every group any of the five services needs in the one image, then rebuild.

</details>

### The API won't start after the change

<details><summary>Hint 1</summary>

What runs before the API? Read `docker compose ps -a`, including exited containers.

</details>

<details><summary>Hint 2</summary>

Did that service keep its command when you moved it to the shared image?

</details>

<details><summary>Hint 3</summary>

The API and the worker take their commands from the last line of their old Dockerfiles. The initializer keeps the command it already had in `compose.yaml`. Restore whichever one went missing, and start again.

</details>

### You're not sure which Dockerfiles to delete

<details><summary>Hint 1</summary>

After your change, which Dockerfiles does any service still build from? Search `compose.yaml` for each file name.

</details>

<details><summary>Hint 2</summary>

Did you write a Dockerfile of your own for the agent server or the tool server in earlier Tasks?

</details>

<details><summary>Hint 3</summary>

Delete every Dockerfile nothing builds from any more, yours included, and commit the change together with the new Dockerfile.

</details>
