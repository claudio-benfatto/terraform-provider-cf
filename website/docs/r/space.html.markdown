---
layout: "cf"
page_title: "Cloud Foundry: cf_space"
sidebar_current: "docs-cf-resource-space"
description: |-
  Provides a Cloud Foundry Space resource.
---

# cf\_space

Provides a Cloud Foundry resource for managing Cloud Foundry [spaces](https://docs.cloudfoundry.org/concepts/roles.html) within organizations.

## Example Usage

The following is a Space created within the referenced Org. All refeenced users must have been added as a member to the owning Org.

```
resource "cf_space" "s1" {
    name = "space-one"
    org = "${cf_org.o1.id}"
    quota = "${cf_quota.dev.id}"
    asgs = [ "${cf_asg.svc.id}" ]
    managers = [ 
        "${cf_user_org_role.tl.user}" 
    ]
    developers = [ 
        "${cf_user_org_role.tl.user}",
        "${cf_user_org_role.dev1.user}",
        "${cf_user_org_role.dev2.user}" 
    ]
    auditors = [ 
        "${cf_user_org_role.adr.user}",
        "${cf_user_org_role.dev3.user}" 
    ]
    allow_ssh = true
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) The name of the Space in Cloud Foundry.
* `org` - (Required) The ID of the [Org](/docs/providers/cloudfoundry/r/org.html) within which to create the space.
* `quota` - (Optional) The ID of the Space [quota](/docs/providers/cloudfoundry/r/quota.html) or plan defined for the owning Org.
* `allow_ssh` - (Optional) Allows SSH to application containers via the [CF CLI](https://github.com/cloudfoundry/cli)
* `asgs` - (Optional) List of running [application security groups](/docs/providers/cloudfoundry/r/asg.html) to apply to applications running within this space.
* `staging_asgs` - (Optional) List of staging [application security groups](/docs/providers/cloudfoundry/r/asg.html) to apply to applications being staged for this space.
* `managers` - (Optional) List of users to assign [SpaceManager](https://docs.cloudfoundry.org/concepts/roles.html#roles) role to.
* `developers` - (Optional) List of users to assign [SpaceDeveloper](https://docs.cloudfoundry.org/concepts/roles.html#roles) role to.
* `auditors` - (Optional) List of users to assign [SpaceAuditor](https://docs.cloudfoundry.org/concepts/roles.html#roles) role to.

## Attributes Reference

The following attributes are exported:

* `id` - The GUID of the Space

## Import

The current Space can be imported using the `space`, e.g.

```
$ terraform import cf_space.s1 a-guid
```
