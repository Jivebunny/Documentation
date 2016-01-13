# Secure email

With SMTPeter you can set up sender domains and private security keys to
enable receiving servers and mailbox providers (like Gmail, Yahoo, Hotmail,
et cetera) to verify that your emails are actually sent by you (and not
by a spammer or malicious fisher). SMTPeter comes with an easy to use
and powerful dashboard to configure this.

Why do you need this? For that to understand, you must know that email was
traditionally not designed to prevent _bad guys_ from sending out emails
pretending to be sent by you. The email protocol was invented a long time
ago, when the internet was only used by researchers as a tool to send
messages to each other. It was designed to be simple, and did not have
checks or verification mechanisms to ensure that it was not misused.
Because of this, it is up to this very day still very easy to change the
_from_ address of a mailing, and send out emails just as if they were
sent by someone else. Spammers and fishers abuse this.

Fortunately, new technologies have been invented to solve this shortcoming,
technologies with names like DMARC, DKIM and SPF. These technologies are
here to help you. If you configure your domain and DNS records correctly,
you can be pretty sure that others can no longer send out email out of
your name. However, these technologies not only make it harder for spammers
and fishers to send out email, but it makes it more difficult for legitimate
senders (like you!) to send email as well. SMTPeter can help you out.


## DNS Records

The DKIM, SPF, and DMARC technologies use the domain name system (DNS) to
secure email. DNS is traditionally the technology to convert human readable
domain names (like yourcompany.com) into numeric IP addresses that are 
understood by computers. This system has been enhanced to not only convert
domain names into IP addresses, but also to answer questions about who
is allowed to send email using specific domains. If you own one or more
domains, you can simply add a couple of extra records to your DNS
servers (or ask your provider to do this for you) so that you can send
legitimate email.

SMTPeter provides guidance in how the change your DNS records in order 
to use the above mentioned technologies. In the dashboard of SMTPeter
you can find forms and wizards to generate DNS records that you can
copy into your name server configuration.


## Signing your emails

With SMTPeter you can automatically sign your emails. This allows receiving
parties to verify if the mail was indeed sent by you and that the content
of the email (including attachments) was not modified during transport.

This signing technology is based on the mechanism of related private and 
public keys. Your public keys are published in the DNS records of your domains
(and can therefore be seen by everyone in the world), while the private
key that are needed to sign your messages are privately stored on the SMTPeter
servers. This technology ensures that mails can only be signed by you (if
you send your mails through SMTPeter), and that they can be verified by
everyone else (because everyone has access to the public key to check
the signatures). This makes it impossible for spammers, fishers, or anyone 
else to sign mails out of your name, simply because no one else has access to 
your private key.


## Generating keys

When you create a sender domain, a DKIM key is automatically generated for you.
You simply have to copy and paste these records from the SMTPeter dashboard into
your own DNS server (or the DNS tool of your provider) and you're ready to go. 

Do you already have private and public key pairs, and do you now want 
SMTPeter to use these? No problem, get in touch with us and we will fix
this for you.


## Important: automatic DKIM keys rotation!

Your private keys are stored on the SMTPeter servers and are never exposed.
The technology behind the public and private keys is very secure, yet, if 
someone spends a lot of time on it, the keys can be broken. Therefore, you 
do want to update your keys every now and then.

SMTPeter automatically generates every six months a new DKIM key for you.
Instructions are send to you by email and are, ofcourse, also available
in the SMTPeter's dashboard. After a while, you are notified when it is
safe to remove the old DKIM key from your DNS.


## Setting up your sender domain

Via the dashboard on SMTPeter.com you can add the domain names from
which you normally send out email. For example, if you normally send out
email from the domain name "example.com", you can enter this domain in the
SMTPeter dashboard. SMTPeter will start up a wizard that asks you some
questions. After you have answered these questions SMTPeter will show you
step by step how to adjust your DNS records for setting up DMARC, DKIM and
SPF. If you copy-and-paste these DNS records from the SMTPeter.com dashboard
into your DNS configuration tool, your email platform is ready for use.

