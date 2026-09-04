# terraform-provider-iptrack

Terraform provider for [`iptrack`](https://github.com/f0rkz/iptrack).

This provider manages:

- `iptrack_network`
- `iptrack_address`

and exposes matching ID-based data sources against the iptrack HTTP API.

## Usage

```hcl
terraform {
  required_providers {
    iptrack = {
      source  = "f0rkz/iptrack"
      version = ">= 0.1.0"
    }
  }
}

provider "iptrack" {
  endpoint = "http://127.0.0.1:8080"
}

resource "iptrack_network" "lab" {
  name = "lab"
  cidr = "10.20.0.0/24"
}

resource "iptrack_address" "router" {
  network_id = iptrack_network.lab.id
  ip         = "10.20.0.1"
  hostname   = "router.lab"
  status     = "assigned"
}
```

If `ip` is omitted from `iptrack_address`, the provider asks iptrack to allocate
the next free address atomically.

## Development

```sh
go test ./...
go vet ./...
go build .
```

For local Terraform development, add a `dev_overrides` entry for
`registry.terraform.io/f0rkz/iptrack` in your Terraform CLI config.

## Releases

This repository is intended to publish signed provider artifacts for the public
Terraform Registry from version tags like `v0.1.0`.

Release instructions are in [docs/RELEASING.md](./docs/RELEASING.md).
