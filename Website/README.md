# Website

## Overview

Notes for [rpubus.org]

[rpubus.org]: https://rpubus.org


## Order Form

Presently the form is a static web page with javascrip that validats DOM elements and populates them from a JSON price list. It does not submit and can only be used to print (e.g., to PDF) and then that is sent in an e-mail from the customer.

https://rpubus.org/Order_Form.html

To-Do: I want the order form to pull telemetry (e.g., stock qty, price) from a place I can update with ease and keep track of when and who updates it. I think it could just be a JSON file on a Github repo that I pull (raw) into the browser as an object and then populate the DOM elements.

Once the order form is filled out, it will need manual validation to check that I have the parts in stock, and figure out the shipping.

With the shipping price known and stock checked; I will generate and upload a payment web page, it is an invoice which provides links to Paypal (or [Stripe] if I can sort that out) for payment.

[Stripe]: https://github.com/stripe/stripe-python

After the payment is received a receipt will be sent and added to the invoice online then the order will be shipped.


## Keep It Static 

One lesson I have to learn over and over is to keep my web site static. No matter what dynamic web thing I have tried, at home, from a hosted service, or in the cloud. It gets poked and prodded and then breaks. It seems there is no way to do a generic server-side interface with things like PHP that can stand up to the fuzzing that happens in the wild. I have been educated about this so many times that at some point it needs to stick. Static web pages are the only way to go. The modern browser can run the code that makes it look dynamic (e.g., javascript), and input telemetry from a verity of sources (e.g., JSON) that is provided from middleware with tight limits on its fuzz-able interface. The result means a robust server side (it is static), and the client needs no changes. 