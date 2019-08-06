This is a small utility that makes it easier to use the `aws sts assume-role` command:

## Installation

On OS X, the best way to get it is to use homebrew:

```bash
brew install remind101/formulae/assume-role
```

If you have a working Go 1.6 environment:

```bash
$ go get -u github.com/remind101/assume-role
```

## Configuration

The first step is to setup "aliases" of the roles to assume, this is done in a yaml formatted configuration file in `~/.aws/roles`.

**Example**

```yaml
prod:
  role: arn:aws:iam::1234:role/SuperUser
  mfa: arn:aws:iam::5678:mfa/eric-holmes # Enable MFA for this role. Note that this should be the MFA device in the account that you're assuming FROM, not the account you're assuming TO.
stage:
  role: arn:aws:iam::9012:role/SuperUser
```

## Usage

Perform an action as the given IAM role:

```bash
$ assume-role stage aws iam get-user
```

The command provided after the role will be executed with the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` and `AWS_SESSION_TOKEN` environment variables set.

If the role requires MFA, you will be asked for the token first:

```bash
$ assume-role prod aws iam get-user
MFA code: 123456
```

If no command is provided, `assume-role` will output the temporary security credentials:

```bash
$ assume-role prod
export AWS_ACCESS_KEY_ID="ASIAI....UOCA"
export AWS_SECRET_ACCESS_KEY="DuH...G1d"
export AWS_SESSION_TOKEN="AQ...1BQ=="
export AWS_SECURITY_TOKEN="AQ...1BQ=="
# Run this to configure your shell:
# eval $(assume-role prod)
```

## TODO

* [ ] Cache credentials.

Test
