current code


develop

+ Q

pardhasaradhi-kr Include secret in blacklist for testing✓

52b592d- 9 months ago

History

Q Go to file

dependabot.yml

terraform/workspaces

dev

_tfconfig.tf

main.tf

prod

_tfconfig.tf

qa

11 }

_tfconfig.tf

> stage

> test

.gitignore

CCM_MANIFESTO.md

Code

Blame

Raw

# Testing secrets sync module

# Beiggining with single hardcoded secret for sanity check

module "secrets_sync" {

source

= "github.com/krogertechnology/ngpos-core-tfmodules//ngpos-secret-sync"

resource_group_name

= "rg-lzcm-ngpos-ngpisa-dev-eus2"

key_vault

expected_prefix

"ngpo-isa-dev"

whitelist

"ngpos-isa"

blacklist

= ["ngpos-isa-cosmos-db-connection-string"]

= ["ngpos-isa-cosmos-db-connection-string"]

Q Search


tfconfig.tf file


Code

Blame

* Terraform version is managed in GitHub Actions Workflow files.

terraform {

required_version = ">= 1.7.5"

required_providers {

7

azurera = {

8

source

"hashicorp/azurera"

9

version="~> 4.10.0"

10

}

11

12

google = {

13

version "~> 6.12.0"

14

}

15

}

16

17

backend "azurers" {

18

container_name

"ngpos-core-secrets-sync-dev"

19

key

"env-dev.tfstate"

20

resource_group_name

"rg-cloudops-bootstrap-eastus2"

21

storage_account_name =

"krlzcmngposeu2"

22

subscription_id

"2912a3d7-4fae-4252-9f75-670d4c28b63a"

23

tenant_id

24

"8331e14a-9134-4288-bf5a-5e2c8412f074"

use oidc

true

25

use_azuread_auth

true

Q Search

tenant_id

- "8331e14a-9134-4288-bf5a-5e2ca

24

use_oidc

= true

25

use_azuread_auth

= true

26

}

27

}

28

29

# nextgenposnonprod

30

provider "azurerm" {

31

32

33

subscription_id = "9d7222ed-5588-4078-93bb-d9c6ed814622"

features {}

34

}

35

36

37

38

provider "google" {

project = "kr-9985-edgcmp-d"

}


