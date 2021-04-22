---
layout: post
title:      "React Project - Far Out"
date:       2021-04-22 02:31:57 +0000
permalink:  react_project_-_far_out
---

For my final project at Flatiron, I decided to tap into the NASA Astronomy Photo of the Day API. I wanted to create a web app that allowed users to vote between two pictures on their favorite, maintain the vote count on a database, and also show the Top 5 and Newest photos.

## Links
[Far Out Frontend](https://github.com/nlewis84/farout-frontend)
[Far Out Backend](https://github.com/nlewis84/farout-backend)
[Video Walkthrough](https://youtu.be/3SRwopQNobY)

## The Process
I started off by wireframing my design in Figma. I decided on a layout for everything, then set out structuring my database and models in Rails. Finally, I worked on the React portion and spent several hours developing my understanding of all things React.

### Next Steps
I currently have my app setup to fetch a specific route when the user clicks on the corresponding link. On the suggestion of a friend, I am planning to create a new branch that will fetch all three current sets of data (2 random, top 5, and newest picture), then persist them in React. Then, do not refetch the data, until that set of data becomes used or out of date. It seems that these would be two good choices for ways to handle this sort of application.

### Advice for future students
Try to spend as much time as possible with the planning portion of your project. Think really hard about exactly what data you want present in your models. Purposely wireframe what you want your specific pages to look like. Write a bulleted list to demonstrate where you want containers and components in React. Figure out what you want to name your routes in RoR. You may run into a roadblock when you actually start coding your project, but you will save yourself from heartache if a majority of your time is spent on planning.

