


63-158-0-142:packer-templates sdorsett$ vi templates/esxi-cloning_configuration.sh
63-158-0-142:packer-templates sdorsett$ vi templates/esxi.json
63-158-0-142:packer-templates sdorsett$ packer build templates/esxi.json
esxi55 output will be in this color.

==> esxi55: Downloading or copying ISO
    esxi55: Downloading or copying: file:///Users/sdorsett/Documents/packer/packer-templates/iso/VMware-VMvisor-Installer-5.5.0-1331820.x86_64.iso
==> esxi55: Creating virtual machine disk
==> esxi55: Building and writing VMX file
==> esxi55: Starting HTTP server on port 8482
==> esxi55: Starting virtual machine...
    esxi55: The VM will be run headless, without a GUI. If you want to
    esxi55: view the screen of the VM, connect via VNC without a password to
    esxi55: 127.0.0.1:5910
==> esxi55: Waiting 5s for boot...
==> esxi55: Connecting to VM via VNC
==> esxi55: Typing the boot command over VNC...
==> esxi55: Waiting for SSH to become available...
==> esxi55: Connected to SSH!
==> esxi55: Uploading puppet/modules/vagrantbaseconfig/files/vagrant.pub => /etc/ssh/keys-root/authorized_keys
==> esxi55: Provisioning with shell script: scripts/esxi-vmware-tools_install.sh
    esxi55: Installation Result
    esxi55: Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
    esxi55: Reboot Required: true
    esxi55: VIBs Installed: VMware_bootbank_esx-tools-for-esxi_9.7.0-0.0.00000
    esxi55: VIBs Removed:
    esxi55: VIBs Skipped:
==> esxi55: Provisioning with shell script: scripts/esxi-cloning_configuration.sh
    esxi55: diff: can't stat '/tmp/auto-backup.35215//etc/ssh/keys-root/authorized_keys': No such file or directory
    esxi55: Saving current state in /bootbank
    esxi55: Clock updated.
    esxi55: Time: 14:42:00   Date: 01/02/2015   UTC
==> esxi55: Gracefully halting virtual machine...
    esxi55: Waiting for VMware to clean up after itself...
==> esxi55: Deleting unnecessary VMware files...
    esxi55: Deleting: output-esxi55/564d2ab2-395b-a9ba-9c17-2fe36682237c.vmem
    esxi55: Deleting: output-esxi55/esxi55.plist
    esxi55: Deleting: output-esxi55/vmware.log
==> esxi55: Cleaning VMX prior to finishing up...
    esxi55: Unmounting floppy from VMX...
    esxi55: Detaching ISO from CD-ROM device...
==> esxi55: Compacting the disk image
==> esxi55: Running post-processor: vagrant-vmware-ovf
==> esxi55 (vagrant-vmware-ovf): Creating Vagrant box for 'vmware_ovf' provider
    esxi55 (vagrant-vmware-ovf): Deleting key: floppy0.present
    esxi55 (vagrant-vmware-ovf): Deleting key: ide1:0.filename
    esxi55 (vagrant-vmware-ovf): Setting key: floppy0.present = FALSE
    esxi55 (vagrant-vmware-ovf): Setting key: ide1:0.present = FALSE
    esxi55 (vagrant-vmware-ovf): Creating directory: output-esxi55/ovf
    esxi55 (vagrant-vmware-ovf): Starting ovftool
    esxi55 (vagrant-vmware-ovf): Reading files in output-esxi55/ovf
    esxi55 (vagrant-vmware-ovf): Copying: esxi55-disk1.vmdk
    esxi55 (vagrant-vmware-ovf): Copying: esxi55.mf
    esxi55 (vagrant-vmware-ovf): Copying: esxi55.ovf
    esxi55 (vagrant-vmware-ovf): Compressing: Vagrantfile
    esxi55 (vagrant-vmware-ovf): Compressing: esxi55-disk1.vmdk
    esxi55 (vagrant-vmware-ovf): Compressing: esxi55.mf
    esxi55 (vagrant-vmware-ovf): Compressing: esxi55.ovf
    esxi55 (vagrant-vmware-ovf): Compressing: metadata.json
Build 'esxi55' finished.

==> Builds finished. The artifacts of successful builds are:
--> esxi55: 'vmware_ovf' provider box: esxi55-vmware_ovf-1.1.box
