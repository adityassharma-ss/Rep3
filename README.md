# Rep3

Secret sync for NGPOS-ISA

+

Description

Sync Azure KV secrets to GCP secret manager.

Ensure if Azure KeyVault for ISA (e.g. ngpos-isa-dev) is present or not

Create Keyvault (if not present)

Store secrets required for sync. Get details from existing GCP secret manager

Note: All secrets with prefix ngpos-isa need to be stored in KeyVault for sync

Create module for each environment to sync secrets to GCP

Reference - ngpos-core-secrets-sync/terraform/workspaces/dev/main.tf at develop. krogertechnology/ngpos-

core-secrets-sync

Validate the creation of secrets on GCP secret manager

Acceptance Criteria

Azure key vault exists with secrets

Creation of secrets in GCP secret manager

Additional Information

Apps

None

rojects

Linked work items

has the initiative

NGPOS

NGROS

NGPOS

is cloned by

NGPOS

NGPOS-23258 Secret sync for NGPOS-Platform

OPEN

NGPOSI

Data Pro

Issue Matrix

View all

Activity

Behavioral

Comments All

History

Work log

Transitions

Reopenings History

Hierarchy

4

Open

4 Improve work item

Pinned fields

Click on the next to a field label to start pinning.

NGPOS-17474 Analyze Secrets for GCP Sync and Renaming Needs

CLOSED

X

Details

Unassigned

Assignee

Assign to me

Kumar, Naresh (NonEmp)

Reporter

Create branch

Development

Create commit

View all pr

Add a comment...

Thanks...

Labels


