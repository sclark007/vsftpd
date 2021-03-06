# DESCRIPTION

Very Basic installation and configuration of vsftpd to support Secure (SSL) SFTP.

Uses pretty close to standard vsftpd.conf. Mainly turns on ssl and adds require_ssl_reuse=No.
Will use ssl certificates you specify in attributes and put into the files/default directory or files that are already installed on target server.


# REQUIREMENTS

Only tested on Ubuntu 10.04 and the vsftpd that comes from the standard apt sources at the time this cookbook was created. No attempt has been made to support other target platforms or versions of vsftpd.

# ATTRIBUTES

* vsftod[:chroot_local_user] - If set to 'YES', then the chroot_list_file (contents of vsftpd[:chroot_users]) will specify list of local users to NOT chroot their home directories. If set to "NO" the users in chroot_list_file will have their home directories chroot'd. Default: "YES"

* vsftpd[:chroot_users] - Optional list of local usernames to be put in the chroot_list_file (/etc/vsftpd.chroot_list) file. Default: empty

* vsftpd[:ssl_cert_path] - The pathname of the directory that the ssl cert file should live. Default: /etc/ssl/certs

* vsftpd[:ssl_private_key_path] - The pathname of the directory that the ssl private key file should live. Default: /etc/ssl/private

* vsftpd[:ssl_certs_basename] -Base name of the ssl cert PEM file and ssl private key filenames. Default: 'ftp.example.com' You will want it to be the FQDN of your host that you used to create the certs

* vsftpd[:use_ssl_certs_from_cookbook] - If set, you must have the ssl public and private cert files in the cookbook's files directory. Default: true

# USAGE

## SSL Certificates for secure ftp

You will need to have SSL public certificate end up in the directory as specified by vsftpd[:ssl_cert_path] with the certificate basename specified by vsftpd[:ssl_certs_basename] and the suffix of .pem (defaults to /etc/ssl/certs/ftp.example.com.pem)

The SSL private key must end up in the directory as specified by vsftpd[:ssl_private_key_path] with the certificate basename specified by vsftpd[:ssl_certs_basename] and the suffix of .key (defaults to /etc/ssl/private/ftp.example.com.key)

You can use an SSL certificate public/private files that are not being managed by this cookbook by updating the vsftpd ssl attributes to match the paths/filenames that are on your server. In this case you must set vsftpd[:use_ssl_certs_from_cookbook] to false

Or you can put the public/private files with the basename and suffix described above in the cookbook's files/default directory. You can use the chef-repo's rake ssl_cert command to create the files and then move them into the cookbook's files/default directory with the proper names. vsftpd[:use_ssl_certs_from_cookbook] must be true (default)

You should not use the example ftp.example.com.{pem,key} that are in files/default. They are there just for an example and testing.

## Extra Configuration

You can tweak the templates/default/vsftpd.conf to configure other aspects of vsftpd. Most of them are the default.

# LICENSE and AUTHOR:

Author:: Robert J. Berger (rberger+maintainer@ibd.com)

Copyright:: 2010, Robert J. Berger

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
