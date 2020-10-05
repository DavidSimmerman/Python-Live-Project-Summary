# Python-Live-Project-Summary

## Introduction
While enrolled at The Tech Academy, I worked on a live project that simulated what a software development job would be like. The project used HTML/CSS, JavaScript, and Python within the Django framework. I, as well as several other students, were tasked with creating our own Django applications within the same project. We were given 10 stories to complete that included both front and back end development with models, views, APIs and web scraping. We were assigned to create a list creation web page. I decided to make a movie review app. I had used Django skills I aquired earlier through The Tech Academy's Python course, as well as new skills that I developed while working on this project. I spent a great of time creating a thorough front end with lots of functionality and a versatile, in-depth back end to compliment it. I also learned a lot about a professional developer environment that works with stories, version control, and social aspects such as the daily stand ups. I enjoyed problem solving and learning new information throughout the two weeks, and I am glad that I was given this oportunity to gain more skills and experience to apply in my Computer Science career.

Below I have detailed my experience throught the project including things I had learned and struggled with along with pictures and code from the project.

## Front End Work
I spent a good portion of my time working the front end of my website. I wanted to make it look good, but also not too flashy. I especially didn't want to sacrifice the user experience to make a flashy website. I kept it simple and looked at some of my favorite popular websites to see what they did well. I created a basic template that inluded the header image with a background and a simple task bar. The template can easily be altered for each page. I also created a container that could easily be reused to create a uniform aesthetic for all of the content on my page. I am very satisfied with the final look of the website.

### Creating a Form
The Django framework allows a built in form for a developer to add input into their database. It was simple to implement, but I thought it looked too simple and out of place. I instead created my own form to make the page seem more fluent. I used the ```request.POST.get()``` function in the views.py file to grab the form information. The biggest challenge I had encountered was creating a star rating system. I wanted the user to select from 5 stars to leave a review on a movie. I was able to find a couple of tutorials online, but I had to alter them to work for my specific use-case.
<details>
  <summary>HTML and CSS Star Ratings</summary>

  HTML: 
  ```
  <div class="stars">
      <input class="star star-5" id="star-5" type="radio" name="star" value="5" {% if e_stars == 5 %}checked{% endif %}/>
      <label class="star star-5" for="star-5"></label>
      <input class="star star-4" id="star-4" type="radio" name="star" value="4" {% if e_stars == 4 %}checked{% endif %}/>
      <label class="star star-4" for="star-4"></label>
      <input class="star star-3" id="star-3" type="radio" name="star" value="3" {% if e_stars == 3 %}checked{% endif %}/>
      <label class="star star-3" for="star-3"></label>
      <input class="star star-2" id="star-2" type="radio" name="star" value="2" {% if e_stars == 2 %}checked{% endif %}/>
      <label class="star star-2" for="star-2"></label>
      <input class="star star-1" id="star-1" type="radio" name="star" value="1" {% if e_stars == 1 %}checked{% endif %}/>
      <label class="star star-1" for="star-1"></label>
  </div>
  ```
  The e_stars variable as passed in from the views.py file. This was used if the user was editing an old review and wanted to change their rating. That way it would save their previous setting.

  CSS: 
  ```
  div.stars {
    display: inline-block;
  }

  input.star { display: none; }

  label.star {
    float: right;
    padding: 0 5px;
    font-size: 30px;
    color: lightgray;
    transition: all .2s;
    margin: 0;
    height: 27.5px;
  }

  input.star:checked ~ label.star:before {
    content: '\f005';
    color: rgb(255,190,100);
    transition: all .25s;
  }

  label.star:hover { cursor: pointer; }

  label.star:before {
    content: '\f005';
    font-family: FontAwesome;
  }

  .dr-stars {
      font-family: FontAwesome;
      color: lightgray;
  }

  .selected-star {
      color: rgb(255,190,100);
  }
  ```
  
  </details>
I am happy with the final product; however, I wish I had more time to add half stars. This would add a little more depth and user experience to the project.

### Search System
As I started using the website in testing, I realized I needed to add a search system to sort through movies and reviews. I created two javascript search functions. One would search through movies while the other searched through reviews. The only main difference between the two was the tags that it searched for.
  
<details>
  <summary>Javascript Movie Search Function</summary>
  
  ```
  function SearchMovies()
  {
      console.log("run")
      search_string = document.getElementById("js_movie_search").value.toLowerCase();
      search_words = search_string.split(" ");

      var movie_containers = document.getElementsByClassName("js_searchable");
      for (var i = 0; i < movie_containers.length; i++)
      {
          var movie_title = document.getElementById(movie_containers[i].id).getElementsByTagName("p")[0].innerHTML.toLowerCase();

          movie_contains_words = true;

          for (var w = 0; w < search_words.length; w++)
          {
              if (!movie_title.includes(search_words[w]))
              {
                  movie_contains_words = false;
                  break;
              }
          }

          if (!movie_contains_words) movie_containers[i].style.display = 'none';
          else movie_containers[i].style.display = 'block';

      }
  }
  ```

</details>

My goal was to make the best user experience in the time that I had. I made sure to break down each word of the search input to help eliminate user error. For example, if the user searched for "Bat Man" or "The Batman," but the movie they wanted was simply called "Batman," those two search results wouldn't find the movie they wanted. By breaking it down into each word, they would get the desired result.

