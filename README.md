Sample of HipChat notifications integration
===========================================

This sample demonstrates how to add HipChat build notifications to a Rails+Postgres application.

The integration with HipChat is done by calling its RESTful API. There are two versions of this
API that may be chosen from: the original, handled by `hipchat_notifier.py` script, and the
new, revised version 2 that `hipchat_notifier_v2.py` is prepared to work with.

The major difference between the versions is that for v1 only the group administrator is able to
create the access token and it is possible to set the sender username when posting messages to
a HipChat room. In version 2, every team member can create API token, but the messages are
always sent on his behalf.

The description below is equally applicable to both versions.
Please choose the correct script according to your needs.
Both scripts can be freely copied to your project to setup the integration.

The scripts require the following arguments:

* `--project` -- the human-readable name of your project
* `--room` -- name of your HipChat room to which you would like to post the notifications
* `--token` -- your HipChat API token, acquired in Account settings (for v2 of API) or Group admin panel (for v1)

By default, the build is marked as failure. To mark it as success, add `-s` option. You will most
likely prefer to save the values for parameters above as environment variables, with HipChat token
being an encrypted one:

    - PROJECT=sample-rubyonrails-postgres-heroku HIPCHAT_ROOM=Shippable-test
    - secure: i+9otey/FM/oxDtQ7Zp75elgi6KBi8GfWv6NBzfrCuEyO6eVxE0W2iFbsOvp+V7DT3v23hibMHnx/9pBMzfDdizntEAVDKT403AO4jB4Os/qxSKwAbt6K5NeNuRCCsaV5ejW+Q9+Wm4W+HSUqF3/Ugp6/EY7Qdz1QxDf6Ib6rAN+IDNpg53oEZmF2I6PjmJq8UyKB+sX+CphOvwwt1+4fw94gjVxpEbXB4CV0noe8qT+Noc0jiVdBMlRki8U3OrpNUObQgD1jtKHtuD2WP5E4s0iT88Vh0rU/CBCVAy7ytdpEZ2GpAvC0GFrIaE5KHaQqvvuZHzxLTo6WSTiciWgFg==

Then, you can add `after_failure` and `after_success` steps as follows:

    after_failure:
      - python hipchat_notifier.py --project $PROJECT --room $HIPCHAT_ROOM --token $HIPCHAT_TOKEN

    after_success:
      - python hipchat_notifier.py --project $PROJECT --room $HIPCHAT_ROOM --token $HIPCHAT_TOKEN -s

For more detailed documentation, please see:

* Shippable's continuous deployment section: http://docs.shippable.com/en/latest/config.html#continuous-deployment
* HipChat API documentation: https://www.hipchat.com/docs/apiv2

This sample is built for Shippable, a docker based continuous integration and deployment platform.
