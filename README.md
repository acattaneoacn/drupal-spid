# Drupal SPID
Repository with all the information to configure SPID authentication in drupal 9.

## IMPORTANT
    composer install --ignore-platform-reqs

## Service Provider Configs
Those are the values used in the Drupal SPID module and in the certificate creation.
 - common-name: "Drupal SPID"
 - entity-id: "http://api.drupalspidbbb.local:8080" (must be the URL of your website)
 - org-id: "VATIT-12345678901" (use this, there are some rules about this value)
 - org-name: "Drupal SPID"
 - sector: private (tested only for private, more tests needed for public)

## Create Certificate and Private key
https://github.com/italia/spid-compliant-certificates (not tested)

https://github.com/italia/spid-compliant-certificates-python

FOR PYTHON:

    cd spid_cert/
    pip install spid-compliant-certificates
    spid-compliant-certificates generator --key-size 3072 --common-name "TEST SPID" --days 365 --entity-id http://api.drupalspidbbb.local:8080 --locality-name Roma --org-id "VATIT-12345678901" --org-name "Test Drupal Spid" --sector private

This command create 3 files: crt.pem, csr.pem, key.pem inside the dir spid_cert/.

## Configure drupal SPID module

The module is already installed (cim).
For the configuration of the user fields go to: /admin/config/people/accounts/fields.
You need to create at least 2 fields:
- CF
- mail

And all the other fields you want to save from the SPID.

For the configuration of the module go to: /admin/config/people/spid.
- Entity ID: entity-id (same of before)
- Certificate file path: ../spid_cert/crt.pem
- Certificate key file path: ../spid_cert/key.pem
- IDP metadata folder: ../IDPS_metadata/
- Organization name: Drupal SPID (same of before)
- Organization display name: Drupal SPID

Then maps the field of the user with the field from the SPID. (CF and mail required)

Go to: /admin/config/people/spid/metadata_idp. And click 'Download metadata'.

## Configure test env
Clone this repository in a local folder.
    git clone https://github.com/italia/spid-testenv2.git

Open 'Dockerfile' and replace libffi6 with libffi7.

Open file 'conf/sp_metadata.xml.example'.
Replace its content with '/admin/config/people/spid/metadata_sp'.

The build docker image:
    docker build -t italia/spid-testenv2 .




