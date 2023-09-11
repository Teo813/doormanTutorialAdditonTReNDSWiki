---
layout: default
title: Modules
nav_order: 2
parent: List of software
last_modified_date: 09/11/2023 10:32
---
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

# How to Use Doorman as an admin
Doorman's admin commands are controlled through the usage of Modals. Each of these commands have different functions
### /addnewgroup
Caling this command will open a screen which allows you to enter 3 things
1. Answer Label 
2. Example Questions
3. Answer
   
The **Answer label** section should be a short descriptive title for the type of questions the answer corresponds to. For example if the answer describes how to hire a new student then the label should be something like "Student Hiring".
**Example Questions** should be several questions or statments that correspond to your question. They should not be keywords but rather sentences, phrases, or questions that are answered by your answer. **Answer** This section should have the answer to the example questions and prompts, if there are any links in your answer please insert them as the full hyperlink.

### /rmgroup
Calling this command will open a screen with all of the current questions doorman is using and a corresponding delete button. Clicking the delete button will remove the question from Doorman.

### /printqlist
Calling this command will show you all of the questions in plain text in a message from Doorman.

### /howmanyqs 
Caling this command will tell you how many questions are currently in the dataset Doorman is calling from.