# Before you submit

The basic checks confirm that your work is there. They don't judge whether it's right. Your instructor reads your pull requests and evidence notes with four questions. Ask them of your own work first.

## Did you meet the Task's requirements?

- Does every first-party service run one image, built once, with no source mount except your scripts' data?
- Does the record hold exactly the six fields, each read from its source at build time?
- Does the record command refuse a dirty tree and a tag that already exists?
- Does the verifier compare every field and every container, name the mismatched rows and exit non-zero?

## Did you explain your design choices?

- Does your note give the configuration digest rule and the tag form?
- Does it say why `commit_sha` is checked by ancestry, not equality?

## Can your tests fail when the code is wrong?

- Did you let the record accept a seventh field, and make the verifier report `match` when the image IDs differ, once each, and see tests fail?
- Did you keep the candidate's tag safe while you broke code?

## Did you state the limits of what your checks show?

- Does your note say what a local build doesn't prove, such as a registry digest or another machine's build?
- Do both verifier outputs carry the tag the stack ran with, and short image IDs only?
