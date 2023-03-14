    Team: APAC Development 
    Created By: Utkarsh Patil 
    Release Date: 14th March, 2023


node-red-node-umail
===================

<a href="http://nodered.org" target="info">Node-RED</a> nodes to send and receive simple emails.

Pre-requisite
-------------

You will need valid email credentials for your email server. For GMail this may mean
getting an application password if you have two-factor authentication enabled.

**Note :** Version 1.x of this node requires **Node.js v8** or newer.

Install
-------

You can install by using the `Menu - Manage Palette` option, or running the following command in your
Node-RED user directory - typically `~/.node-red`

        cd ~/.node-red
        npm i node-red-node-umail

GMail users
-----------

If you are accessing GMail you may need to either enable <a target="_new" href="https://support.google.com/mail/answer/185833?hl=en">an application password</a>,
or enable <a target="_new" href="https://support.google.com/accounts/answer/6010255?hl=en">less secure access</a> via your Google account settings.</p>

Usage
-----

Nodes to send and receive simple emails.

### Input node

Fetches emails from an IMAP or POP3 server and forwards them onwards as messages if not already seen.

The subject is loaded into `msg.topic` and `msg.payload` is the plain text body.
If there is text/html then that is returned in `msg.html`. `msg.from` and
`msg.date` are also set if you need them.

Additionally `msg.header` contains the complete header object including
**to**, **cc** and other potentially useful properties.

### Output node

Sends the `msg.payload` as an email, with a subject of `msg.topic`.

The default message recipient can be configured in the node, if it is left blank it should be set using the `msg.to` property of the incoming message. You can also specify any or all of: `msg.cc`, `msg.bcc`, `msg.replyTo`, `msg.inReplyTo`, `msg.references`, `msg.headers`, or `msg.priority` properties.

In the previous email node, `msg.password` didn't exist and wasn't defined anywhere,  and `msg.from` was inoperative when it was passed in the function node as it was not parameterized.

For emails to be sent, the node requires email authentication, for which we need to enter **emailId** and **password** into the fields present in email node.
We needed to pass the **password** and sender's **emailId** manually in the email node everytime.

Now the node is enhanced with the parameterization of the msg.from and msg.password which now will be `msg.user` and `msg.pass` respectively.

In addition we have also enhanced the existing email node with the response output which will forward the message/status to the corresponding outputs to debug node.

The `msg.user` and `msg.pass` can be set as **userid** and **password** respectively which was being entered in the email node manually.

The umail *from* can be set using `msg.user` and the sender's email id can be stored in the function node itself instead of umail node.

Simailarly umail *password* can be set using `msg.pass` and the password can also be stored in the function node.  

The email *from* can be set using `msg.from` but not all mail services allow
this unless `msg.from` is also a valid userid or email address associated with
the password. Note: if `userid` or msg.from does not contain a valid email
address (userxx@some_domain.com), you may see *(No Sender)* in the email.

The payload can be html format. You can also specify `msg.plaintext` if the main payload is html.

If the payload is a binary buffer then it will be converted to an attachment.

The filename should be set using `msg.filename`. Optionally
`msg.description` can be added for the body text.

Alternatively you may provide `msg.attachments` which should contain an array of one or
more attachments in <a href="https://nodemailer.com/message/attachments/" target="_new">nodemailer</a> format.

If required by your recipient you may also pass in a `msg.envelope` object, typically containing extra from and to properties.

If you have own signed certificates, Nodemailer can complain about that and refuse sending the message. In this case you can try switching off TLS.

Use secure connection - If enabled the connection will use TLS when connecting to server. If disabled then TLS is used if server supports the STARTTLS extension. In most cases set this to enabled if you are connecting to port 465. For port 587 or 25 keep it disabled.

This node uses the *nodemailer* npm module.
