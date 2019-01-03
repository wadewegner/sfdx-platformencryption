# sfdx-platformencryption

This is a simple example of using encrypted fields in a DE scratch org.

## Steps to try:

1) Create a new scratch org: `sfdx force:org:create -s -f config/project-scratch-def.json`

2) Push all the source into the scratch org: `sfdx force:source:push`

3) Assign the permission set granting you `ManageEncryptionKeys`: `sfdx force:user:permset:assign -n Encryption`

4) Create a tenant secret: `sfdx force:data:record:create -s TenantSecret -v "Description=test"`. Details on tenant secrets can be found [here](https://help.salesforce.com/articleView?id=security_pe_key_admin_creating.htm&language=en_US&type=0).

5) Open the file `force-app/main/default/objects/Account/fields/EncryptedText__c.field-meta.xml` and add a carriage return to mark the metadata as "dirty".

6) Push the updated `EncryptedText__c` custom field: `sfdx force:source:push`

7) Open the fields & relationships setup page for Account: `sfdx force:org:open -p one/one.app#/setup/object/Account/all/FieldsAndRelationships`

8) Confirm that the `EncryptedText` field has the encrypted field set.

That's it!

Note: the need to make a change to the `EncryptedText__c.field-meta.xml` file is not ideal. However, there is currently a "chicken & egg" problem. If you push all the source into the scratch org first, assign the permission set, then generate the tenant secret, your custom field will not have the encrypted attribute set. However, if you try to generate the tenant secret before you've pushed source (and assigned the permission set), you don't have access and it will fail. This is something we'll look into resolving.
