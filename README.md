# AWS-recipes

## Installation

1. Clone this repo
1. cd to this folder
1. git submodule init
1. git submodule update

## Session management tools

Because MFA-protected API access is currently not convenient to use for CLI
users, iSEC wrote several tools to help management of MFA-related credentials.
These scripts leverage the "standardized" way to manage credentials described
on the [AWS
Blog](http://blogs.aws.amazon.com/security/post/Tx3D6U6WSFGOK2H/A-New-and-Standardized-Way-to-Manage-Credentials-in-the-AWS-SDKs),
and build on top of it to facilitate integration with the Security Token
Service (STS) and MFA-protected API access.

### aws_configure.py

This tool works similarly to the CLI _aws configure_, but saves values in a
file under .aws/credentials.no-mfa, instead of the standard .aws/credentials.
It also allows users to configure their MFA serial token number, such that they
will no longer have to enter it every time they call the
Security Token Service (STS).

    ./aws_configure

It supports profile and allows creation of multiple profiles via calls like

    ./aws_configure --profile isec

### aws_init_session.py

This tool reads credentials configured in the .aws/credentials.no-mfa file,
prompts users for their MFA code, and retrieves STS credentials (key id,
secret, and session token). The short-lived credentials are then saved under
the "standardized" .aws/credentials file to be accessible to other tools.

    ./aws_init_session.py

After calling this function, users of the AWS CLI may just work as if
MFA-protected API access was not there:

    aws iam list-users

### aws_rotate_my_key.py

Because credentials rotation is important, and because it is always overlooked
by AWS users, iSEC created a tool that does it for you. Just run this tool and
a new access key will be generate and stored in your .credentials.no-mfa file.
Your old access key will be deleted.

