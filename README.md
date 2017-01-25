## Setup Commands for AlienVault OSSIM Virtualbox image on Google Cloude

Step1:
-------

Install the iso of OSSIM in Virtual box as per Normal Procedures only,
Have to configure these things when you create VBox VM.

1. Create VM make sure your disk size is 10, 20,30,40 not as (10.5GB)=> Not accepted by GCP. 
2. Create VM with HDD format type of QCOW recommended for GCP.
3. Disable the Floopy disk drive.
4. Disable the Audio as not needed in GCP.
5. Configure Netowrk Adaptar Type : `Paravirtualized Network (virtio-net)` recommended for GCP.

And Install and Update you VM OS in virtualbox.

Step2:
-------

Configure GRUB Loader as per GCP Requirements

1. Connect to terminal on VM system ossim which can be done my Jailbraking system option.
2. Edit the GRUB config file. Usually this file is at `/etc/default/grub`, but on some older distributions it might be located      in a non-standard directory.
3. Make the following changes to the GRUB config file:
   - Remove any line that has splashimage=. Compute Engine does not support splash screens on boot.
   - Remove the rhgb and quiet kernel command line arguments.
   - Add console=ttyS0,38400n8d to the kernel command line arguments so that the instance can function with the Interactive        Serial Console.
4. Regenerate the grub.cfg file. Use one of the following commands depending on your distribution.
   - Debian and Ubuntu: `sudo update-grub`
   - RHEL, CentOS, SUSE, openSUSE: `sudo grub-mkconfig -o /boot/grub2/grub.cfg`
5. Edit the /etc/fstab file and remove references to all disks and partitions other than the boot disk itself and partitions      on that boot disk. Invalid entries in /etc/fstab can cause your system startup process to halt.

Step3:
-------

After you configure the bootloader Create and compress the disk image.

If you prepared your system in a VirtualBox environment, you can use the VBoxManage tool to convert a .vdi or .qcow2 disk image to disk.raw format.

1. Shut down the VirtualBox guest machine that you want to import. You can shut down the guest machine with the VirtualBox        interface or the VBoxManage utility.

    `#:~VBoxManage controlvm [GUEST_NAME] acpipowerbutton`
    
    where [GUEST_NAME] is the name of the guest machine.
2. Convert the guest image to RAW format with the VBoxManage utility:

    `#:~ VBoxManage clonehd [GUEST_IMAGE] \ ~/disk.raw --format RAW`
    
    where [GUEST_IMAGE] is the path to the guest image .vdi or .qcow file.
3. Compress the raw disk into tar.gz format. This step compresses the image file so that you can more quickly upload it to        Google Cloud Storage. On OSX, install gtar and use it for this step instead of tar.
    
    `#:~/Documents/Test_VM/av-ossim|
     ⇒gtar -Sczf dhawalossim-5.tar.gz disk.raw`

Step4:
-------

The image file is compressed and ready to upload to Google Cloud Storage.

`#:~/Documents/Test_VM/av-ossim|
⇒  gsutil cp dhawalossim-5.tar.gz gs://dhawalossim/dhawalossim-5.tar.gz`

Copying file://dhawalossim-5.tar.gz [Content-Type=application/x-tar]...
==> NOTE: You are uploading one or more large file(s), which would run          
significantly faster if you enable parallel composite uploads. This
feature can be enabled by editing the
"parallel_composite_upload_threshold" value in your .boto
configuration file. However, note that if you do this large files will
be uploaded as `composite objects
<https://cloud.google.com/storage/docs/composite-objects>``_,which
means that any user who downloads such objects will need to have a
compiled crcmod installed (see "gsutil help crcmod"). This is because
without a compiled crcmod, computing checksums on composite objects is
so slow that gsutil disables downloads of composite objects.

| [1 files][  1.7 GiB/  1.7 GiB]    2.8 MiB/s                                   
Operation completed over 1 objects/1.7 GiB.`

`#:~/Documents/Test_VM/av-ossim|
⇒  gcloud`

Usage: gcloud [optional flags] <group | command>
  group may be           app | auth | components | compute | config |
                         container | dataflow | dataproc | datastore |
                         deployment-manager | dns | iam | organizations |
                         preview | projects | service-management | source |
                         sql | topic
  command may be         docker | feedback | help | info | init | version

For detailed information on this command and its flags, run:
  gcloud --help


`#:~/Documents/Test_VM/av-ossim|
⇒gcloud compute images create av-ossim --source-uri gs://dhawalossim/dhawalossim-5.tar.gz`

`#:~/Documents/Test_VM/av-ossim|
⇒ gcloud instances create ossim-server --image dhawalossim --machine-type f1-micro --zone asia-east1-c`

For Serial Port enable while creating Instance on GCE:

`gcloud compute instances create ossim-test4 --zone asia-east1-c --image ossim-4d --machine-type f1-micro --metadata=serial-port-enable=1`

Now connect via serial Console to Instance:
`gcloud beta compute --project "omise-experimental" connect-to-serial-port "ossim-test2" --zone "asia-east1-c"`
