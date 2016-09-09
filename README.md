# Slack chat plugin for Redmine

This plugin posts updates to issues in your Redmine installation to a Slack
channel. Improvements are welcome! Just send a pull request.

## Screenshot

![screenshot](https://raw.github.com/sciyoshi/redmine-slack/gh-pages/screenshot.png)

## Installation

From your Redmine plugins directory, clone this repository as `redmine_slack` (note
the underscore!):

    git clone https://github.com/sciyoshi/redmine-slack.git redmine_slack

You will also need the `httpclient` dependency, which can be installed by running

    bundle install

from the plugin directory.

Restart Redmine, and you should see the plugin show up in the Plugins page.
Under the configuration options, set the Slack API URL to the URL for an
Incoming WebHook integration in your Slack account.

## Customized Routing

You can also route messages to different channels on a per-project basis. To
do this, create a project custom field (Administration > Custom fields > Project)
named `Slack Channel`. If no custom channel is defined for a project, the parent
project will be checked (or the default will be used). To prevent all notifications
from being sent for a project, set the custom channel to `-`.

For more information, see http://www.redmine.org/projects/redmine/wiki/Plugins.

## Customized messages

In the settings page of the plugin, you can customize the messages sent using a
`sprintf` format string.
This setting is available for messages sent after issue creation and updates.
The sprintf method is invoked with hash argument, so you must used named references
in the format string.
The following parameters are available:
```
issue_tracker
issue_id
issue_subject
issue_url
issue_status
issue_project
issue_description
issue_author
issue_assigned_to
```

Additionally, the following parameters are also available after an issue update:
```
journal_user
journal_notes
```

You have the possibility to create a user custom field in Redmine named
`Slack user`. If such a field is present, a parameter named
`issue_assigned_to_slack_user` will also be available. It can be useful to add
a slack mention to the assigned user in the messages.

A sample issue created format string
```
@%<issue_assigned_to_slack_user>s (%<issue_status>s) <%<issue_url>s|%<issue_tracker>s #%<issue_id>s %<issue_subject>s> created by %<issue_author>s
%<issue_description>.100s...
```

A sample issue updated format string
```
@%<issue_assigned_to_slack_user>s (%<issue_status>s) <%<issue_url>s|%<issue_tracker>s #%<issue_id>s %<issue_subject>s> updated by %<journal_user>s
%<journal_notes>.100s...
```

For more information about sprintf formatting, see
https://ruby-doc.org/core-2.3.1/Kernel.html#method-i-sprintf or
http://idiosyncratic-ruby.com/49-what-the-format.html
