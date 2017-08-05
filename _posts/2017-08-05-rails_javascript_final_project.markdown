---
layout: post
title:  "Rails + Javascript final project "
date:   2017-08-05 19:19:22 +0000
---


[github](https://github.com/sattelbergerp/rails-project-tracker) [video](https://youtu.be/CgQ7pJDbeZA)

I started with my tasks index page. In the original project there were two routes: /tasks and /tasks/overdue. The first listing all tasks and the second listing tasks past their complete by date. The first thing I did was add a ?filter= parameter so I could add a filter dropdown later. The only other thing of note i9s that I spent about five minutes trying to figure out why activemodel serializers wouldn't work before realizing I din't restart the server because I had it running in another tab.

Next I had to make a page where the information was populated by javascript that had a next button. Now the when we implemented a next button in the labs it had a pretty major issue. It would simply fetch the resource with an id one greater than the current resource. However say you have 3 resource with ids 1, 2, and 3. Then you delete the resource with the id of 2. What happens when you view the resource with the id of 1 and click next? It will fail and your app will think you are at the end of the list. I wanted to fix this so my app sends a filed called next_id and a field called prev_id. If there is no next or previous project the filed will be null and the app can hide the next or previous button accordingly. Unfortunatly there is no easy way to get the next item in activerecord. The simplest way seems to be to simply iterate over the the id and stopping if it exists or is greater than #maximum(:id).

The project show page has a text field for the name of the task to add to the project. This was pretty much build for javascript amd I already had an easy way to generate HTML for a task. One issue is that I had a Task.buildHTML() method that reutrned a string of html too add to a list. This would be fine but I wanted to add an event to a button so the task could be deleted in the background. Even though it is not convetion I decident to use onclick= over having to do $('#my-button').click() everytime I add a task.

Also two major things changed after I made the video: I added a simple andimation to the tasks list and I remebered that I needed to change the url of the edit and delete buttons when next or previous was click (This is why tests exist).
