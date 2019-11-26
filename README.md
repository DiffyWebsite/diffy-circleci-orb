# Diffy CircleCI Orb

[Diffy](https://diffy.website) is a visual regression testing platform for websites.


Repository contains code for Diffy's [CircleCI Orb](https://circleci.com/docs/2.0/orb-intro/).

CircleCI registry https://circleci.com/orbs/registry/orb/diffy/diffy.

Orb provides a job to compare Pantheon's DEV environment with Multi Dev. This job is a replacement of
BackstopJS visual regression job in [Pantheon Build Tools workflow](https://pantheon.io/docs/guides/build-tools). 

Once you have set up a project by using [Drops 8 workflow](https://github.com/pantheon-systems/example-drops-8-composer) you can use
Diffy to run visual regression testing.

Accept "[Allow Uncertified Orbs](https://circleci.com/docs/2.0/orbs-faq/#using-3rd-party-orbs)" under your Organization Security settings in CircleCI.
This can be done in `https://circleci.com/gh/organizations/YOUR_USERNAME_OR_ORGNAME/settings#security`

Configure CircleCI environment variables. Check [documentation page](https://diffy.website/documentation/pantheon-build-tools).

Once variables are configured you need to edit your `.circleci/config.yml` file.

Add an orb (place it at the top of the file)
```yaml
orbs:
   diffy: diffy/diffy@0
```

Then declare Diffy's job in jobs section
```yaml
diffy_visual_regression_test: diffy/compare_multidev_dev
```

And last -- add a new job in workflows (you can remove standard `visual_regression_test` step from workflow too).

```yaml
- diffy_visual_regression_test:
  requires:
    - configure_env_vars
    - deploy_to_pantheon
  filters:
    branches:
      ignore:
        - master
```

Here is how your result config.yml will look like
![Diffy CircleCI add custom Orb](img/diffy-orb-drops-8-build-tools.png)

## Development

To release the orb after making some changes you need to
```
circleci orb validate src/orb.yml
circleci orb publish increment src/orb.yml diffy/diffy patch
```