### Notification Modals
My biggest struggle was creating some way to notify the user about events such as bad input, an error in the website, or successful creations/edits to reviews. I decided to use Bootstrap's Modals to convey this information which turned out to be a challenge. Modals are built in to be used by a button, but I needed a way to get it to appear on a page load. First, I created a session variable. If an error was thrown or if there was some other notification that needed to be displayed to the user, it would be stored in the session variable. Then, when the next page loads, it would display the needed info if the variable was set.
<details>
  <summary>Example of Modal Notifications</summary>

  ```
  {% if e_message is not None %}
      <div class="modal show" id="actionNotif" tabindex="-1" role="dialog" style="display: block" onclick="$('.modal').hide()">
          <div class="modal-dialog" role="document">
              <div class="modal-content">
                  <div class="modal-header bg-danger text-light">
                      <h5 class="modal-title" id="exampleModalLabel">Submission Error</h5>
                      <button type="button" class="close" data-dismiss="modal" aria-label="Close" onclick="$('.modal').hide()">
                          <span class="text-light" aria-hidden="true">&times;</span>
                      </button>
                  </div>
                  <div class="modal-body">
                      You have received the following errors on your last submission: <br/><br/>
                      {% if e_message == 'movie-already-reviewed' %}
                          &nbsp;- You have already reviewed that movie.<br/>
                          &nbsp;&nbsp;&nbsp;&nbsp;Click <a href="{{ e_rev_obj.get_edit_url }}">here</a> to edit.
                      {% else %}
                          {{ e_message }}
                      {% endif %}
                  </div>
              </div>
          </div>
      </div>
  {% endif %}
  ```

</details>

## Backend Work
A large amount of time was spent on the backend of the website. Creating logic and problem solving is why I love programming so much which is mostly in the backend of a website. I spent a lot of time thinking out a model for the database of the website. I created an in depth model for the movies and broke them down into multiple tables for the Director and Genres. I had originally planned to suggest a movie or genre to the user based on their reviews, but I did not have enough time in the two week sprint for this. Most of my time was spent error handling or on small functions here and there, so there isn't very much code to be shown. I broke down most of the user's input to ensure that it is saved in the database correctly and does not cause any errors to occur. 
<details>
  <summary>User Input Parsing</summary>
  
  ```
  # If no movie found or movie already reviewed, no need to check errors
  if valid_review:
      # Checks if the user entered a star rating
      if review_stars is None:
          valid_review = False  # Tells the program that there is an error
          # Adds to the error message to say what the error is
          error_message += " - No stars selected. Please select a star rating before submitting.\n"
      # Makes sure the user inputted review text
      if len(review_text) < 1:
          valid_review = False  # Tells the program that there is an error
          # Adds to the error messages to say what the error is
          error_message += " - No review written. Please write out a review before submitting.\n"
      # Checks if the user's review has too many characters
      if len(review_text) > 1000:
          valid_review = False  # Tells the program that there is an error
          extra_characters = len(review_text) - 1000  # Calculates how many chars over the limit the user is
          # Adds to the error message to say what the error is - includes number of exceeded characters
          error_message += " - Your review  is {} characters too long. There is a maximum of 1000 characters\n"\
              .format(extra_characters)
  ```
  
</details>


### Working with APIs
Finally, the last big part of this project was the API. I had never used an API before this part of the project, so I was going in completely fresh. The original idea was overwhelming, but eventually I learned that it was actually rather simple. I created a function that would take a movie from IMDb's database and add it to the website. The user could search for a movie on their own, or it could be created within the code. The json response would then be broken down and added to the database.
<details>
  <summary>API Response to Database</summary>
  
  ```
  def add_movie_to_db(response):
      if response.status_code != 200 or not response.json()['Response']:
          return None

      response_string = response.json()
      try:
          d = Director.objects.get(director_name=response_string['Director'])
      except:
          if response_string['Director'] == 'N/A':
              d = Director(director_name=response_string['Writer'])
              d.save()
          else:
              d = Director(director_name=response_string['Director'])
              d.save()

      genre_list = response_string['Genre'].split(',')
      g = [None, None, None]
      for i in range(len(genre_list)):
          if i > 2:
              break
          try:
              g[i] = Genre.objects.get(genre_name=genre_list[i])
          except:
              g[i] = Genre(genre_name=genre_list[i])
              g[i].save()

      movie_name = response_string['Title'].replace('/', ' ')
      movie_name = movie_name.replace('&', ' ')

      try:
          m = Movie.objects.get(movie_name=movie_name)
      except:
          if '–' in response_string['Year']:
              movie_year = response_string['Year'][:4]
          else:
              movie_year = response_string['Year']
          m = Movie(movie_name=movie_name, movie_year=movie_year, movie_director=d,
                    movie_image=response_string['Poster'], movie_genre1=g[0], movie_genre2=g[1], movie_genre3=g[2])
          m.save()
  ```
  
</details>

## Conclusion
I learned a lot from this project and had a blast solving problems and picking up new tricks. Working with Django was an amazing experienc,e and I am glad to now have experience working with an API. What I really learned from this project was that you aren't expected or required to know everything about the project you are working on going into it; you learn more and more about the task at hand as you progress and research the parts you are confused about in order to accomplish the challenge you have. I also learned a lot about Django, Python, and APIs that I hope to use in future projects.
