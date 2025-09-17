passtools-java
==============

Official Java SDK for the PassTools API


## Overview 
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Furbanairship%2Fpasstools-java.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Furbanairship%2Fpasstools-java?ref=badge_shield)


* The SDK makes it easy to manage Apple Passbook passes through the PassTools API.
* Please refer to the [API Doc](https://github.com/tello/passtools-api) for the raw apis.
* Indexed documentation for the Java SDK is available at [http://tello.github.com/passtools-java/](http://tello.github.com/passtools-java/).

## How to use the SDK


Let's say you wanted to create a personalized coupon for one of your customers. You would
* create a coupon template through the PassTools UI where you could for instance create secondary fields to capture the first and last name of your customer. Let's say those fields have the keys _first_name_, _last_name_.
* You then get the template ID from the template list page. Say the ID is 5.


From the java code, you first step is to set your API key to PassTools. You API key is supplied after you contact PassTools at help@passtools.com for early access to the API program.
Please build the code with _mvn clean install_ and load passtools-java-1.0.jar into your app from the target dir. (p.s: We are working on publishing the passtools-java library to public maven repositories and will update when done).

```java
PassTools.apiKey = "yourKey"
```

You then call _Template.get()_ to retrieve your newly created template
```java
Template template = Template.getTemplate(5L);
```

and set the 2 secondary fields for your customer "Marie Lie"

```java
JSONObject firstNameField = template.fieldsModel.get("first_name");
firstNameField.put("value", "Marie");

JSONObject lastNameField = template.fieldsModel.get("last_name");
lastNameField.put("value", "Lie");
         
```



You can then create Marie's pass 
```java
Pass coupon = Pass.create(template.getId(), template.fieldsModel);
```

And finally, you could either update the pass or download the pass and then email it to Marie
```java
JSONObject price = template.fieldsModel.get("price"); //assuming you set up the "price" in the UI template builder
price.put("value","$10"); //it was $20 default before

coupon.fields=template.fieldsModel;
Pass.update(coupon);//from the created coupon above

Pass.downloadPass(coupon.id,"MarieCoupon.pkpass");//the pass is downloaded locally to your file

//you can now deliver the pass to Marie by email, sms or through a hosted URL.

```


Please note that we are currently deploying the com.passtools.* maven artifacts to the the central repositories. If they do not currently show on search results, please build the project with "mvn install" in the mean time. Thanks.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request






## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Furbanairship%2Fpasstools-java.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Furbanairship%2Fpasstools-java?ref=badge_large)