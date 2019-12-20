---
layout: post
title:  "Using JavaScript with Qualtrics"
date:   2019-12-18
---

Qualtrics is a very powerful tool to design and distribute online surveys. Its graphical interface can already fulfill the majority design needs. However, occasionally we might need to implement a more complex design. When readily available features in Qualtrics fall short, we can still rely on JavaScript to achieve the design. As a starter, here are some useful resources about using Javascript in Qualtrics:

- [Qualtrics JavaScript Question API Class](https://s.qualtrics.com/WRAPI/QuestionAPI/classes/Qualtrics%20JavaScript%20Question%20API.html)
- [Qualtrics JavaScript Help Page](https://www.qualtrics.com/support/survey-platform/survey-module/question-options/add-javascript/)
- [Common Use Cases and Code](https://psrc.princeton.edu/sites/psrc/files/html_tips_tricks.pdf)

In addition, below is a scenario that I ran into in a project, where JavaScript has proven to be very useful. Subjects in a survey needs to complete several rounds of prediction tasks, in _random_ order. After each round, a summary statistic of the subject's performance _up to_ the current round needs to be displayed. One can create embedded data to keep track of round index and the cumulative performance, but because of the randomized order of tasks, JavaScript is needed to automatically update the embedded data after each round. Function `Qualtrics.SurveyEngine.setEmbeddedData(ED_Name, NewValue)` can update a given embedded data field. However, it only takes effect after the current page (which hosts the JavaScript) is submitted, i.e., the JavaScript-enabled updates of embedded data must be done before the embedded data are referenced.