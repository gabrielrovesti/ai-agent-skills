# Version And Migration Signals

Use this reference when the review diff touches build files, dependencies, framework APIs, test annotations, or configuration driven by library versions.

## Review for these risks

- dependency bump with no matching code or config updates
- framework API already deprecated or replaced in the local stack
- test annotation or helper drift after framework upgrade
- starter or module change with hidden runtime impact
- migration-like refactor done partially across only some files

## Review questions

- What version does the build file actually declare?
- Does the changed code match that version family?
- Are related tests or configuration updated in the same change?
- Is this a direct functional fix, or a partial upgrade disguised as one?

## Reporting guidance

- Name the exact dependency, annotation, helper, or config surface involved.
- If the correct answer depends on current framework docs, say that the risk is version-sensitive.
- Keep the finding concrete; avoid turning every old API into a finding unless it creates a real regression or migration block.