Is that all you have to do? No, sadly it is not. The moment you have copied
and installed the DNS records according to the suggestions made by SMTPeter,
your DNS records immediately list SMTPeter.com
as the one and only party from which "example.com" emails may be sent.
This is a very safe setup and makes it very hard for fishers and spammers
to send out mails from your domain, but you must be absolutely sure that
from that moment on, all your own legitimate mail is really routed via the
SMTPeter.com gateway. This is not only a requirement for your commercial
newsletters, but also for your transactional emails (like order confirmations
and password reminders) and all the regular office mails sent by you
and your colleagues! So, you have to set up all your applications, websites
and email clients (don't forget the mobile devices) to send email via
SMTPeter.com.

If you do not want to reconfigure all your applications to send out mail via SMTPeter.com
you have two options. Firstly, you can modify the suggested DNS records,
so that they also include the data of your regular email servers. Secondly,
you could use SMTPeter.com only for sending mails from a certain _subdomain_.


## Incorporating your own servers

We mentioned that once you have copied the DNS records that are suggested by
SMTPeter.com to your DNS servers, all your mails must be sent out by
SMTPeter.com. This is true. However, you are free to edit the DNS record
suggestions and include your own settings in it too. If you do this, it
is possible to send out mail from both your own servers, and from the
SMTPeter.com servers.
For more information on the format of the DKIM, SPF, DMARC records and to
find out how you can modify them to include your
own IP and/or own DKIM keys, you can take a look at the official
specification of these technologies. You can find links to the
specifications near the bottom of this article.


## Using subdomains

If changing your entire mail architecture is too much of a hassle (for now)
and you also do not feel like manually editing DNS records, you can take a
third approach: you can use a (new) _subdomain_ for your email delivery that is
used by SMTPeter instead. For example, if your normal mail is sent out from the
"example.com" domain name, you can use SMTPeter.com to set up a sender
domain for a subdomain e.g. "newsletter.example.com". After this setup
you can use the SMTPeter.com service just for messages with a _from_ address that ends with 
"@newsletter.example.com". You still can use your current setup to send 
mails from example.com.

If you want to use this setup, you can create a sender domain via the dashboard
just like for a regular sender domain. When the wizard asks you the name
of your sender domain you use the subdomain "newsletter.example.com".
SMTPeter will present you the DNS records that you can
copy-paste into your DNS configuration tool so that your newsletters that are
sent out through SMTPeter.com are accepted by the mailbox providers. Your
newsletters must use a _from_ address of the form "something@newsletter.example.com".
This of course does not look as cool as a "something@example.com", but
it allows you to setup SMTPeter.com without changing the rest of your
email infrastructure. Obviously, if you use this approach, only mails
that end with "newsletter.example.com" can be verified by receiving mail servers. 
Mails ending with just "example.com" do not benefit from the increased
security.


<a name="slowrollout"></a>
## Slow rollout

By setting up a sender domain or by signing your emails a mistake is easily made. You do not
want to have all your emails blocked, simply because you made a typo in your DNS, or because
you forgot to send out a mail via SMTPeter.com.
Therefore, we recommend to always use a slow rollout, when you start using
sender domains or signing your emails.

The wizard that helps you to setup your sender domain will as a default
suggest that you slowly rollout the sender domain already. This default starts with a
setting in which all your emails should be accepted by receiving mail servers,
no matter whether the mails are sent by SMTPeter.com or not. Moreover,
a receiving mail server should notify SMTPeter that a mail was received
that was not sent by SMTPeter.com or not properly signed. SMTPeter will keep track on how
often these notifications occur and will provide reports with statistics on a regular
basis on your dashboard. This gives you the information to check
if you have made any mistakes or not. If you see a lot of notifications
about mails that were received but did not fulfill the promised requirements,
you can check for mistakes. You can rerun the wizard to see were you have
made a mistake in the DNS settings and you can check if you are sending
out emails via another server than SMTPeter.com.

If you are comfortable with the results in the reports, you can make some
reconfigurations, so receiving servers do not simply accept all your emails
anymore. This will increase the security of your emails, yet, there is also
a greater impact if you do make a mistake. Fortunately, changing the behavior 
is not an all or nothing setting, so you can gradually increasing the security
without increasing the impact of an error to much. You can basically configure two things:
(1) what do you want receivers to do with mails that are not sent out via
SMTPeter.com (we refer only to SMTPeter for simplicity. If you have manually
edited the DNS records to incorporate your own servers these should be
accepted as well), and (2) for what percentage of the failing mails do you
want this behavior?

What a receiver should do with your emails can be set with a policy.
There are three types of policies: "none", "quarantine", and "reject".
The most loose policy is "none". This means that all mails 
should be accepted, even the ones that were not sent via SMTPeter.com. This
is the policy that is advised by SMTPeter to start with.
"Quarantine" is more strict and indicates that you want mails that are not
sent via SMTPeter to be placed in the receiver's spam folder. Finally,
if you choose "reject", you instruct all receivers that your mails that
are not received from SMTPeter.com should be rejected. This is the most
strict policy. You should be aware that in reality some mailbox providers do
not respect the "none" policy and quarantine or reject such messages anyway.

Besides the policy you can specify a percentage of mails to which the
policy should be applied to. If you set the policy to "quarantine", and the
precentage to "10", it means that only 10% of all mails that do not
come from a SMTPeter.com server should be quarantined, and you want all
other mails to be accepted anyway. If you set the policy to "reject, and
the percentage to "5", it means that only 5% of all mails that do not
come form SMTPeter.com server should be rejected. The other 95% of the 
mails that do not come from SMTPeter.com will be quarantined in this case.

So, after having used the slow rollout setting suggested by SMTPeter
(policy "none" and percentage 100%) for a while and you are comfortable
with the results in the report you can increase the policy to "quarantine"
and set the percentage to e.g. 1%. The worst case with this setting is
that if something goes wrong 1% of the messages that are not sent via SMTPeter
will be put in the receiver's spam folder. If the reports indicate that
still everything is fine you can increase this 1% slowly to 100%. If you still
are satisfied with the results in the reports, you can switch to the "reject"
policy, starting at 1%, after which you can slowly increase it to 100% as
well. A safe deployment cycle resembles something like this:

| Domain policy | Percentage |
| ------------- | ----------:|
| Monitor       | 100%       |
| Quarantine    | 1%         |
| Quarantine    | 5%         |
| Quarantine    | 10%        |
| Quarantine    | 25%        |
| Quarantine    | 50%        |
| Quarantine    | 100%       |
| Reject        | 1%         |
| Reject        | 5%         |
| Reject        | 10%        |
| Reject        | 25%        |
| Reject        | 50%        |
| Reject        | 100%       |

Of course SMTPeter guides you on how the adjust your DNS records accordingly.


## What if you do not sign emails or set up a sender domain?

Signing emails or setting up a sender domain via the SMTPeter.com dashboard is _optional_.
If you do not sign emails or set up sender domains, all mails that you send through the
SMTPeter.com gateway are simply forwarded to the internet, using our IP addresses
and using our domain names - and many emails will probably just end up
where you want them to be: in the mailboxes of your recipients.

However, mailbox providers like Yahoo, Gmail, AOL and basically all the
others are getting more and more strict in applying the DMARC, DKIM and
SPF protocols, so we do recommend to spend some extra time on the
SMTPeter.com dashboard and with your DNS configuration. Adding these
extra records is very helpful.


## Some extra background: What is DKIM, SPF and DMARC?

In this document we have mentioned the DKIM, SPF and DMARC abbreviations
a couple of times. These are technologies that (1) allow a sender to
sign an email, so that nobody can modify mails, and receivers can verify
that a message was really sent by the person who claims to be the sender
(this is DKIM), a technology (2) to list the servers that have permission
to send out email for a certain domain (this is SPF), and (3) a technology
that receivers can use to find out what they should do in case the DKIM
or SPF checks fail (this is DMARC).

DKIM, SPF and DMARC all use the Domain Name System (DNS, the technology to
turn domain names into IP addresses, and to store other domain name related
information), and it is therefore up to you as the domain owner to add
records to your DNS configuration to enable this for your emails. Setting
up these DKIM, SPF and DMARC records can sometimes be troublesome. This
is exactly where SMTPeter helps you, because if you tell SMTPeter that
you want to send out mail from a certain domain, we will show you exactly
what type of DNS records you have to copy-and-paste into your DNS
configuration tool.

To learn more about these technologies, we recommend to take a look at
the offical protocol specifications:

* [RFC 6376 DomainKeys Identified Mail (DKIM)](https://tools.ietf.org/html/rfc6376)
* [RFC 7208 Sender Policy Framework (SFP)](https://tools.ietf.org/html/rfc7208)
* [RFC 7489 Domain-based Message Authentication, Reporting, and Conformance (DMARC)](https://tools.ietf.org/html/rfc7489)

You can also visit [dkim.org](http://www.dkim.org/), [openspf.org](http://www.openspf.org/), and [dmarc.org](https://dmarc.org/) for extra background
information on DKIM, SPF, and DMARC.