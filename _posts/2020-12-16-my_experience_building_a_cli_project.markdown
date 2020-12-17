---
layout: post
title:      "My Experience Building a CLI Project"
date:       2020-12-16 19:35:10 -0500
permalink:  my_experience_building_a_cli_project
---


I am a student at Flatiron School and have really been enjoying the last month of learning Ruby. It's time for me to build my first major portfolio project, a CLI Data Gem, built primarily in Ruby. 

# What exactly am I building?
* Must have a CLI
* Must provide access to data from a web page
* The data provided must go at least one level deep
* Should not be too similar to a project used in a previous assignment
* Must use goo OO (Object Oriented) design patterns (scrape once per web page!)

# What next?
I asked myself, what is some sort of data source that I use on a daily basis. I narrowed it down to two possibilities: 
1. www.tapspace.com // A music publisher website that I use almost daily to select new compositions for my performing ensembles
2. www.boardgamegeek.com // A modern board game website that I use to find new and hot board games, and to see info on previously released games

I decided to go with the first option of www.tapspace.com.

Next, I decided on some variables that I would like to sort data by. I went with Number of Players, Difficulty, Composer, and UIL. UIL is the governing body in charge of basically all school run contests and competitions in Texas and I wanted to cross check pieces of music against that list. This is functionality that is not in my current iteration, but I would like to add it down the road.

From here, I mapped out what my classes should be. I went with the following classes:
* CLI
* Scraper
* Ensemble
* Composer

Next, I grabbed a scratch piece of paper and began to brainstorm what sort of displays for my CLI I wanted to build.

Finally, I found this [great video by Beth Schofield](https://www.youtube.com/watch?v=KwBMwZ89Hj8) about building a CLI Data Gem.

[Part 2 of that video](https://www.youtube.com/watch?v=TaRZ9Z8dK2s)

[Part 3 of that video](https://www.youtube.com/watch?v=VMAW3VjPUPw)

After that, I wrote out a flowchart showing how the user would interact with my CLI.

[Flowchart.jpeg](https://drive.google.com/file/d/1_j66CTxXJoHHAOn4dqvCYc7ROtoyr-_j/view?usp=sharing)
# Time to Code!
At this point, I began to set up my classes. I knew the Ensemble class was going to be my bread and butter, but I didn't know how I would initialize an instance in that class yet. I decided to get my scraper up and running first. Using Pry, I was able to get everything setup there. Due to the website not loading in all of the pieces in their library (without scrolling) I took a LOOONG detour trying to figure out a way to get Watir working with my program. I eventually decided on the advice of a mentor to not worry about this. Here's what my scrape method looks like:

```
    def scrape(site)
        doc = Nokogiri::HTML(open(site))

        ensembles = []

        doc.css("div.catalog-list2").each do |card|
            current_ensemble = {}

            ensemble_title = card.css("div.catalog-product-image img").first["title"]
            composer_name = card.css("div.catalog-product-title a:nth-of-type(2)").first["title"]
            
            composer = PercussionEnsembles::Composer.find_or_create_by(composer_name)
            
            more_info = card.css("div.catalog-fields")

            current_ensemble.merge!(:name => ensemble_title, :composer => composer)
            more_info.each do |info|
                full_details = {}
                info.text.split(" | ").each do |details|
                    key, value = details.split ': ', 2
                    full_details.merge!(key.downcase.to_sym => value)
                    current_ensemble.merge!(full_details)
                end
            end
            ensembles << current_ensemble
        end
        ensembles
    end
```

# First major struggle
Once I bailed on using Watir, my next struggle was that I was using mass assignment to instantiate in my Ensemble class. I wanted to have my Ensemble class to have a "belongs to//has many" relationship with composers, but that was complicated. I was building my array of hashes in Scraper and when I passed it to Ensemble, it couldn't do anything with the string for composer....at least not the way I wanted it to. My Ensemble initialize method looked something like this at that time:

```
    def initialize(ensemble_hash)
        ensemble_hash.each do |key, value|
            self.send("#{key}=", value)
        end
    end
```

and this line in my Scraper did not have a find or create by name method.

```
composer = PercussionEnsembles::Composer.find_or_create_by(composer_name)
```

That was my next move, I created that method and was able to finally uncomment my add_song(song) method.

```
    def self.find_or_create_by(name)
        if self.all.find {|comp| comp.name == name}
            composer = self.all.find {|comp| comp.name == name}
        else
            PercussionEnsembles::Composer.new(name)
        end
    end

    def add_song(song)
        song.composer(self) unless song.composer
        @songs << song unless @songs.include?(song)
    end
```

# Smooth Sailing!
Around this time, I switched over to building my CLI. I formatted my data using Colorize.

```
    def display_ensembles(ensembles = PercussionEnsembles::Ensemble.all)
        ensembles.each do |ensemble|
            puts "#{ensemble.name.upcase}".magenta
            puts "#{ensemble.composer.name}".yellow
            puts "#{ensemble.level}".blue
            puts "#{ensemble.personnel}".cyan
            puts "#{ensemble.duration}".white
            puts "---------------------------------------------".green
        end
    end
```

I fleshed out my menu structure.

```
    def menu
        input = ""

        while input!= "exit"
            puts "To search by composer, enter 'composer'.".cyan
            puts "To search by difficulty, enter 'difficulty'.".cyan
            puts "To search by number of players, enter 'players'.".cyan
            puts "To search by difficulty and number of players, enter 'both'.".cyan
            puts "To quit, type 'exit'.".red
            puts "What would you like to do?".yellow

            input = gets.strip

            case input
            when "composer"
                composer
            when "difficulty"
                difficulty
            when "players"
                players
            when "both"
                both
            end
        end
    end
```

Then I built my "sorting" methods.

```
    def difficulty(ensembles = PercussionEnsembles::Ensemble.all)
        input = ""

        while input != "exit"    
            puts "1. Easy".cyan
            puts "2. Med-Easy".cyan
            puts "3. Medium".cyan
            puts "4. Med-Advanced".cyan
            puts "5. Advanced".cyan
            puts "To return to the main menu, type 'exit'.".red
            puts "Type the number of the difficulty you would like to see.".yellow

            input = gets.strip

            puts "---------------------------------------------".green
            
            case input
            when "1"
                display = display_ensembles(ensembles.select {|ensemble| ensemble.level == "Easy"})
            when "2"
                display = display_ensembles(ensembles.select {|ensemble| ensemble.level == "Med-Easy"})
            when "3"
                display = display_ensembles(ensembles.select {|ensemble| ensemble.level == "Medium"})
            when "4"
                display = display_ensembles(ensembles.select {|ensemble| ensemble.level == "Med-Advanced"})
            when "5"
                display = display_ensembles(ensembles.select {|ensemble| ensemble.level == "Advanced"})
            end
        end
    end
```

Several days later, after proofreading everything, removing redundant and repetitive code, I had a project that was doing what I wanted, the way I wanted!

# What next?
Here's a list of additional functionality that would be cool to add to this and some of the hurdles I foresee:

* Scrape the UIL PML (University Interscholastic League prescribed music list) and search only for pieces on that list that are published by TapSpace (or other composers I can scrape!)
* Scrape more data from www.tapspace, using something like Watir that can emulate human behavior of navigating a website (to activate JavaScript and AJAX elements)
* Scrape from www.c-alanpublications.com. This will require quite a bit of Regex as they do not sort out their info quite as neatly.

Thanks for reading, and I hope that this has helped if you are building your first project!

