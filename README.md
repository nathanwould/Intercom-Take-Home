# Intercom Take-Home

## 1. Describe a coding solution you're most proud of.

I think the pieces of code I’m the most proud of are from my final project at General Assembly: a React on Rails application aiming to be the start of a fake POS system for the last Blockbuster. In most of my projects I made things difficult for myself by coming up with ideas that involved complicated forms, but with each project I made more improvements over my first attempt. 
In this app the form is used to add a new movie to the database or update an existing one - the same form is used for both functions, it’s just pre-populated with the data of an existing movie if you’re editing it. It collected the movie’s title, year of release, directors and featured actors with options to add fields for multiple directors and actors, which was pretty straight-forward on the front end, but since my database contained separate tables for Movies, Directors, and Actors, I wanted to avoid making 3 API calls to add a movie. So instead, the front end makes a post or put request submitting all of the information as one packet, “Form Data,” to the Movies controller, which checks to see if the information already exists, and if not adds each item to its respective table.

## 2. Troubleshooting a 504 error code.

## 3. Debugging exercise.

## 4. Helping a customer solve their root issue.

## 5. Tell us about a time where you didn't know what to do.
