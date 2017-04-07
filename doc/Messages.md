This document keeps track of all the parent/child messages that Greasemonkey
passes.  All sections should look like:

    # MessageName
    Sent by: `whatever.js`

    Description of the purpose of this message, its important details, values
    within the data property and so on go here.

All message should specify `JSON.parse()`-able bodies, and always with a `name`
parameter for dispatching.  Additional values are documented per messsage name
below.

# ListUserScripts
Received by: `bg/user-script-registry.js`.

Lists all installed user scripts.  No data is sent.  Response data:

* An array of `.details` from installed `RunnableUserScript` objects.

# InstallProgress
Sent by: `bg/user-script-install.js`
Received by: `content/install-dialog.js`

While downloading a user script (and all dependencies), reports the current
progress as a percentage.  Sent specifically back to the content process
(tab / frame) which started the install.  Data:

* `errors` A (possibly empty) list of string error messages.
* `progress` A number, 0.0 to 1.0, representing the completion so far.

# UserScriptInstall
Sent by: `content/install-dialog.js`
Received by: `bg/user-script-install.js`

Triggered when the install button of the install dialog is clicked by the
user.  Data:

* `details` An object of values parsed from the `==UserScript==` section,
  as produced by `parseUserScript()`.

# UserScriptUninstall
Sent by: `content/manage-user-scripts.js`
Received by: `bg/user-script-registry.js`

Triggered when the Remove button of the manage user scripts dialog is clicked
by the user.  Data:

* `uuid` The UUID value of a script as returned by `ListUserScripts` message.

Response data:

* `null`, but presented upon async completion.