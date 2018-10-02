# vSphere Automation - Welcome to hell

## Requirements
* [govc](https://github.com/vmware/govmomi/tree/master/govc) - vSphere CLI written in Go wrapping [govmomi](https://github.com/vmware/govmomi)
* [pass](https://www.passwordstore.org/) - Simple UNIX Password Manager
* [gpg](https://gnupg.org/) - GNU Privacy Guard utility for encrypting, signing keys
* [direnv](https://direnv.net/) - Folder scoped environment management. Uncluttering your `env`
## Setup
Setup your GO Path and install `govc`
`go get github.com/vmware/govmomi/govc`

Install `pass` and `gpg` with [Homebrew](https://brew.sh/)
`brew install pass gpg`

Generate a key with GPG
`gpg --full-generate-key`

Follow the prompts until you create a GPG-ID.  **Remember this GPG-ID**.

Setup your password-store
`pass init -p vcenter [GPG-ID]`

Add your vcenter login password
`pass insert vcenter/[GPG-ID]`
`[PROMPT] Enter password for vcenter/[GPG-ID]:`

Replace the appropriate values in your `.envrc` file
```
VCENTER_DOMAIN=[FQDN or IP]
VCENTER_USERNAME=[username]
VCENTER_PASSWORD=$(pass vcenter/[GPG-ID])

export GOVC_URL="https://$VCENTER_USERNAME:@$VCENTER_DOMAIN/sdk"

# Allowing Self-Signed Certs
export GOVC_INSECURE=1  
```

Test connectivity
`govc about`
