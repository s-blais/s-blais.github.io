---
layout: post
title:      "Wrappin' up Rails"
date:       2020-05-11 23:56:38 +0000
permalink:  wrappin_up_rails
---


Wow, within the last couple hours, I recorded some walkthrough video content for my Rails project. I'm terrifically excited that I accomplished this, and possibly even more excited to soon start moving forward in the curriculum again!

Years ago, I was part of a team that implemented one of the earliest large-scale commercial electronic medical record systems. Even though that kind of product was quite new, the complexity of it was obvious (and formidable), and a lot of the mechanics of it were mysterious and magical (to me), even though I knew the front end and sysadmin functions like the back of my hand. While pondering different ideas for my Rails project, I thought back to that medical record system, and how it would be interesting to de-mystify my memories of that system by creating some similar functionality myself, with my new skills. 

Some thoughts about my project:

I love building vertically, as I described in my [previous post](https://s-blais.github.io/building_vertically). It's the way to go, for sure!

In addition to a join-table model (Prescription) that connected two models (Patient and Medication) in a many-to-many relationship, I created a self-join-table model (Interaction) for some functionality that I was determined to add, even if it was not required for the project. The most common example given for a self-join is friendships on social media platforms, where a table includes two foreign keys from the same model (in the Friendship model example, it would be two User ids). I self-joined Medication ids in the Interaction table, with an extra attribute of description. Wiring up this relationship correctly required some research, as it wasn't covered in the curriculum. I didn't quite find a 1:1 reference for what I was trying to accomplish, so I pieced it together a bit from [this Medium post](https://medium.com/better-programming/building-self-joins-and-triple-joins-in-ruby-on-rails-455701bf3fa7) and [this Stack Overflow post](https://stackoverflow.com/questions/957169/activerecord-relationships-for-a-join-table-linking-two-records-of-the-same-tabl#957542).

The most interesting thing for me was getting the Patient model to return an array of applicable Interaction objects for a Patient's prescriptions. I eventually got there! First, I returned an array of a patient's medication ids pulled from the patient's prescriptions. Then, using that array, I returned an array of all possible pairings of those medication ids. Then, selected an array of Interaction objects through comparison of *that* array to the Interaction table pairs. Then, passed an @interactions to the view. It felt really complex at first, but after distilling and refactoring, wasn't half what I had expected. 

Similar to my Sinatra project, I repeatedly restrained myself from adding extraneous attributes for the purposes of sticking to MVP. Off the top of my head, I could think of at least several, sometimes a dozen or more attributes to add to the models.

Before I got to setting up my User and Session controllers, by luck I saw an Stack Overflow post about using `SecureRandom` to generate attribute values to pass user model validation when said attribute isn't always necessary and a placeholder will do. I made a note of that, and used that approach to satisfy my user model's has_secure_password even when passwords aren't in play (that is, when a user is created via OmniAuth). No lab leading up to the project included *both* native username/password authentication and OmniAuth, so it required some finagaling to get them to happily coexist, but I made it work. I believe that Devise does this by design, but Devise was dropped from the community bootcamp curriculum, and I wanted to see if I could make it work myself (and I did!)

I could keep tweaking my app indefinitely, and I have a lengthy list of could-do's, but it's time to wrap it up and move on to JavaScript. I just looked on GitHub and I have over 100 commits on this project. Wow!

My favorite accomplishments of this project are getting the two types of User creation/authentication to coexist, and setting up the Interaction self-join relationships and figuring out how to match those interactions to a patient. The validations on the models took longer than I expected, but the final effect with the error handling/display and field_with_errors on the forms was definitely satisfying. 

Onward! 😷




