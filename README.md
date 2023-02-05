# eFnotify

This command is simply forked from [https://github.com/vlevit/eFnotify](https://github.com/vlevit/eFnotify)



#Usage

## Usage

eFnotify has all command line options of notify-send with a few
additional ones:

    Usage:
      eFnotify [OPTION...] <SUMMARY> [BODY] - create a notification

    Help Options:
      -?|--help                         Show help options

    Application Options:
      -u, --urgency=LEVEL               Specifies the urgency level (low, normal, critical).
      -t, --expire-time=TIME            Specifies the timeout in milliseconds at which to expire the notification.
      -f, --force-expire                Forcefully closes the notification when the notification has expired.
      -a, --app-name=APP_NAME           Specifies the app name for the icon.
      -i, --icon=ICON[,ICON...]         Specifies an icon filename or stock icon to display.
      -c, --category=TYPE[,TYPE...]     Specifies the notification category.
      -h, --hint=TYPE:NAME:VALUE        Specifies basic extra data to pass. Valid types are int, double, string and byte.
      -o, --action=LABEL:COMMAND        Specifies an action. Can be passed multiple times. LABEL is usually a button's label. COMMAND is a shell command executed when action is invoked.
      -d, --default-action=COMMAND      Specifies the default action which is usually invoked by clicking the notification.
      -l, --close-action=COMMAND        Specifies the action invoked when notification is closed.
      -p, --print-id                    Print the notification ID to the standard output.
      -r, --replace=ID                  Replace existing notification.
      -R, --replace-file=FILE           Store and load notification replace ID to/from this file.
      -s, --close=ID                    Close notification.
      -v, --version                     Version of the package.


So, for example, to notify a user of a new email we can run:

    $ eFnotify --icon=mail-unread --app-name=mail --hint=string:sound-name:message-new-email Subject Message

To replace or close existing message first we should know its id. To
get id we have to run eFnotify with `--print-id`:

    $ eFnotify --print-id Subject Message
    10

Now we can update this notification using `--replace` option:

    $ eFnotify --replace=10 --print-id "New Subject" "New Message"
    10

Now we may want to close the notification:

    $ eFnotify --close=10

Use `--replace-file` to make sure that no more than one notification
is created per file. For example, to increase volume by 5% and show
the current volume value you can run:

    $ eFnotify --replace-file=/tmp/volumenotification "Increase Volume" "$(amixer sset Master 5%+ | awk '/[0-9]+%/ {print $2,$5}')"

You can add a button to the notification with `-o` or `--default-action=`:

    $ eFnotify "Subject" "Message" -o "Show another notification:eFnotify 'new Subject' 'New Message'"

You can specify multiple actions by passing `-o` multiple times. Use
`-d` or `--default-action` for action which is usually invoked when
notification area is clicked. Use `-l` or `--close-action` for action
performed when notification is closed.

    $ eFnotify "Subject" "Message" \
        -d "eFnotify 'Default Action'" \
        -o "Button Action:eFnotify 'Button Action'" \
        -l "eFnotify 'Close Action'"
