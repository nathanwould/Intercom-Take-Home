# Intercom Take-Home

## 1. Describe a coding solution you're most proud of.

I think the pieces of code I’m the most proud of are from my final project at General Assembly: a React on Rails application aiming to be the start of a fake POS system for the last Blockbuster. In most of my projects I made things difficult for myself by coming up with ideas that involved complicated forms, but with each project I made more improvements over my first attempt. 

In this app the form is used to add a new movie to the database or update an existing one - the same form is used for both functions, it’s just pre-populated with the data of an existing movie if you’re editing it. It collected the movie’s title, year of release, directors and featured actors with options to add fields for multiple directors and actors, which was pretty straight-forward on the front end, but since my database contained separate tables for Movies, Directors, and Actors, I wanted to avoid making 3 API calls to add a movie. So instead, the front end makes a post or put request submitting all of the information as one packet, “Form Data,” to the Movies controller, which checks to see if the information already exists, and if not adds each item to its respective table.

```
  # POST /movies
  def create
    # First, create a new instance of a Movie. Actors and directors are ignored because associations will be handled later.
    @movie = Movie.new(movie_params.except(:actors, :directors))
    # Then, see if actors in incoming movie params exist and if not, add them to the Actors table. The associated actors are saved in the @actors variable.
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

First I would collect whatever identification information is standard practice for Intercom (name and company, verify identity as required). Since a 504 error would most likely be an issue with Intercom’s server rather than a client-side issue, I would check to see if there were any outages with Intercom’s servers. If there were none I would then start gathering context around the error to see what specific request caused the 504 error. I would figure out if the error was happening in the browser or through another tool like Postman, and if it was happening in the browser I would have them clear the cache and refresh. 

If that didn’t resolve the issue I would check the API routes specified to make sure there wasn’t an incorrect route causing the issue - Intercom’s error handling seems robust enough that a typo in a route would just fail and not cause a timeout but I think it’s always better to check.

## 3. Debugging exercise.
```
###
# Problem Definition: This Ruby method should ensure 
# that the word "Twitter" is spelt correctly. 
###

# Original code to be debugged:
# Essentially: "If the input is this one particular 
# misspelling of twitter, fix it, or else fall into 
# an infinite recursive loop."

# def fix_spelling(name)
#   if name = "twittr"
#     name = "twitter"
#   else 
#     fix_spelling(name)
#   end   
#   return "name"
# end

# My fixed algorithm works properly without falling
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

I think one of the best examples was during my time at StoryCorps organizing a 10-day training that happened twice a year and involved almost every department of the institution for at least part of it. It was the onboarding and training of new Facilitators, who did much of the groundwork of the nonprofit, physically recording nearly every interview produced. While some departments knew exactly what they had to do as their portion of the training, there were some small departments who had recently lost the individual who usually conducted their training session so much of my time was spent helping different departments track down what should be covered and how. 

It quickly became clear that a central repository with all materials used for each training and a general outline of what would be covered was needed, and I started collecting as much information as possible to put on a page on the company Wiki so everything would be publicly accessible and not get lost in the email inbox or hard drive of someone who moved on to a new position elsewhere. Since there was also little documentation of what was expected of me as the coordinator, I also created a walkthrough for myself and whoever would take over after my time at StoryCorps. 

Despite feeling like I was flying by the seat of my pants the entire 10 day training and for almost 6 weeks before, I got feedback from participants that it was the smoothest a Facilitator Training had ever gone in their time at the company. By the time the next training rolled around, the extra lengths I went to document everything cut the time it took for myself to coordinate everything by about half, with similar time savings for departments that had encountered issues the time before or had turnover before the second training. Before my time the mentality had been that all of these individual departments owned the training and HR was just there to support, but with my involvement we shifted to HR owning the training, which a variety of departments were responsible for executing.


## 5. Tell us about a time where you didn't know what to do.
