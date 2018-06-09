# ansible-cert-authority

This ansible playbook will allow you to setup a CA that is either a root or an intermediate.

Usage:
To setup a root CA with one intermediate CA:

git clone git@github.com:finalriposte/ansible-cert-authority.git
cd ansible-cert-authority
ansible-playbook -c local main.yml
To create a root CA only:
ansible-playbook -c local main.yml --tags root
Once the root CA is created you can create an interemediate CA only by using:
ansible-playbook -c local main.yml --tags intermediate

Files to edit before running the ansible-playbook command:
roles/common/vars/main.yml
roles/root/vars/main.yml
roles/intermediate/vars/main.yml

common:
  Creates the base folder that the root keys and all intermediates get installed into.
  
root:
  Creates the certs and details for the root CA
  
intermediate:
  Creates an intermediate CA and all its files
  
variables:
(common, root and intermediate)
ca_root_dir: the root of the CA structure, in my case I have used /opt/CA

(root and intermediate)
catype: to help specify the config file name.  Only used for root at this stage.
email: the email address associated with the certificate.
org: the org and org unit for the cert's Distinguished Name. (NOTE: create another variable if you want them to be different)
country: the two letter country code for the Distinguished Name.
state: the state code for the Distinguished Name.
loc: the location (i.e. city) for the Distinguished Name.
ca_key: the passphrase for the key.  KEEP THIS SECRET!
common: the common name for the Distinguished Name.

(intermediate only)
int_domain: The intermediate domain that you are creating the CA for.


!!!IT IS HIGHLY RECOMMENDED THAT YOU RUN VAULT ON THE TWO YML FILES WITH SECRETS!!!
ansible-vault encrypt roles/root/vars/main.yml
ansible-vault encrypt roles/intemediate/vars/main.yml
