---
layout: post
title:      "Weather Today in a Ruby CLI"
date:       2021-04-06 10:00:07 -0400
permalink:  weather_today_in_a_ruby_cli
---


So I made my first gem Yay. It was challenging. It was hard. It took me a long time. 

I decided to build an app that took data from three different APIs. Why? Well I wanted to make it a bit challenging.

This CLI was built to give a quick update of the weather either by your current location or any location you ask for. Also It  provides headlines of BBC news as a plus. 

[Check it out](https://github.com/JcBonassin/weather_today)

So how do I build it? 

The first step was to identify the API's to use: 
so I signed for these awesome ones:

- [OPENWEATHERMAP](https://openweathermap.org/). To get the good weather data.
- [NEWSAPI](https://newsapi.org/). To get the news.
- [Abstract](https://app.abstractapi.com/). To get a precise timeZone on the location enquiry. They've got a free plan too. So all cool. 

So let's write the code: 

I  needed to get the data for the weather first so I built one Class for the IP location and one Class for a city search. I used  HTTParty to get the data and JSON to parse it. 

```cassandraql
def self.api_location(unit)
response = HTTParty.get("https://api.openweathermap.org/data/2.5/weather?lat=#{lat}&lon=#{lon}&appid=#{ENV['API_KEY']}&units=#{unit}")
        data = JSON.parse(response.body, symbolize_names: true)
        @weather_today = self.new
        @weather_today.location = data[:name]
        @weather_today.time = Time.at(data[:dt])
        @weather_today.temp = data[:main][:temp].to_i
```

same for the search per city.

```cassandraql
def self.select_name(units, location)
response = HTTParty.get("http://api.openweathermap.org/data/2.5/weather?q=#{location}&appid=#{ENV['API_KEY']}&units=#{units}")
      data = JSON.parse(response.body, symbolize_names: true)
      @weather_today = self.new
      @weather_today.response_code = data[:cod]
      if @weather_today.response_code === "404"
        spinner = TTY::Spinner.new("[:spinner] cod")
        spinner.error("404")
        return
      else
      @weather_today.location = data[:name]
```

Once I located all data I needed for current weather and also forecast I started bulding my CLI. I wanted to ge the following results: 

- Check the current weather at current location and Forecast for the next 5 days 
- Check the current weather and Forecast for the next 5 days at any city you name in the world plus a link to the city location on Google Maps. It only will work with city names. 

-  It gives you 3 unit system to choose from: 
  - Default (temperatures in Kelvin)
  - Metric (temperatures in Celsius)
  - Imperial (temperatures in Fahrenheit)

- Read and open in your browser the latest world headlines from BBC News. 
- Gives you a funny quote according to weather conditions courtesy of the [AUTHENTIC WEATHER APP](https://github.com/reduxd/authentic-ubersicht). 
- Also a Big GoodBye. 


"Voila" you are ready to check the weather and news. 

Well not so Easy. I wanted to make it more user friendly so on my CLI I added a few TTY components that are really handy. 
Especially TTY-Prompt, TTY-Box & TTY-Tables. Want to check them all click [Here](https://ttytoolkit.org/components/). They are easy to implement and you can find plenty of documentation online. So give it a try if you like them.

```cassandraql
class WeatherToday::CLI

    $prompt = TTY::Prompt.new(active_color: :cyan)
   
    def call
        welcome
    end
    
    def welcome
        puts Rain.go 
        puts Intro.go 
        sleep (3)
        puts"
                                             .-. .-.  ,---.   ,-.     ,-.      .---.   
                                             | | | |  | .-'   | |     | |     / .-. )  
                                             | `-' |  | `-.   | |     | |     | | |(_) 
                                             | .-. |  | .-'   | |     | |     | | | |  
                                             | | |)|  |  `--. | `--.  | `--.  \ `-' /  
                                             /(  (_)  /( __.' |( __.' |( __.'  )---'   
                                            (__)     (__)     (_)     (_)     (_)     
    ".colorize(:cyan)
         units_selection
         puts ''
         puts "World's News"
```


All right. Once my CLI was running and the hard part of the project was done I decided to add some art ;). Well not exactly but something like a ASCII art. There are plenty of resources online so you won't get lost. 

![Screenshot 2021-04-06 at 08 47 56](https://user-images.githubusercontent.com/72950188/113721208-ee3a2600-96b4-11eb-9ab2-9f564ec218f9.png)

Well I know it is a lot but It was fun. What I learn: 

- API can be tricky and hard to work with but once you are in the Matrix It becomes easier. 
- I need to improve my level of abstraction but for the first project I wasn't so bad I believe.
- There is so much you can do but overtime. It's hard to learn all at once. 
- Finally it is cool to add something else you love ... like Art for example. It makes coding more enjoyable.

Thanks for reading ..

So long 







