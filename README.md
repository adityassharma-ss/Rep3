ngpos-core-secrets-sync

.github

3

on:

workflows

4

!gh_pr_create.yml

!tf_apply_merge-dev.yml

! tf_apply_merge-qa.yml

!tf_apply_merge-stage.yml

9

10

!tf_apply_merge-test.yml

!tf_apply_merge.yml

12

!tf_drift_cron-dev.yml

13

!tf_drift cron-prod.yml

14

!tf_drift_cron-qa.yml

15

!tf_drift_cron-stage.yml

!tf_drift cron-test.yml

! tf_drift_cron.yml

! tf_plan_pr-dev.yml

16

17

18

!tf_plan_pr-prod.yml

!tf_plan_pr-ga.yml

!tf_plan_pr-stage.yml

!tf_plan_pr-test.yml

!tf_plan_pr.yml

tf_tool_unlockstate-dev.yml

!tf_tool_unlockstate-prod.yml

!tf_tool_unlockstate-ga.yml

!tf_tool_unlockstate-stage.yml

!tf_tool_unlockstate-test.yml

@

FCODEOWNERS

! dependabot.yml

> OUTLINE

> TIMELINE

PROBLEMS OUTPUT

main.tf...\ngpos-secret-sync 1

! tf_apply_merge-dev.yml

ngpos-core-secrets-sync >

.github >

!tf_toolunlockstate-dev.yml

main.tf...\dev

data.tf M

variables.tf M

workflows > !

tf_tool_unlockstate-dev.yml

1 name: "[dev] Terraform Force-Unlock"

2

workflow_dispatch:

inputs:

TF_LOCK_ID:

I

!tf_apply_merge-prod.yml

type: string

required: true

description: "Lock ID from state lock"

jobs:

11

uses: krogertechnology/cs-cloudops-gha/.github/workflows/tf_tool_unlockstate.yml@vi

with:

tf_plan_pr:

WORKING_DIRECTORY: ./terraform/workspaces/dev

ENVIRONMENT: dev-plan

TF_VERSION: 1.7.5

TF_LOCK_ID: ${{ inputs.TF_LOCK_ID }}

secrets: inherit

TERMINAL PORTS

DEBUG CONSOLE

DLT5444@OFWCXVDPC120159 MINGW64 ~/Desktop/work/ngpos-core-secrets-sync (feature/modules_secret_sync_isa)

$ git push origin feature/modules_secret_sync_isa ee66a7a. 439018d feature/modules_secret_sync_isa -> feature/modules_secret_sync_isa

DLT544480FWCXVDPC120159 MINGW64 ~/Desktop/work/ngpos-core-secrets-sync ( $ cd..

feature/modules_secret_sync_

DLT544480FWCXVDPC120159



ngpos-core-secrets-sync > .github > workflows > ! tf_plan_pr-dev.yml 1

name

: "[dev] Terraform Plan (on PR)"

2

on:

10

11

pull_request_target:

types:

opened

synchronize

reopened

ready_for_review

branches:

- develop

paths:

"data/***

"terraform/modules/***

"terraform/workspaces/dev/***

22

23

24

pull_request:

types:

opened

synchronize

reopened

ready_for_review

25

26

branches:

v.yml

27

- develop

-".github/workflows/tf_plan_pr-dev.yml"

od.yml

28

.yml

paths:

age.yml

30

jobs:

33

tf_plan_pr:

34

35

uses: ./.github/workflows/tf_plan_pr.yml

36

with:

I

# uses: krogertechnology/cs-cloudops-gha/.github/workflows/tf_plan_pr.yml@v1

In


jobs:

tf_plan_pr:

# uses: krogertechnology/cs-cloudops-gha/.github/workflows/tf_plan_pr.yml@v1

with:

uses: ./.github/workflows/tf_plan_pr.yml

WORKING_DIRECTORY: ./terraform/workspaces/dev/

ENVIRONMENT: dev-plan

TF_VERSION: 1.7.5

TF_FORWARD_SECRETS: true

TF_DRAFTPR_SKIPREFRESH: true

secrets: inherit

43

I


ngpos-core-secrets-sync >

.github

> workflows > !

tf_drift_cron-dev.yml

data.tf M

variables.tf M

name: "[dev] Terraform Drift Detection"

1

2

3

on:

4

5

67

9

10

11

12

13

14

15

16

17

18

workflow_dispatch:

schedule:

cron: '15 3** # runs nightly at 3:15 am

jobs:

tf_drift_cron:

# uses: krogertechnology/cs-cloudops-gha/.github/workflows/tf_drift_cron.yml@v1

uses: ./.github/workflows/tf_drift_cron.yml

with:

WORKING_DIRECTORY: ./terraform/workspaces/dev/

ENVIRONMENT: dev-plan

TF_VERSION: 1.7.5

ISSUE_ASSIGNEE: angelh-kr

TF_FORWARD_SECRETS: true

secrets: inherit


ngpos-core-secrets-sync >

.github >

workflows > !

tf_apply_merge-dev.yml

1 name: "[dev] Terraform Apply (on merge)"

X

main.tf...\dev

data.tf M

variables.tf M

push:

5

branches:

6

- develop

paths:

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

"data/**"

"terraform/modules/**"

"terraform/workspaces/dev/***

workflow_dispatch:

jobs:

2

3

on:

4

tf_apply_merge:

# uses: krogertechnology/cs-cloudops-gha/.github/workflows/tf_apply_merge.yml@v1

uses: ./.github/workflows/tf_apply_merge.yml

with:

WORKING_DIRECTORY: ./terraform/workspaces/dev/

ENVIRONMENT: dev-plan

ENVIRONMENT_APPLY: dev-apply

TF_VERSION: 1.7.5

TF_FORWARD_SECRETS: true TF_OUTPUTS_AUTODOC: true

I

secrets: inherit



