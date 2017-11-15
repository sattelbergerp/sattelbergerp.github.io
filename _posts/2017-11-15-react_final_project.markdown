---
layout: post
title:      "React Final Project"
date:       2017-11-15 13:16:44 -0500
permalink:  react_final_project
---



## 1. Logins
The first thing any app needs is a login system. I decided to use devise mostly because an existing windley used library will probably be more secure than something I made myself. It turns out its pretty simple to get devise to work with json. There are some blog that offer very complicated solutions. This [post](https://blog.andrewray.me/how-to-set-up-devise-ajax-authentication-with-rails-4-0/) offers a very simple and effective solution and anyone who has to do this should follow it. After that all you have to do is set up a form and submit a json request to the url for whatever you want to do. Also you have to pass the credentials: "same-origin" in the opetions object for every fetch request to remember the login session between requests.

## 2. Design

The flow of the application goes something like this
1. The client send a request to create or join a game. The server returns the current state of the game.
2. The client sends a "heartbeat" (GET /games/:id) once a second (up to the client) this will return the current game state, a timestamp and any messages created after the since perameter.
3. Chen a user clicks a a tile the client sends a POST /games/:id/turn containing the index of the tile. The server resposts with the current game state. If the board is full or a player has won the server will reset the board state and swap turns before responding.
4.  When a user leaves the client sends DELETE /games/:id if the client is either player1 or player2 the game will be deleted. Otherwise the server will return an error.

## 3. Chat
Chat is something i kinda wanted but it quickly turned into my biggest regret for the project. A large part of it is that chat was thrown in fairly quickly and therefore feels much less polished than the rest of the project.
Here is a list of issues i have with the way chat is implemented:

*  When you type a message you get no feedback the message just dissapears and then a second+ later it appears in chat
*  In some cases (Game does not exist, not signed in) the chat box will be shown and enabled but sending a message does nothing.
*  Chat is shown starting at the top instead of the bottom like most would expect.
*  Messages take longer than they should to show up and occasionally show up twice. This is because the timestamp only has second accuracy and not millisecond accuracy.

All of these have valid technal reasons to exist but they could also be fixed with some effort. The one I don't know how to fix is the timestamp one.


## 4. Annoying issues

1. Building the game board I ran isto a large number of issues. Initially I just rendered the tile letter ("X" for X, "O" for O and " " for no tile yet). However dav div with a space in it will be rendered/placed differently than a div with a space in it. My next idea was to use an svg however dynamically sized svgs seem to be impossible so I just gave up and had a fixed size game board. But wait theres more! When adding a fade in animation the animation would only be played when going wfrom no svg to an svg inside the div. But a div with and svg will be placed differently than a div without one. This leads to some tiles being placed below others as if they have a larger margin-top even though they don't. Oddly enough the solution to this is setting float:left on the divs. I assume it also fixes the text issue but I am not sure.
2. I wanted to add a mesasges to my games table when there are no games but I could not find a good way to do that. You would think it would be easy but I had a hard time getting my text to be centered in the entire table instead of just the first column. Eventually I just created an absolutly positioned div with a width of 100% (note you must set the table to position: relative for this to work).
3. There doesn't seem to be a good way to get a textarea to scroll to the bottom in readct without using document.getElementById
4. Error messages for devise routes are returned in differed way than errors form other routes.

Thanks for taking the time to read this.
