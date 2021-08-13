# Intercom Take-Home

## 1. Describe a coding solution you're most proud of.

I think the pieces of code I’m the most proud of are from my final project at General Assembly: a React on Rails application aiming to be the start of a fake POS system for the last Blockbuster. In most of my projects I made things difficult for myself by coming up with ideas that involved complicated forms, but with each project I made more improvements over my first attempt. 

In this app the form is used to add a new movie to the database or update an existing one - the same form is used for both functions, it’s just pre-populated with the data of an existing movie if you’re editing it. It collected the movie’s title, year of release, directors and featured actors with options to add fields for multiple directors and actors, which was pretty straight-forward on the front end, but since my database contained separate tables for Movies, Directors, and Actors, I wanted to avoid making 3 API calls to add a movie. So instead, the front end makes a post or put request submitting all of the information as one packet, “Form Data,” to the Movies controller, which checks to see if the information already exists, and if not adds each item to its respective table.

`
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
`

## 2. Troubleshooting a 504 error code.

## 3. Debugging exercise.

## 4. Helping a customer solve their root issue.

## 5. Tell us about a time where you didn't know what to do.
