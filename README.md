Creates:
* s3 bucket to host site content 
* route53 A record for site
* acm cert 
* route53 TXT record for acme dns challenge for acm cert
* cloudfront distribution for s3 bucket
* creates cloudfront origin access identity, permissions s3 bucket for it (so bucket doesn't need to be public).

Also uses null provider to exec shell commands to build react app.  

Requires:
* a domain in Route53 AWS Console (done in sandbox already by eks terraform)
* aws creds
* app specific build commands need added to local-exec providers in the null_resource, if you're using different app than that provided in ~/my-app/ dir.

Todo:
add remote s3 backend if you doing this via pipeline

Usage:
* edit dev.tfvars file to specify site url, zone, bucket name 
* `terraform init -var-file=dev.tfvars`
* `terraform apply -var-file=dev.tfvars`

The react app and most of the terraform came from Garrett Sweeney, and the cloudfront OAI and bucket policy using it from Milan Vit. 
Note that 2 aws providers are required, as cloudfront only sees ACM certs in us-east-1, so you have to create the cert using the us-east-1 provider.
