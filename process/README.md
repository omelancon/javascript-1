# Process

## Definition of Done (DOD)
- Follow the Testing Guide
- Follow the Programming Guide
- Get your pull request approved

## Pull Request
The purpose of a pull request is to ensure the quality of the work being proposed. The focus should be on the design and the functionality itself. The best way to accomplish this is to read the spec, read the implementation, validate the principle that supports it and conduct an actual test of the feature.

### Guidelines
- Anyone can propose changes to the pull request.
- Anyone can Approve the pull request.
- Only one approval is needed.
- Anyone can update the branch to avoid it getting stale.
- Whoever updates the branch must deal with the merge conflicts.
- `Ready to Merge` tag can only be applied by the repository owners.
- Anyone can merge a `Ready to Merge` pull request.
- Pull requests with `Work In Progress` are hands off, until the tag is removed.
- When ready to review a pull request should be tagged with `Ready for Review`.

### Approving a Pull Request
The approval means that the approver has done its due diligence.
1. Checkout the pull request.
1. Read the specifications.
1. Read the implementation.
1. Read the tests.
1. Test the feature locally.
1. Run the Functional Tests.
1. Validate that the DOD was followed.
1. Remove the `Needs Review` tag and submit your review comment (approve/comment).

### Who Can Merge
To merge you have to complete the DOD and have a repository owner give you the green light by applying the `Ready to Merge` tag.
