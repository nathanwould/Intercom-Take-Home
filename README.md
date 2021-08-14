# Intercom Take-Home

## 1. Describe a coding solution you're most proud of.

The pieces of code I’m the most proud of are from my final project at General Assembly. I created a React on Rails application for the last Blockbuster store to provide them with a new POS (point of sale) system that aimed to make the employees' lives easier by deprecating their outdated system and process for tracking inventory. Before tackling this project, my earlier projects involved lots of complicated forms, but with each project I was able to iterate on what I had learned previously and make small improvements over how I develop forms. 

In the Blockbuster app, a form is used to add a new `Movie` to the database or update an existing one. The same form is used for both functions (`create` and `update`), it’s just pre-populated with the data of an existing movie if you’re editing it. The form collected the movie’s title, year of release, directors and featured actors with options to add fields for multiple directors and actors. This was pretty straight-forward on the front end but because the database contained separate tables for `Movies`, `Directors`, and `Actors`, I wanted to avoid making 3 API calls to add one `Movie`. So instead, the front end makes a `post` or `put` request, submitting all of the information as one packet, `formData`, to the `Movies` controller, which checks to see if the information already exists, and if not adds each item to its respective table. I felt this was overall more efficient than solutions I had delivered previously and showed my growth as an engineer.

```
  # POST /movies
  def create
    # First, create a new instance of a Movie. Actors and directors 
    # are ignored because associations will be handled later.
    @movie = Movie.new(movie_params.except(:actors, :directors))
    # Then, see if actors in incoming movie params exist and if not, 
    # add them to the Actors table. The associated actors are saved in the @actors variable.
    @actors = movie_params[:actors].map do |actor|
      Actor.find_or_create_by(name: actor[:name])
    end
    # Repeat the same process for directors
    @directors = movie_params[:directors].map do |director|
      Director.find_or_create_by(name: director[:name])
    end
    # Then handle the associations.
    @movie.actors = @actors
    @movie.directors = @directors
    @movie.user = @current_user
    # Finally, save if successful and return json response to the front-end, or return error. 
    if @movie.save
      render json: @movie, status: :created, location: @movie
    else
      render json: @movie.errors, status: :unprocessable_entity
    end
  end
```

## 2. Troubleshooting a 504 error code.

First, I would collect whatever identification information is standard practice for Intercom to verify the client's identity as required. Since a `504` error would most likely be an issue with Intercom’s server rather than a client-side issue, I would check to see if there were any outages with Intercom’s servers on the System Status page. If the `504` were due to an issue with Intercom's servers, I would first make sure the relevant engineers were aware of the issue by flagging it in the appropriate channels so it could be addressed ASAP. I would then let the client know that our teams are aware of the incident and are working on a fix. I would share a link to the System Status page to let the client check in on any updates asychronously and apologize for any inconveniences this outage might cause for them. 

If there were no internal outages, I would then start gathering additional context from the client around the error to see what specific request caused the `504` error, and if the error came with any additional information. I would figure out if the error was happening in the browser by trying to reproduce it on my own machine or through another tool like Postman. If it was happening in the browser I would have the client clear the cache and refresh. Depending on their technical capabilities, I might send them a link to additional resources such as a knowledge base article to help accomplish this, or just tell them the hotkey for their operating system and browser.

If that didn’t resolve the issue, I would check the API routes the client was attempting to request to make sure there wasn’t an incorrect route causing the issue - Intercom’s error handling seems robust enough that a typo in a route would just fail and not cause a timeout but I think it’s always better to check. If the issue still persisted, I would likely reach out to my team for guidance or escalate the issue.

## 3. Debugging exercise.
```
###
# Problem Definition: This Ruby method should ensure 
# that the word "Twitter" is spelt correctly. 
###

# Original code to be debugged:

# def fix_spelling(name)
#   if name = "twittr"
#     name = "twitter"
#   else 
#     fix_spelling(name)
#   end   
#   return "name"
# end

# The `else` statement to be executed creates an infinite recursive loop if the `name` is anything
# other than `twittr`.

# My solution works properly without falling
# into the void, and I expanded it to work in a greater
# variety of use cases, or return something cheeky
# if a word that probably isn't "Twitter" is input.

def fix_spelling(name)
  if name == "Twitter"
    return name
  elsif name.downcase.include?('twit')
    name = "Twitter"
  else 
    return "Did you mean Instagram?"
  end
  return name
end
```

## 4. Helping a customer solve their root issue.

During my time in the Human Resources department at StoryCorps, I organized a 10-day training that happened twice a year and involved almost every department of the institution for at least a portion of it. The event onboarded and trained new Facilitators, who did much of the groundwork for the nonprofit by physically recording nearly every interview produced. While some departments knew exactly what they had to do as their portion of the training, there were some small departments who had recently lost the individual who usually conducted their training session so much of my time was spent helping different departments track down what should be covered and how. 

It quickly became clear that a central repository with all materials used for each training and a general outline of what would be covered was needed to solve the root of this problem. I started collecting as much information as possible to put on a page on the company Wiki so all necessary information would be publicly accessible and not get lost in the email inbox or the hard drive of someone who moved on to a new company. Since there was also little documentation of what was expected of me as the Coordinator, I also created a walkthrough for myself and whoever would take over after my time at StoryCorps. 

At the end of the training, I got feedback from participants that it was the smoothest Facilitator Training they had ever attended during their time at the company. By the time the next training rolled around, the extra effort I put into documenting everything cut the time it took for me to coordinate the event by about 50%, with similar time savings for departments that had encountered issues the time before or had turnover before the second training. Before my time, the mentality had been that all of these individual departments owned the training and HR was just there to support, but with my involvement we shifted to HR owning the training, to create a cohesive training experience for all involved.


## 5. Tell us about a time where you didn't know what to do.

A good example of my process when encountering a new challenge is how I approached my first project as a student at General Assembly. The prompt was to create any kind of application using vanilla Javascript, HTML, and CSS while utilizing an API, and in brainstorming and researching ideas I decided I wanted to try to create a musical synthesizer to combine my musical training with everything I had learned as a developer.

Once I decided what my project would be, I sketched out a detailed schedule to give myself as much structure as possible, and devoted the first two days entirely to research. I started looking at examples of synthesizers other developers had created to see how they approached it. I also found the MDN Web Docs documentation of the Web Audio API and a tutorial for building a synthesizer. This was incredibly helpful, but the tutorial walked you through building a synthesizer played only through mouse clicks and I was determined to have mine be playable with a computer keyboard. I also learned that the Web Audio API was built into most browsers so it did not satisfy the portion of the prompt that required us to use an external API. So after some brainstorming I decided to use PokeAPI to allow users to search for a Pokemon that would dance with their synthesizer playing.

Despite these pivots I had to make, I had set enough time to do some research on handling key events in vanilla javascript (I was not allowed to use a framework for handling key events) and css animations, and all of this research greatly reduced the number of issues I ran into while actually building the application. Because of those time savings I was able to do some additional research and implement 2 oscillator filters: a pinking filter (which adds pink noise to the pitch played and made the tone of the synthesizer a little more natural and less cheesy sounding), and a lowpass filter attached to a slider that let you cut out high frequencies to change the timbre of the synthesizer.

While I wasn’t able to accomplish all of my post-MVP goals, my strategy of trying to approach a project by taking the time to understand what it entailed as much as possible paid off.
