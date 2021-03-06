# aws-s3-baseline
A minimal baseline to check for insecure or public S3 buckets and bucket objects in your AWS Environment.

## Description

This [InSpec](https://github.com/chef/inspec) compliance profile verifies that you do not have any insure or open to public S3 Bucket or Bucket Objects in your AWS Environment in an automated way.

InSpec is an open-source run-time framework and rule language used to specify compliance, security, and policy requirements for testing any node in your infrastructure.

## Requirements

- [InSpec](http://inspec.io/) at least version 2.1
- [AWS CLI](https://aws.amazon.com/cli/) at least version 2.x

## Setting up AWS credentials for InSpec

InSpec uses the standard AWS authentication mechanisms. Typically, you will create an IAM user specifically for auditing activities.

- Create an IAM user in the AWS console, with your choice of username. Check the box marked “Programmatic Access.”
- On the Permissions screen, choose Direct Attach. Select the AWS-managed IAM Profile named “ReadOnlyAccess.” If you wish to restrict the user further, you may do so; see individual InSpec resources to identify which permissions are required.
- After generating the key, record the Access Key ID and Secret Key.

## Get started

The following attributes can be configured in an attributes file. More information about InSpec attributes can be found in the [InSpec Profile Documentation](https://www.inspec.io/docs/reference/profiles/).
 
```
# Description: List of buckets exempted from inspection.
exception_bucket_list: []

```

Bundle install required gems <br>
- `bundle install`

Before running the profile with InSpec, define environment variables with your AWS region and credentials.  InSpec supports the following standard AWS variables:

- `AWS_REGION`
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_SESSION_TOKEN` (optional)

In any AWS MFA enabled environment - you need to use `derived credentials` to use the CLI. Your default `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` will not satisfy the MFA Policies in AWS environments. 

The AWS documentation is here: https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html

A useful bash script for automating this is here: https://gist.github.com/dinvlad/d1bc0a45419abc277eb86f2d1ce70625

To generate credentials using an AWS Profile you will need to use the following AWS CLI commands.

  a. `aws sts get-session-token --serial-number arn:aws:iam::<$YOUR-MFA-SERIAL> --token-code <$YOUR-CURRENT-MFA-TOKEN> --profile=<$YOUR-AWS-PROFILE>` 

  b. Then export the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` and `AWS_SESSION_TOKEN` that was generated by the above command.

## Note

In this InSpec profile implementation, the `s3-objects-no-public-access` control iterates through and verifies every  objects in each bucket in your AWS Environment, thus its runtime will depend on the number of objects in your S3 Buckets.


## Usage

InSpec makes it easy to run your tests wherever you need. More options listed here: [InSpec cli](http://inspec.io/docs/reference/cli/)

```
# Clone Inspec Profile
$ git clone https://github.com/aaronlippold/aws-s3-baseline

# Install Gems
$ bundle install

# Set required ENV variables
$ export AWS_ACCESS_KEY_ID=key-id
$ export AWS_SECRET_ACCESS_KEY=access-key

# run profile locally and directly from Github
$ inspec exec /path/to/profile -t aws:// 

# run profile locally and directly from Github with cli & json output 
$ inspec exec /path/to/profile -t aws://  --reporter cli json:aws-results.json

```

### Run individual controls

In order to verify individual controls, just provide the control ids to InSpec:

```
$ inspec exec /path/to/profile --controls s3-buckets-no-public-access -t aws:// 
```

## Contributors + Kudos

- Rony Xavier [rx294](https://github.com/rx294)
- Matthew Dromazos [rx294](https://github.com/dromazmj)
- Aaron Lippold [aaronlippold](https://github.com/aaronlippold)

## License and Author


### Authors

- Author:: Rony Xavier [rx294@nyu.edu](mailto:rx294@nyu.edu)
- Author:: Matthew Dromazos [dromazmj@dukes.jmu.edu](mailto:mattdromazos9@gmail.com )
- Author:: Aaron Lippold [lippold@gmail.com](mailto:lippold@gmail.com)

### License 

* This project is dual-licensed under the terms of the Apache license 2.0 (apache-2.0)
* This project is dual-licensed under the terms of the Creative Commons Attribution Share Alike 4.0 (cc-by-sa-4.0)
