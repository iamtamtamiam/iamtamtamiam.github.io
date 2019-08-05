---
layout: post
title:      "CLI Gem Project - Best_Schools"
date:       2019-08-05 03:02:37 +0000
permalink:  cli_gem_project_-_best_schools
---

#### What’s a teacher’s favorite nation?
An expla-nation! 

```
if you_get_it && funny
  chuckle
else 
  crickets
end 
	```

#### What’s a teacher’s favorite district?
...well let me introduce you to my gem for my CLI project. 

Whether you’re a parent looking for a good area to move in, a teacher looking for a good place to work in, or a student applying for a college you’re trying to get in, knowing how well the school district in the area ranks among other districts can be an important factor to consider. As a teacher myself, I remember trying to decide where I wanted to work in and with Houston being such a big area, I had quite a lot of districts to choose from. If you’re from Houston you probably understand why. For my friends who have never been, here are some visuals:

When I say big, 

![img](https://www.texasmonthly.com/wp-content/uploads/2014/04/san-francisco-8.png)

I mean big...

![img](https://www.texasmonthly.com/wp-content/uploads/2014/04/london-8.png)

...really big.

![img](https://www.texasmonthly.com/wp-content/uploads/2014/04/honolulu-8.png)

And so, my inspiration for my CLI project was found. Best_Schools (short for best_school_districts) scrapes some of the current best districts of the Houston Area from Niche.com (for more information on how the rankings are determined, you can refer to niche here: [](https://www.niche.com/k12/search/best-school-districts/m/houston-metro-area/)https://www.niche.com/k12/search/best-school-districts/m/houston-metro-area/. Users can view the districts in a numbered ranking list and select a district (by its rank number) to view more information on it (such as student population or average graduation rate). 

### The Lesson Plan

I read the project requirements...and I re-read...and I repeated the process a few more times until everything became a blur from the overwhelming anxiety. **Where does one even begin???!** Suddenly I heard an angelic voice in my head -a recent memory from a CLI online study group I had attended began to play - and the voice of my wise project coach said "well...you can begin by watching me do it," and so, I looked up her example on how to build a CLI project. I ended up watching several more videos of other project examples and though it consumed many hours, it gave me the confidence and knowledge to actually begin setting up a gem. Thus, the process began and I started to build my CLI class with my call method which served as a short outline of the basic methods I would need. 

```
def call
    puts "Welcome! According to Niche, the best districts of the Houston Area are:"
    get_district_rankings #scrape the districts off the site
    list_districts  #district name by rank number
    get_user_district_selection #let input be a number
    #show_district_info_for(@district_selection)
    #return_or_exit #maybe find way to exit anytime
  end 
	```

Then it was time to start building each method. "It's just scraping in this method, right?" I thought to myself. Well yes, but no.
- First I had to build a scraper method...which requires a scraper class...
- but if I scrape I have to be able to store it somewhere...so I should build a district class first to hold all of my district's
- so I have to be able to initialize a district from the scrape and store it
- and each district will have different attributes which will come from thier specific details
- which brings me back to scraping each individual districts webpage...oh boy...
- let's just start with the CLI and use fake data like you saw in the videos...

Just from building my CLI class, I had learned to use fake data to help me test out my code which in turn also helped me to improve on using Pry. 


*(lines commented out are the fake data I used, added, and deleted as I continued to build my code)*
![img](https://i.imgur.com/86FjLx8.png?1[/img)

With most of my CLI class basic code being built using the fake data, I was able to then start building my District class. 


```
class BestSchools::District
  
  attr_accessor :name, :url, :district_info, :student_teacher_ratio, :percent_proficient_reading, :percent_proficient_math, :graduation_rate, :number_of_students, :number_of_schools
  
  @@all = []
  
  def initialize(name, url)
    @name = name
    @url = url
    save
  end 
  
  def self.all
    BestSchools::Scraper.scrape_districts
    @@all
  end
  
  def save
    @@all << self
  end 
  
  def district_details
    BestSchools::Scraper.scrape_details(self)
  end 
end 
```

Then to build the Scraper class itself. Now I'll spare you the details on the whole scraping code, but there is an interesting lesson that I'd like to share. You see, this class was the one class I was most excited to build. I was excited because the lessons and labs on scraping with Nokogiri had turned out to be the easiest for me...I thought I was going to be a pro at it...but when I scraped I ended up with this hideously long code:


`districts = doc.css("div.search-results ol.search-results__list li.search-results__list__item div.search-result div.card a.search-result__link div.card__inner h2")`

...After hours of searching different sites on how to scrape and a dose of CLI coaching, I got a lesson on humility. This is all I had to do:  `districts = doc.css("ol.search-results__list  div.search-result")`. Was I as good as I thought I was, absolutely not. But that was absolutely okay. A much appreciated lesson. After that, scraping the details on the individual districts became a lot easier. 

### Anticipatory Set (teacher lingo for lesson engagement)

The code was working! but now to make it pretty. To have a lesson plan is one thing, but to make it engaging is another rodeo. 

I started with what seemed more simple. I looked up different escaping sequences to add spaces and margins. Then I researched some font changing gems and I decided to use Colorize. There are several out there, but after comparing for a hour, this one seemed to be the most syntactically clean to me. I was able to spice things up with some color and do things like *italicize* !

![img](https://i.imgur.com/sJIReiZ.png[/img)


Then came the code refactoring. I would like to take another moment to once again emphasize how great the CLI study groups are. From them I was able to get lessons and advice on where my code could look more clean such as changing variable names and creating methods for when my code was repetitive...even a basic reminder on what DRY stood for! This is an example of my refactored code after I was able to modify my exit methods and variable names:

- Before:
```
 def get_user_district_selection
    @input = gets.strip
      if  @input.to_i > 0 && @input.to_i <= @districts.length #@district_selection.to_i.between?(1, @districts.length)
        show_district_info_for(@input.to_i)
        return_or_exit
    elsif @district_selection.downcase == "exit".downcase || @district_selection.downcase == "e".downcase 
      puts "Good-bye!".colorize(:black).on_white.bold
     else 
        puts "\vYou have entered an invalid response. Please enter a number between 1 and #{@districts.length} or, to exit the program, enter 'e' or exit' t.\v".colorize(:red).bold
      call
  end
 end 
 ```

- After:
 ```
 def get_user_district_selection
    @input = gets.strip
    unless exit_input
      if valid_input
        show_district_info_for(@input.to_i)
        return_or_exit
      else 
        puts "\vYou have entered an invalid response. Please enter a number between 1 and #{@districts.length} or, to exit the program, enter 'e' or exit' t.\v".colorize(:red).bold
        get_user_district_selection
      end
    end 
  end 
	```



 ###  Lesson Self-Evaluation
 
 Overall this project was quite challenging but I learned so much. Lots of lessons and skills were developed but I think the most valuable for me was to not give up when I had a question. I found myself watching hours of video lectures (videos on how to set up repositories, videos on how to install gems, videos on different completed projects, etc.), attending more study groups than I have ever before, and discovering loads of resources that are quite useful.  A part of me wants to see if I can go back and add more features, a part of me wants to do it all again with a different site, and a part of me is ready to move foward with the cirriculum. Thanks for reading and good luck to those who are starting thier project! YOU CAN DO IT!
 
 Here's my video see the user experience for the gem:
 
[video](https://youtu.be/DIFj4cbSBys)
 
 
 
 
 
 
 









