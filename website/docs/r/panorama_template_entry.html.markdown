---
layout: "panos"
page_title: "panos: panos_panorama_template_entry"
sidebar_current: "docs-panos-panorama-resource-template-entry"
description: |-
  Manages a specific device in a Panorama template.
---

# panos_panorama_template_entry

This resource allows you to add/update/delete a specific device in a Panorama
template.

This resource has some overlap with the `panos_panorama_template`
resource.  If you want to use this resource with the other one, then make
sure that your `panos_panorama_template` spec does not define any
`device` blocks, and just stays as "computed".

This is the appropriate resource to use if you have a pre-existing template
in Panorama and don't want Terraform to delete it on `terraform destroy`.

An interesting side effect of the underlying XML API - if the template does
not already exist, then this resource can actually create it.  However, since
only the single entry for the specific serial number is deleted, then a
`terraform destroy` would not remove the template itself in this situation.


## Import Name

```
<template>:<serial>
```


## Example Usage

```hcl
# Example for a virtual firewall.
resource "panos_panorama_template_entry" "example1" {
    template = "my template"
    serial = "00112233"
}

# Example for a physical firewall with multi-vsys enabled.
resource "panos_panorama_template_entry" "example2" {
    template = "my template"
    serial = "44556677"
    vsys_list = ["vsys1", "vsys2"]
}
```

## Argument Reference

The following arguments are supported:

* `template` - (Required) The template name.
* `serial` - (Required) The serial number of the firewall.
* `vsys_list` - (Optional) A subset of all available vsys on the firewall
  that should be in this template.  If the firewall is a virtual firewall,
  then this parameter should just be omitted.
