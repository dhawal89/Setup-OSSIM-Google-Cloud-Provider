### Setup Commands for AlienVault OSSIM Virtualbox image on Google Cloude

`dhawal@EDI:~/Documents/Test_VM/av-ossim|
*⇒  tar -cSzf dhawalossim-5.tar.gz disk.raw*`


`dhawal@EDI:~/Documents/Test_VM/av-ossim|
*⇒  gsutil cp dhawalossim-5.tar.gz gs://dhawalossim/dhawalossim-5.tar.gz*`
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

`dhawal@EDI:~/Documents/Test_VM/av-ossim|
*⇒  gcloud*`
Usage: gcloud [optional flags] <group | command>
  group may be           app | auth | components | compute | config |
                         container | dataflow | dataproc | datastore |
                         deployment-manager | dns | iam | organizations |
                         preview | projects | service-management | source |
                         sql | topic
  command may be         docker | feedback | help | info | init | version

For detailed information on this command and its flags, run:
  gcloud --help


`dhawal@EDI:~/Documents/Test_VM/av-ossim|
*⇒  gcloud compute images create av-ossim --source-uri gs://dhawalossim/dhawalossim-5.tar.gz*`
