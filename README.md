Files

<

ngpos-core-secrets-sync/terraform/workspaces/dev/main.tf

develop

+ Q

pardhasaradhi-kr Include secret in blacklist for testing✓

52b592d- 9 months ago

Histo

Q Go to file

terraform/workspaces

dev

Code

Blame

1

11

# Testing secrets sync module

# Beiggining with single hardcoded secret for sanity check

source

_tfconfig.tf

main.tf

module "secrets_sync" {

"github.com/krogertechnology/ngpos-core-tfmodules//ngpos-secret-sync"

prod

_tfconfig.tf

qa

resource_group_name

"rg-lzcm-ngpos-ngpisa-dev-eus2"

key_vault

"ngpos-isa-dev"

}

expected_prefix

"ngpos-isa"

_tfconfig.tf

whitelist

= ["ngpos-isa-cosmos-db-connection-string"]

stage

blacklist

= ["ngpos-isa-cosmos-db-connection-string"]

_tfconfig.tf

test

.gitignore

CCM_MANIFESTO.md
