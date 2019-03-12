bootloader
==========

This role configures the bootloader (if present) including its kernel boot parameters. It does NOT install a bootloader, this is the job of the "os-installer".

The following bootloaders are supported:

- `grub2`
- `extlinux` (syslinux)

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
