# Fedmsg GCM Demo

This is a demo (NON-PRODUCTION) of what it's like to use fedmsg in conjunction
with GCM (Google Cloud Messaging) for Android.

**This is totally Proof of Concept, and not for production!**

This uses Python, PyYAML and Requests to communicate with GCM over HTTP.

There's a new way of communicating with GCM which uses XMPP, which we could
look into using for production if we wanted -- which offers things like
asynchronous message sending, but for this demo I wanted to keep things simple.

# Registering for a GCM API key

Follow the **Creating a Google API project** section of this article:
http://developer.android.com/google/gcm/gs.html

Once you do that, you'll have an API key. Put that in `./config.yaml` under
the `api_key` field. You'll also have a Sender ID. Put that under the
`sender_id` field.

# Registering devices

In production, there would be a UI for registering devices. However, for our
demo purposes, we handle this in a YAML file manually.

Open Fedora Mobile and select **Fedmsg GCM Demo** from the navdrawer. This will
spawn a new Activity which will ask you for your **Sender ID** - Input that.
Once that is done, it will give you a **Registration ID** back, in the form of
a sprunge.us link. Go to the link and copy the string. You'll store this in the
config. To do this, open `config.yaml` and add an element to the `users` array.

It should contain `registration_id`, and an array called `topics` which are the
topics you wish to subscribe the device to.

# Subscribing to events

There's no filtering of any sort right now, except based on event keys. That
is, you can subscribe to `buildsys.tag` or `askbot.tag.update`, but not
messages containing only the word "firefox" or only events that "ralph"
created.

When
[fedmsg-notifications](https://github.com/fedora-infra/fedmsg-notifications)
exists, we'll have that flexibility, but this is just meant to be a quick demo.

Once you've registered your device and added it to `config.yaml`, do the
following:

* **TODO: Make this not required** - Edit gcmconsumer.py to correct the path to
  your YAML config.

* Exceut the following commands:

```bash
sudo yum install fedmsg-hub
sudo cp config.py /etc/fedmsg.d/gcmconsumer.py
python setup.py egg_info
PYTHONPATH=$(pwd) fedmsg-hub
```

# License

This project is released under the Apache Software License version 2.0.

See `LICENSE` and `NOTICE` for more information.
