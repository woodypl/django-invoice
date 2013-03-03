:author: Tomas Peterka & Simon Luijk
:title: Django-invoice

Django Invoice
==============

General purpose invoicing app.

This app provides simple (but sufficient) Invoice model with export abilities.
The default export is into PDF. Model is fully configurable 
The exporter to PDF is open for extension. The current look of an exported invoice you
can see below. The tests generates one anyway, so you can  try on your own.

The model provides custom Address model via setting `INVOICE_ADDRESS_MODEL` and custom
Bank Account model via `INVOICE_BANK_ACCOUNT_MODEL`. The only rule for custom model
is that it has to have a method `as_text` which returns unicode string where newline
separators are `\n`. The string will be rendered as contractor resp. subscriber.

The Invoice model has optional BankAccount foreign key which has the same rules as 
Address model. It will be rendered below contractor information.

The invoice is intended to be referenced via foreign key from another model which handles
access policy and payments. These mechanisms are not provided in this app in favor for its
generality.

Invoice has some interesting methods:

**invoice.export_bytes()** - returns bytes of rendered invoice

**invoice.export_response()** - returns HttpResponse with the invoice as an attachment

**invoice.export_file(basedir)** - saves the invoice into a file in basedir and returns absolute path to the file

**invoice.export_attachment()** - returns MIMEApplication usable in email ::

    email = EmailMessage(to=[email, ], subject="Invoice", text="Hello")
    email.atttach(invoice.export_attachment())
    email.send()

Here we provide an example invoice generated from test

.. image:: example.jpeg
    :align: right
    :class: right




Export module
-------------

Provides base for Export class and PDFExport class which is the default 
export class in Invoice model. There is possibility to write your own exporter
and then simply use it within Invoice ::

    from myproject import MyExporter
    
    order = MyOrder.objects.get(pk=1)
    order.invoice.export = MyExporter()

    email = EmailMessage(to=[email, ], subject="Invoice", text="Hello")
    email.atttach(order.invoice.export_attachment())
    email.send()


