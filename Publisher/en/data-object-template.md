# Data object: template variable

A variable that gives access to a unpersonalized **template**. 

You can edit the value of this variable and its properties under the "Tools" menu in 
the email designer.

## Available properties:

* **ID**: The ID of the template (Read-only)
* **name**: The name of the template (Read, write)
* **subject**: The subject of the templates (Read, write)
* **data**: See the documentation on [the data object](./data-object-data)

## Available methods

* **send(target)**:     send a mailing

The send method can be used to send this template object to a *target*. The target can
be a normal single destination - such as a profile or a subprofile - but also many destinations, 
such as an entire database or collection. The mail is currently scheduled to be sent immediately,
and will be treated as any other scheduled email to the target.

## Example

The following example in javascript can be used to access the subject of a template.

```javascript
var mySubject = template.subject;
```

Now for a more exciting example, say you have another template ready to send once the target has clicked the link. 
Sending the template using its *id* is as simple as the line below.

```javascript
copernica.template(*id*).send(destination);

// set global destination
copernica.template(templateID).send(destination)
```

## More information

* [The data-script object](./data-object-scripting)
* [The data object](./data-object-data)
* [Personalized template variable](./data-object-message)
* [Mailing variable](./data-object-mailing)
