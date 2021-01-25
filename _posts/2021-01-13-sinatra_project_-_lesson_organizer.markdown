---
layout: post
title:      "Sinatra Project - Lesson Organizer"
date:       2021-01-13 23:12:07 -0500
permalink:  sinatra_project_-_lesson_organizer
---


The last month of Flatiron has been a whirlwind! I moved on from a very successful CLI build to SQL, ORMS, Rails, ActiveRecord, and Sinatra. I felt like I was in a good place with this in the beginning...all of the SQL database material made lots of sense to me since I have a strong background with Access and Excel. The thing I struggled most with in this section was how ActiveRecord handles associations. 

I spent a lot of time reading the docs for ActiveRecord, specifically [this page](https://guides.rubyonrails.org/association_basics.html). I definitely could have built an app for this project that would have met specs, and been significantly less complex, but I really wanted to dig in on associations and understanding them better.

[My project](https://github.com/nlewis84/lesson-organizer) is a web app to be used in a school setting. There are 4 models: Admin, Teacher, Student, Lesson. Their associations are below:

```
Admin
has_many :teachers
has_many :students

Teacher
has_many :lessons
has_many :students, through: :lessons
belongs_to :admin

Student
has_many :lessons
belongs_to :teacher
belongs_to :admin

Lesson
belongs_to :student
belongs_to :teacher
```

One particularly challenging part of my web app was the deletion part of CRUD. I wanted it to function where if you delete a student, it would also delete all of the associated lessons. Same thing for teachers...delete all associated lessons. I experimented with using a callback in destroy_before, but had difficulty getting it to function the way I wanted. I ended up hard coding the deletion using this line of code in my `teacher/:id/delete` route:

`@teacher.lessons.each { |l| l.destroy }`

I did not spend much time with the CSS side of this….hardly any, which is fine based on the specs of the project, but I would like to eventually clean this up for my GitHub page. I believe that the code here is solid and a nice face-lift will be the touch it needs to look great on my portfolio.

