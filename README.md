# Profile Inheritance Example

This is an example InSpec profile that shows how profile inheritance works.

InSpec allows controls from other profiles to be included into another another profile. For example, profile "my-profile" can execute controls from "my-company-base-profile" anytime "my-profile" is executed.

In addition, controls from "my-company-base-profile" can be skipped or modified if needed. For example, if a control from "my-company-base-profile" does not apply to a particular host or application, it can be skipped completely. If a control that is normally considered critical if it fails is not actually critical for a particualr host or application, its impact can be modified.

## Defining Dependencies

All profiles from which controls are to be inherited must be defined in the `inspec.yml` file in the `depends` section:

```yaml
depends:
  - name: linux-baseline
    url: https://github.com/dev-sec/linux-baseline/archive/master.tar.gz
  - name: ssh-baseline
    url: https://github.com/dev-sec/ssh-baseline/archive/master.tar.gz
```

Once defined, the profile names, and the controls from these profiles, can be used within any control file in the including profile.

## Including Controls

From within a controls file in a profile, the `include_controls` and `require_controls` commands provide the ability to include all or specific controls from an included profile.

See the `controls/controls_from_other_profiles.rb` file in this example profile for more details and examples on how this works.

## Using Waivers

Available in InSpec verion 4.18.104 and later, Waivers fulfill a purpose of improving skipped controls by allowing the ability to provide a business justification for controls against which they are unable to be compliant. They can also specify an end date to track when a control should be remediated, or leave it blank to make the waiver permanent.  Below is an example format as well as InSpec CLI syntax to reference the waiver file.  When using the Audit cookbook the instructions can be found here: https://github.com/chef-cookbooks/audit#waivers

```yaml
os-03:
  expiration_date: 2030-12-31
  run: false
  justification: "Not needed until end of 2030 | Steve Monet - Principal Auditor, 2020-04-02"
```
To run via the CLI against a Linux target for which you have access with ssh, from the root of the repo:

`inspec exec . -t ssh://user@host --waiver-file waivers.yml`

## Need more info?

Swing by [inspec.io](https://www.inspec.io) for more information, including [more details on InSpec profiles](https://www.inspec.io/docs/reference/profiles/) and using inheritance.
