Azure Active Directory - leihs Authentication System
====================================================

This project contains code and deployment recipes to use [Azure Active
Directory](https://azure.microsoft.com/de-de/services/active-directory/) via
[OpenID Connect](https://de.wikipedia.org/wiki/OpenID_Connect) as an external
authentication system for [leihs](https://github.com/leihs).


Deployment
----------

The deployment uses [Ansible](https://docs.ansible.com/) and is expected to work 
with a recent version of Ubuntu LTS. The following
variables must be supplied via the 
[Ansible Inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html):

0. `adl_external_base_url`, the url without path and trailing under which this service is available from the web,
0. `adl_tennant`, see https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-protocols-openid-connect-code,
0. `adl_client_id`, see https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-protocols-openid-connect-code,
0. `adl_email_attribute`, the attribute in the supplied id token data which corresponds to the users email address, 
0. `adl_leihs_public_key`, the ES256 public as configured in the leihs instance for this service,
0. `adl_private_key`, ES256 private key of this deployed service,
0. `adl_public_key`, ES256 public key of this deployed service which must match the one configured in leihs for this service.


This service can be deployed on the same machine as leihs itself or on any
other internet host. The value of `adl_external_base_url` must be properly 
adjusted and also a corresponding value in leihs. In the case the service runs 
on the leihs host `reverse_proxy_custom_config` should probably include something like: 

    ProxyPass /authenticators/ms-open-id http://localhost:3434/authenticators/ms-open-id	nocanon retry=0

To start the deploy process invoke: 

    ansible-playbook -i $INVENTORY_HOSTS_FILE -l $TARGET_MACHINE deploy/deploy_play.yml

Development
-----------

See `Gemfile` and `Gemfile.lock`.



Notes and Links
---------------

https://login.microsoftonline.com/phzh.onmicrosoft.com/.well-known/openid-configuration


```
openssl ecparam -name prime256v1 -genkey -noout -out tmp/key.pem
openssl ec -in tmp/key.pem -pubout -out tmp/public.pem
```
