---
title: "Syllabus"
exports:
  - format: pdf
    output: exports/syllabus.pdf
---

# Stat 153/248: Introduction to Time Series Analysis

# Instructor and GSIs

* **Instructor** Liberty Hamilton - [liberty.hamilton@berkeley.edu](mailto:liberty.hamilton@berkeley.edu)
  * **Office Hours** (Evans 351) - Tuesday 9:30–10:30am starting Jan. 27


* **GSIs:** 
  * Nicholas Liskij - [nliskij@berkeley.edu](mailto:nliskij@berkeley.edu)
    * **Office Hours**: Thursdays 10:30am-2:30pm Evans 428
  * Yichen Pan - [yichenpan@berkeley.edu](mailto:yichenpan@berkeley.edu)
    * **Office Hours**: Monday 12-4pm Evans 428

* **Readers:** Brandon Xu and Jack Zhang

# Important Info 

* **Class times:** Tuesdays and Thursdays, 8-9:30am, VLSB 2060
* **Lab times:** Fridays, 9-11am, 11-1pm Evans 330 and 1-3pm, 3-5pm, Evans 342
* **Ed discussion** We have a site on [Ed Discussions](https://edstem.org/us/courses/93784) where you can post questions and interact with others in the class.
* **Github** The [github site](https://www.github.com/berkeley-stat153/spring-2026) will have homeworks and labs, but these will also be posted to this site.
* **bCourses** [bCourses](https://bcourses.berkeley.edu/courses/1551495) will be used to keep track of grades.

(about-stat-153)=
# Course Information 

A time series is a sequence of data points collected over time, for example, hourly temperature readings, stock market fluctuations, or brain responses over time for a particular stimulus. Such methods are important for uncovering regular patterns over time, including cycles and trends. In this class we will learn techniques for interpreting and modeling such data. Content will include (a) Multiple linear regression models, (b) Nonlinear regression models, (c) Regularized High-dimensional linear regression models, (d) Variance models and spectral analysis, (e) Lagged regressions and ARIMA models, (f) Recurrent Neural Networks, (g) self-supervised models. This class will use the Python programming language. Multiple applications will be explored, including but not limited to speech processing, neuroscience, astronomy, and epidemiology.

(prerequisites)=
## Prerequisites

Undergraduate probability at the level of STAT 134 or DATA 140 or EE 126 is required. Statistics at the level of STAT 133 and STAT 135 are strongly recommended. STAT 135 may be taken concurrently, though some students have found this course
challenging without prior completion of STAT 135. Labs, homework, and projects will be completed in the Python language, and familiarity with Python is a prerequisite.

(materials)=
## Required Materials

**Required Texts:** There are no required textbooks for this course. Instead, I will provide lecture notes as well as book chapters as PDFs, some of which come from the following recommended texts:

* Recommended Texts: <a href="https://link-springer-com.libproxy.berkeley.edu/book/10.1007/978-3-031-70584-7">Shumway & Stoffer Time Series Analysis and Its Applications</a>. 4th or 5th edition. 5th edition is available online for free through Springer Books (use the [library proxy instructions here](https://guides.lib.berkeley.edu/ezproxy/browser-bookmarklet) if you have trouble accessing - install the Berkeley proxy bookmarklet, go to the link, then click the bookmarklet).

(exams)=
## Exams

For STAT 153, there will be two exams: Midterm and Final. The Midterm will be on March 17 in class. The Final exam will be on May 14 from 7-10pm. 

For STAT 248, there will be one exam: Midterm (in class on March 17). Instead of the final exam, there will be a final project (details will be announced later).

(grading)=
## Grading

Grades will be based on five homework assignments, one midterm exam, and one final exam (STAT 153) or final project (STAT 248). The grading breakdown is as follows (each homework assignment is worth an equal amount):

* Homeworks: 50%
* Midterm: 20%
* Final Exam (Stat153) or Final Project (Stat248): 30%

Alternatively, the grade may be calculated as 50% homework + 50% final exam/project, whatever is the maximum.

## Differences between Stat153 and Stat248

This class is crosslisted as both an undergraduate (Stat158) and a graduate (Stat248) class. Each homework assignment will have 1-3 additional questions that only students taking STAT248 need to answer. STAT 153 has a final exam while STAT 248 will have a final project.

## Late policy

You have a total of 120 late hours that you can apply to your homework for the entire semester. No points will be awarded for any homework which brings the total late hours to more than 120.

## Accommodations for students with disabilities

If you require course accommodations due to a physical, emotional, or learning disability, contact UC Berkeley’s Disabled Students’ Program (DSP). You must have a Letter of Accommodation on file with UC Berkeley to have accommodations made in the course. Please also contact the Instructor (Liberty Hamilton) as early as possible to discuss these accommodations within the first few weeks of class.

## Academic Integrity

You are encouraged to work in small groups on homework problems. However, you must write up solutions on your own, including writing your own code, and you must never read or copy the solutions of other students. Similarly, you may use books or online resources to help solve homework problems, but you must credit all such sources in your writeup and you must never copy materials verbatim. This includes use of gen A.I. models such as ChatGPT, Claude, Gemini, etc. If you use these tools, you must check and verify each step and cite their use -- e.g., "Consulted ChatGPT for Problem 1.4, Shumway and Stoffer for Problem 1.5". Any students found to be cheating automatically risks failing the class and being referred to the Office of Student Conduct. In particular, copying solutions, in whole or in part, from other students in the class or any other source without acknowledgement constitutes cheating. 

Any test, homework, paper, or report submitted by you and that bears your name is presumed to be your own original work that has not previously been submitted for credit in another course unless you obtain prior written approval to do so from your instructor. If you are unclear about the expectations for completing an assignment or taking a test or examination, be sure to seek clarification from your instructor or GSI beforehand. For additional information on plagiarism and how to avoid it, read the UC Berkeley Library Citation Page, Plagiarism Section.

As a member of the campus community, you are expected to demonstrate integrity in all of your academic endeavors and will be evaluated on your own merits. The consequences of cheating and academic dishonesty—including a formal discipline file, possible loss of future internship, scholarship, or employment opportunities, and denial of admission to graduate school—are simply not worth it. Read more about Berkeley’s Honor Code.

## Take care of yourself

It is important to take care of your mental and physical health. If you need help or support, there are many resources on campus that can help. Please find them at the [Academic Accommodations Hub](https://evcp.berkeley.edu/programs-resources/academic-accommodations-hub). If you are sick, please do not come to class - this will help you recover and keep your classmates / course staff from getting sick. You can get the notes from a friend, or access materials online on the course website. 

# Tentative Schedule

Topics will be available on the [course website](https://stat153.berkeley.edu/spring-2026) as they occur. Below is a plan for the semester (subject to change as we move through):

| Date     | Topic                                                | Assignments     |
|----------|------------------------------------------------------|-----------------|
| Jan. 20  | Introduction to the class                            |                 |
| Jan. 22  | Characteristics of time series data                  |                 |
| Jan. 27  | Measures of dependence                               |                 |
| Jan. 29  | Measures of dependence (cont'd)                      | HW1 posted      |
| Feb. 3   | Simple Linear Regression                             |                 |
| Feb. 5   | Simple Linear Regression (cont'd)                    |                 |
| Feb. 10  | Multiple Linear Regression                           | HW1 due         |
| Feb. 12  | Multiple Linear Regression (cont'd)                                 |    |
| Feb. 17  | Nonlinear Regression          |                 |
| Feb. 19  | Class canceled | HW2 available                |
| Feb. 24  | Cross-validation, smoothing, regularization          |                 |
| Feb. 26  | Cross-validation, smoothing, regularization (cont'd) |                 |
| Mar. 3   | Power spectral analysis                              | HW2 due         |
| Mar. 5   | Power spectral analysis (cont'd)                     | HW3 available   |
| Mar. 10  | Time frequency analysis                              |                 |
| Mar. 12  | Time frequency analysis (con't)                      | HW3 due Mar. 13 |
| Mar. 17  | MIDTERM EXAM (in class)                              |                 |
| Mar. 19  | AR models                                            |                 |
| Mar. 24  | SPRING BREAK                                         |                 |
| Mar. 26  | SPRING BREAK                                         |                 |
| Mar. 31  | ARMA Models                                          |                 |
| Apr. 2   | ARIMA models                                         |                 |
| Apr. 7   | ARIMA models (cont'd)                                |                 |
| Apr. 9   | Time-lagged regressions                              | HW4 available   |
| Apr. 14  | State space models                                   |                 |
| Apr. 16  | State space models (cont'd)                          |                 |
| Apr. 21  | Convolutional Neural Networks                        | HW4 due         |
| Apr. 23  | Convolutional Neural Networks (cont'd)               | HW 5 available  |
| Apr. 28  | Recurrent Neural Networks                            |                 |
| Apr. 30  | Recurrent Neural Networks (cont'd)                   |                 |
| May 5    | Self-supervised learning                             |                 |
| May 7    | Self-supervised learning (cont'd), wrap up           | HW5 due         |
| May 14 7-10pm | FINAL EXAM  (location TBD)                      |                 |

 
