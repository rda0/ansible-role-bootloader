bootloader
==========

This role configures the bootloader (if present) including its kernel boot parameters. It does NOT install a bootloader, this is the job of the "os-installer".

The following bootloaders are supported and auto-detected:

- `grub2`: enabled when `/etc/default/grub` is present
- `extlinux` (syslinux): enabled when `/syslinux.cfg` is present

custom host/group based inventory parameters
--------------------------------------------

For example to add custom boot parameters and a serial console speed of `57600`:

```yaml
bootloader_serial_console_speed: '57600'
bootloader_kernel_parameters_custom:
  - 'foo=bar'
  - 'baz=raboof'
```

boot parameters from other roles
--------------------------------

All roles that require the setting of special kernel boot parameters need to depend on this role and supply a list of parameters to this role via the dependencies `vars` attribute. This role (`bootloader`) needs to be enhanced to include this list of parameters. For an example of such a role, please refer to the role `hypervisor` and the variable `bootloader_kernel_parameters_hugepages` in role `bootloader`.

inventory config
----------------

The variable defaults in `defaults/main.yml` are tailor made for the `production` inventory, which has the serial console enabled by default, no grub password and a 5 second grub timeout.

For the `workstations` inventory, these variables are overidden in `group_vars/all/bootloader.yml`. The grub prompt is hidden (to show grub, press `SHIFT` during boot) and password protected.

The scheduler parameter `elevator=noop` is intentionally not included in the default configuration. It should only be included when it makes sense (Areca Controller and Kernel <= 4.19). It is therefore set via `host_vars`. I think in the future, we should aim for configuring the io scheduler via udev rules, because the newer multiqueue schedulers `mq_*`, cannot anymore be configured via kernel parameters. For an example see `inventories/production/host_vars/phd-web/base.yml` where the io scheduler must be set due to Areca Controller sending the wrong disk information to the Kernel.

The special kernel parameter for grub `lvmwait=/dev/vg0/root` is not included by default. This parameter should not be required for future installations. It can only have an effect, if the disk where the `grub.cfg` is located, is on an lvm volume. This parameter was required for some old Kernels.

All KVM virtual machines (except some exceptions) are using the `extlinux` bootlaoder, which is already configured in the defaults.

todo
----

### production

unreachable:

phd-san06
phd-san09
spaceml4
wifi-observer

### workstations

excluded:

- dumbledore

unreachable:

- amun
- astrodt3-rsg
- corredor
- galadriel
- gringotts
- hagrid
- hapimou
- heka
- lumos
- nefertem
- nut
- peking
- pf-pc12
- pf-pc16
- pf-pc34
- pince
- portus3
- sarapis
