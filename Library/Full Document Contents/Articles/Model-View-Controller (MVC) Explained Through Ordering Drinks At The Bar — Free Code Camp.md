# Model-View-Controller (MVC) Explained Through Ordering Drinks At The Bar — Free Code Camp

![rw-book-cover](https://cdn-media-1.freecodecamp.org/images/1*VRDZ0Xh-gOEJlAm1h7UMOg.jpeg)

## Metadata
- Author: [[Kevin Kononenko]]
- Full Title: Model-View-Controller (MVC) Explained Through Ordering Drinks At The Bar — Free Code Camp
- Category: #articles
- Publication date: 2016-05-02
- URL: https://medium.freecodecamp.com/model-view-controller-mvc-explained-through-ordering-drinks-at-the-bar-efcba6255053
- Summary: https://medium.freecodecamp.com/model-view-controller-mvc-explained-through-ordering-drinks-at-the-bar-efcba6255053#.vcygtwprj

# Notes

[[Kevin Kononenko - Model-View-Controller (MVC) Explained Through Ordering Drinks At The Bar — Free Code Camp]]

***

![Model-View-Controller (MVC) Explained Through Ordering Drinks At The Bar](https://cdn-media-1.freecodecamp.org/images/1*VRDZ0Xh-gOEJlAm1h7UMOg.jpeg) 
###### If you have been to a bar, then MVC ain’t that hard.

Model-view-controller (MVC) frameworks are a crucial part of building modern web applications. Walk into a room of web developers, and you will likely be bombarded with mentions of Ruby on Rails, Angular or Django.

More generally, MVC logic can be used to describe almost any web development process that uses a language like PHP, Ruby, Python or JavaScript.

###### Never the less…

Many web developers navigate this mysterious world by hacking through the weeds with a smile on their face. When a senior developer or teammate needs to look at the code from one of these developers, they will give an immediate yelp, followed by a swift lecture on common coding practices.

This is no way to go through life! In fact, the MVC pattern in modern web development can be easily explained by ordering a drink from a bartender. And yes, that means if you have been to a bar, then you can understand the major structural pattern shared by all web apps.

![Image](https://cdn-media-1.freecodecamp.org/images/1*f0mX-pFfwmoQKGdIUQoXEQ.jpeg)
*Bravely hacking through the obstacles until reality hits*

**What is the MVC Pattern?**

* **Model**: Structures your data in a reliable form and prepares it based on controller’s instructions
* **View**: Displays data to user in easy-to-understand format, based on the user’s actions
* **Controller**: Takes in user commands, sends commands to the model for data updates, sends instructions to view to update interface.

Or, in diagram form:

![Image](https://cdn-media-1.freecodecamp.org/images/1*4SxbmCrI5YVp1Uyj1Jstsg.png)
*Image Cred: Real Python*

That was boring. Onto the bar.

###### A beginner web developer enters a bar…

You enter a bar on a Friday night, and approach the bartender. Since the bar is already crowded, you push through a crowd until you finally catch the bartender’s attention, and you blurt out, “One Manhattan, please!”

You are the **user**, and your drink order is the **user request**. To you, the Manhattan is just your favorite drink, and you pretty reliably know that this will be a sweet and delicious drink.

The bartender gives you a quick nod. To the bartender, the Manhattan is not a tasty drink, it is merely a series of steps:

1. Grab glass
2. Add whiskey
3. Add vermouth
4. Add bitters
5. Stir drink
6. Add cherry
7. Ask for credit card and charge.

![Image](https://cdn-media-1.freecodecamp.org/images/1*AMWKKh4SxqNrcM5dNnSsfQ.jpeg)
*Image Credit: Wikipedia*

The bartender’s brain is the **controller**. As soon as you say the word “Manhattan” in a language that they understand, the work begins. This work is similar in nature to making a margarita or strawberry daiquiri, but uses distinct ingredients that will never be confused. The bartender can only use the tools and resources that are behind the bar. This limited tool set is the **model,** and includes the following:

* Bartender’s hands
* Shakers/mixing equipment
* Liquors
* Mixes
* Glasses
* Garnishes

Perhaps at a fancier bar, they might have a robot assistant! Or an automatic drink mixer. It does not matter to your particular bartender, who can only use the available resources.

Finally, the finished drink that you can see and consume is the **view.** The view is built out of the limited options from the model, and arranged and transmitted via the controller (that is, the bartender’s brain).

##### **Lessons Learned**

* Want another drink? Shouting at your empty glass, the view, will do you absolutely no good. You must talk to the bartender.
* The time spent between the bartender hearing the request and starting to create the drink should be absolutely minimal. This is sometimes known as a “skinny controller”- in other words, the controller should contain a minimal amount of logic, and delegate as much as possible to the model. A great bartender will not only have recipes memorized, but will also prepare the ingredients and tools in a reliable manner every night so that a minimal amount of searching and arranging is needed once the customers start ordering.
* Could the bartender pour all the ingredients directly in the customers mouth and expect the customer to swish it around and mix the drink? Yes, possibly I suppose. You want to keep as much of your logic within the model as possible as opposed to within the view. In other words, making the drink behind the bar is preferable to mixing it within the customer’s mouth.

![Image](https://cdn-media-1.freecodecamp.org/images/1*fpP-3F2rQUApiAx5Hm3H7w.png)
*Image Credit: Xperience*

* If you order a beer, the bartender will hardly need to do anything. Perhaps they will simply remove the cap and hand you the drink. That being said, you still must request the bartender. The beer will not magically appear in front of you.

##### Tying It Back To Web Development

Here is how the same process plays out in a modern web app:

* The user makes a **request** along a route, let’s say /home.
* The **controller** receives this request and gives a specific set of orders that are related to that route. These instructions could either be for the **view** to update or serve a certain page, or for the **model** to perform specific logic. Let’s assume this request has some logic associated with it.
* The model carries out the logic, pulls from a database and sends back a consistent response based on the controller’s instructions.
* The controller then passes this data to the view to update the user interface.

Whenever a request comes in, it first must go to the controller before it can be converted into instructions for the view or model. The [Ruby on Rails wikipedia article](https://en.wikipedia.org/wiki/Ruby_on_Rails#Technical_overview) contains a further overview if you are looking for more.

Any time you need to learn a new web development framework, you will come across this consistent MVC pattern. And if a particular framework differs from this, you can be sure that the authors will explain their new pattern with references to MVC.

This should make learning a heck of a lot easier- once you develop with MVC once, every new framework can fit within your comfort zone.

**Did you enjoy this guide? Let me know in the comments!**

ADVERTISEMENT

ADVERTISEMENT

ADVERTISEMENT

ADVERTISEMENT

ADVERTISEMENT

ADVERTISEMENT

ADVERTISEMENT

If you read this far, thank the author to show them you care. 

Learn to code for free. freeCodeCamp's open source curriculum has helped more than 40,000 people get jobs as developers. [Get started](https://www.freecodecamp.org/learn/)

ADVERTISEMENT
