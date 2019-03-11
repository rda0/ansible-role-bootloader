bootloader
==========

This role configures the bootloader (if present) including its kernel boot parameters. It does NOT install a bootloader, this is the job of the "os-installer".

The following bootloaders are supported:

- `grub2`
- `extlinux` (syslinux)

Note: This role tries to solve a complex problem. A common configuration suitable for most hosts via `role/defaults`, without the need of specifiyng any inventory configuration. Possiblility to add custom host/group based inventory parameters. And the possibility to specify variables from other roles that need to supply kernel boot parameters.

TODO:

- workstation setup (hidden grub, grub password)

custom host/group based inventory parameters
--------------------------------------------

For example to add the `noop` scheduler boot parameter:

```yaml
bootloader_kernel_parameters_custom:
  - 'elevator=noop'
```

boot parameters from other roles
--------------------------------

All roles that require the setting of special kernel boot parameters need to depend on this role and supply a list of parameters to this role via the dependencies `vars` attribute. This role (`bootloader`) needs to be enhanced to include this list of parameters. For an example of such a role, please refer to the role `hypervisor`.
