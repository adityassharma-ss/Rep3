Files

main

+

Q

Code

Q Go to file

5

6

7

8

ngpos-key-vault

9

data.tf

10

11

main.tf

12

output.tf

13

14

qradar.tf

15

variables.tf

16

ngpos-secret-sync

17

18

data.tf

19

main.tf

20

21

outputs.tf

22

variables.tf

23

.gitignore

24

README.md

25

26

27


ngpos-core-tfmodules/ngpos-secret-sync/data.tf

pardhasaradhi-kr Updated sync secrets module

Code

Blame

1

data "azurerm_key_vault" "source" {

2

name

3

resource_group_name

var.key_vault

var.resource_group_name

4

}

5

data "azurerm_key_vault_secrets" "all_secrets" {

key_vault_id= data.azurerm_key_vault.source.id

data "azurerm_key_vault_secret" "secret" {

7

8

}

9

10

11

12

name

13

14

}

for_each

toset(data.azurerm_key_vault_secrets.all_secrets.names)

each.key

key_vault_id = data.azurerm_key_vault.source.id


gpos-core-t/modules/ngpos-secret-sync/main.tf

+ pardhasaradhi-kr Added logic to prioritise not renamed secrets than renamed secrets

Code

Blame

1

locals {

# Filter secrets based on ngpos-sync-enabled tag (defaults to true if missing)

secrets_to_sync = {

for secret in data.azurerm_key_vault_secret.secret :

secret.name => {

source_name secret.name

target_name = lookup(secret.tags, "ngpos-sync-rename", secret.name)

10

}

if lookup(secret.tags, "ngpos-sync-enabled", "true") "true"

11

12

13

14

}

# Split secrets into renamed and not renamed categories

secrets_renamed = [

15

for secret key, secret in local.secrets_to_sync:

16

{

17

(secret.target_name) = {

18

source_name = secret.source_name

19

target_name secret.target_name

Blame

26

for secret key, secret in local.secrets_to_sync:

27

{

28

(secret.target_name) {

29

source_name secret.source_name

30

target_name secret.target_name

31

}

32

}

33

if secret.target_name secret.source_name

34

35

36

37

38

39

40

41

42

43

# Combine renamed and not-renamed lists, giving priority to not-renamed se

combined_secrets = flatten([local.secrets_renamed, local.secrets_not_renam

# Build the secrets map in the correct order

ordered_secrets merge([for secret in local.combined_secrets: secret]...

* Apply whitelist based on target names

secrets_filtered_by_whitelist = var.whitelist null local ordered_secre

44

for secret_key, secret in local ordered_secrets:

45

secret_key => secret

46

if contains(var.whitelist, secret_key)

47

}

48

Q

Search

Apply blacklist based on target names

secrets_filtered_by_blacklist var.blacklist null? local.secrets_filtered_by_white

51

for secret_key, secret in local.secrets_filtered_by_whitelist:

52

secret_key => secret

53

if Icontains(var.blacklist, secret_key)

54

}

55

56

57

58

59

# Apply expected prefix filter on target names

secrets_filtered_by_prefix = {

for secret_key, secret in local.secrets_filtered_by_blacklist:

secret_key => secret

60

if startswith(secret_key, var.expected_prefix)

61

}

62

63

64

# check if the custom secrets exists on key vault

custom_secrets = {

65

for secret_key, secret in var.custom:

66

secret_key => secret

67

if contains(data.azurerm_key_vault_secrets.all_secrets.names, secret source_name)

68

}

69

70

# Merge filtered secrets with custom secrets

71

final secrets merge/local secrets filtered by prefix local custom secrets)


resource "google_secret_manager_secret" "gcp_secrets" {

for_each

local.final_secrets

secret_id= each.key # The target_name from final_secrets

77

78

labels = {

79

"source" "${var.key_vault)---$(each.value.source_name}"

30

}

31

2

replication {

3

auto {}

4

}

5

}

resource "google_secret_manager_secret_version" "gcp_secret_version" {

for_each local.final_secrets

secret

google_secret_manager_secret.gcp_secrets[each.key].id

secret_data = data.azurerm_key_vault_secret.secret[each.value.source_name].valu

depends_on = [google_secret_manager_secret.gcp_secrets]

}

ngpos-core-tfmodules/ngpos-secret-sync/outputs.tf

pardhasaradhi-kr Added logic to prioritise not renamed secrets than ren.

Code

Blame

1 output "final_secrets" {

2

value = local.final_secrets

3

}


variable "resource_group_name" {

description = "(Required) The Azure resource group where the Key Vault r

type

1

2

3

4

string

nullable

5

}

6

7

false

9

10

variable "key_vault" {

description = "(Required) The name of the Azure Key Vault."

type

string

nullable

false

11

}

2

3

4

variable "expected_prefix" {

description = "(Required) The prefix expected in secret names."

5

type

string

6

nullable

false

}

variable "whitelist" {

description = "(Optional) list of secrets to include in the sync."

type

list(string)

nullable

true

Search

variable "blacklist" {

27

description = "(Optional) list of secrets to exclude from the sync."

28

type

list(string)

29

nullable true

I

30

default

= null

31

}

32

33

34

35

6

B

variable "custom" {

description = "(Optional) A map of custom behavior for secrets (adhoc/rena

type = map(object({

source_name = string.

}))

nullable = true

default = {}

}

