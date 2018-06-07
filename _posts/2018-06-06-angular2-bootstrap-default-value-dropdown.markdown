---
layout: post
title: (Angular/Bootstrap) Selecting default value in dropdown
img: angular2.jpg # Add image post (optional)
date: 2018-06-06 22:00:00 +0300
description: Default Select value in model driven forms with dynamic data source
tag: [Angular, Bootstrap, Typescript]
---
In case anyone is facing a problem with setting the default value in &lt;select&gt;&lt;/select&gt; components, while connecting to a dynamic datasource (e.g. REST).

First the part when we subscribe to receive the data:

~~~
ngOnInit() {
    this.subscription = 
    this._contactService.getContactDetails(this.idContactDetail).subscribe(p => {
        this.contactDetails = p;
    },
    (err) => console.error(err),
    // this is where the magic happens
    () => this.contactForm.controls['address'].setValue(this.contactDetails.address, 
    	{onlySelf: true}));
}
~~~
{: .language-typescript}
\\
Obviously we can define default value for more than one control, as we did here with 'address'.

Now onto the view:

~~~~~
<label class="control-label" for="address">Link Type:</label>
<select id="address" class="form-control" formControlName="address">
  <option [ngValue]="null" [selected]="true" hidden disabled>Select address</option>
  <option *ngFor="let addr of contactAddresses" [ngValue]="addr">{{ addr }}</option>
</select>
~~~~~
\\
Here we can't see where contactAddresses variable is coming from, but we can assume it is a predefined list of addresses somewhere in the controller.

Now if everything works as intended, the 'Select address' option, being a default placeholder <b>should never be shown</b>, as we are selecting the default value from the list, while the component is initialized, after receiving the data. So we can safely get rid of it, to ultimately get the code as follows: 

~~~
<label class="control-label" for="address">Link Type:</label>
<select id="address" class="form-control" formControlName="address">
    <option *ngFor="let addr of contactAddresses" [ngValue]="addr">{{ addr }}</option>
</select>
~~~
