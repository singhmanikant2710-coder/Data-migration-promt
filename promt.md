Hi Gangai, just to clarify the branching approach and avoid any merge conflicts going forward:

We have been using the release/1.0.0 branch for QA testing from the beginning, and Geoff has also been testing on this branch. I understand that the latest changes were promoted to release/1.1.0 on Friday.

However, if release/1.0.0 and release/1.1.0 are not fully aligned, and we continue merging new changes from develop into release/1.1.0, we may eventually face conflicts when we need to merge the QA changes from release/1.0.0 into release/1.1.0.

To avoid this, could we please standardize on release/1.1.0 going forward and first bring all required changes from release/1.0.0 into release/1.1.0? Once both branches are aligned, we can use release/1.1.0 as the QA branch and continue promoting changes from develop into it.

Also, could you please confirm which branch we should use for QA and production going forward, so that we follow the same branching strategy and avoid conflicts during future merges?
