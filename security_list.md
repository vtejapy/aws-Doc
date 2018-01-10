## Delete your root access keys.
Your root user should not have access keys assigned to it. Access keys are the keys used to access resources on AWS. These access keys also contain the rights to delete your account, which is why you don't want access keys allocated to your root account.

## Activate multi-factor authentication (MFA) on your account.
When you log on, you’ll need to provide a code that changes every 60 seconds. I use the Google authenticator for this, as it’s free, very easy to setup, plus it is easy to link to your installed Google authenticator using a QR code that is displayed on the screen.

Note: Do keep a copy of the QR code somewhere safe. If you need to reinstall the authentication app, you’ll need the same QR code for it to generate the same codes.

## Create individual IAM users.
IAM users are the users in your AWS account that are assigned a subset of rights. If you create a user and give it “All access” rights, then it’ll have rights to do anything EXCEPT delete your root account.

## Set up user security groups to assign permissions.
Assign permissions to groups then put users into those groups. IAM is very granular, so if you want to limit permissions for specific users, it may need a bit of testing and tuning.

## Apply an IAM password policy.
The password policy AWS provides can be tweaked to fit your own requirements. You can do all the usual things, such set the amount of previous passwords that are remembered, if a password should have numbers, uppercase letters, lower case letters, and so on.
