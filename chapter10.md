---
title: Programming
description: 'This chapter builds on the last by showing you how to take advantage of the IDE for a more efficient and enjoyable R programming experience. For example, you''ll see how RStudio''s built-in code and style diagnostics can flag things before they go terribly wrong. You''ll learn tricks for using multiple cursors at once, collapsing code for better visibility, and using RStudio''s handy tools for debugging your code.'
---

## RStudio's coding features

```yaml
type: VideoExercise
key: 5bdb6608fb
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871426
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v3/hls-3-1.master.m3u8
```


---

## Code completion features

```yaml
type: RStudioMultipleChoiceExercise
key: 8d14cf84a2
skills: 1
```

You can adjust code completion settings by selecting 'Tools' => 'Global Options' => 'Code' => 'Completion'.

From this window, you are only able to turn code completion on or off.

True or false?

`@possible_answers`
- True
- False

`@hint`
Garrett explains how you can customize code features in the video.

`@sct`
```{r,eval=FALSE}
msg1 <- "Try again! RStudio allows you to customize your code completion settings in more ways than simply turning them on and off."
msg2 <- "Nice! You can customize your code completion settings to your preferences!"

test_mc(2, feedback_msgs = c(msg1, msg2))
```

`@attachments`


---

## Code diagnostics

```yaml
type: VideoExercise
key: 7d1450c870
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871423
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v2/hls-3-2.master.m3u8
```


---

## Diagnostic features

```yaml
type: RStudioMultipleChoiceExercise
key: 7dcf184db5
skills: 1
```

Navigate to 'Tools' => 'Global Options' => 'Code' => 'Diagnostics', then turn on 'R style diagnostics' and press 'OK'. Now open a new R script, type a line of code that uses poor style (e.g. `x<-9`), and save the file.

Which of the following symbols does R use to denote poor style?

`@possible_answers`
- Red 'X'
- Yellow '!'
- Blue 'i'

`@hint`
A style error is different than a code error, like leaving out a parenthesis or misspelling a variable name. A style error makes your code hard to understand, but does not stop it from working. Create a style error in the text editor by leaving out white space around `<-`. What symbol appears?

`@sct`
```{r,eval=FALSE}
msg1 <- "Nice work!"
msg2 <- "Incorrect. Create a style error in the text editor by leaving out white spaces around `<-`. What symbol appears?"

test_mc(3, feedback_msgs = c(msg2, msg2, msg1))
```

`@attachments`


---

## Fixing code in the text editor using diagnostic features

```yaml
type: RStudioMultipleChoiceExercise
key: c5766a6ab4
skills: 1
```

Examine `my_code.R` and fix the code in the editor using the diagnostic features. (You may need to make a change to the file and save it before the diagnostics appear.)

Once you solve the problem, what is the answer to `my_function(14)`?

`@possible_answers`
- 120
- 1200
- 2940
- 210

`@hint`
The function is missing a closing `}`!

`@sct`
```{r,eval=FALSE}
msg1 <- "Hmm, try again! Use the diagnostic symbols to identify and fix the errors."
msg2 <- "Nice work!"
test_mc(4, feedback_msgs = c(msg1, msg1, msg1, msg2))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_05_my_code.R open

---

## Keyboard shortcuts

```yaml
type: VideoExercise
key: 727ab62aa8
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871427
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v2/hls-3-3.master.m3u8
```


---

## Generating the pipe operator using keyboard shortcuts

```yaml
type: RStudioMultipleChoiceExercise
key: 01bab0ca72
skills: 1
```

The keyboard shortcut to generate the pipe operator, `%>%`, is 'Command + Shift + M' (Mac) or 'Control + Shift + M' (PC).

In `my_code.R`, replace the blank space in the script with the pipe operator, and execute the code. What is the output?

`@possible_answers`
- 1.24328
- 2.33245
- 3.21725
- 3.45543

`@hint`
Make sure you've loaded the `dplyr` package!

`@sct`
```{r,eval=FALSE}
msg1 <- "Awesome!"
msg2 <- "Try again. Replace the blank space with the pipe operator using the keyboard shortcut and execute the code. What's the output?"

test_mc(3, feedback_msgs = c(msg2, msg2, msg1, msg2))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_07_my_code.R open

---

## Trying new shortcuts

```yaml
type: RStudioMultipleChoiceExercise
key: 3caadee278
skills: 1
```

What is the effect of highlighting code and hitting 'Control + Shift + C'? Feel free to try it out on the code in `my_code.R`.

`@possible_answers`
- Executes your code in the console
- Saves your code in a .R file for future use
- Either comments or uncomments your code

`@hint`
Try each option, which one works?

`@sct`
```{r,eval=FALSE}
msg1 <- "Great, this can be super helpful!"
msg2 <- "Did you try these options in the text editor? Did they work?"

test_mc(3, feedback_msgs = c(msg2, msg2, msg1))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_08_my_code.R open

---

## Discovering new shortcuts

```yaml
type: RStudioMultipleChoiceExercise
key: e0dfdf55d5
skills: 1
```

Remembering all these shortcuts can be hard, but luckily there is an easy way to find them with 'Option + Shift + K' (Mac) or 'Alt + Shift + K' (PC). Hit the escape key when you wish to close the list.

What is the keyboard shortcut for showing the *history tab* within the environment pane?

`@possible_answers`
- 'Control + 4'
- 'Control + 8'
- 'Shift + 4'
- 'Shift + 8'

`@hint`
Try each option...which one works? Once you get to the shortcut menu, look under the 'Panes' section.

`@sct`
```{r,eval=FALSE}
msg1 <- "Great job! You can also access the 'Keyboard Shortcuts Help' at any time from the 'Tools' menu."
msg2 <- "Incorrect. Use 'Option + Shift + K' (Mac) and 'Alt + Shift + K' (PC) to determine the correct answer."

test_mc(1, feedback_msgs = c(msg1, msg2, msg2, msg2))
```

`@attachments`


---

## Multiple cursors

```yaml
type: VideoExercise
key: d2a9c00502
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871425
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v2/hls-3-4.master.m3u8
```


---

## How do we use multiple cursors?

```yaml
type: RStudioMultipleChoiceExercise
key: bd9f85521f
skills: 1
```

What is the command for using multiple cursors? Give them each a try in `my_code.R`!

`@possible_answers`
- 'Control + Shift'
- 'Control + Option' (Mac) or 'Control + Alt' (PC)
- 'Shift + Option' (Mac) or 'Shift + Alt' (PC)
- 'Shift + C'

`@hint`
Try each option, which one works?

`@sct`
```{r,eval=FALSE}
msg1 <- "Great job!"
msg2 <- "Incorrect, try each option in the console. Which one works?"

test_mc(2, feedback_msgs = c(msg2, msg1, msg2, msg2))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_13_my_code.R open

---

## Navigate & edit code

```yaml
type: VideoExercise
key: b6ec116bf8
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871424
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v2/hls-3-5.master.m3u8
```


---

## Finding the right line of code

```yaml
type: RStudioMultipleChoiceExercise
key: 23fd7997bf
skills: 1
```

Your colleague mentions that they spotted an error in line 543 of your code. What command could you use to quickly navigate to that line?

Give it a try with a smaller number in `my_code.R`!

`@possible_answers`
- 'Command + Shift + Option + A' (Mac) or 'Shift + Alt + A' (PC)
- 'Command + Shift + Option + F' (Mac) or 'Shift + Alt + F' (PC)
- 'Command + Shift + Option + G' (Mac) or 'Shift + Alt + G' (PC)

`@hint`
Try each option, which one works?

`@sct`
```{r,eval=FALSE}
msg1 <- "Great job!"
msg2 <- "Incorrect, try each option in the console. Which one works?"

test_mc(3, feedback_msgs = c(msg2, msg2, msg1))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_13_my_code.R open

---

## Closing folds

```yaml
type: RStudioMultipleChoiceExercise
key: 3b307896e8
skills: 1
```

Which command quickly closes all folds in the text editor? Give them a try in `my_code.R` to see what works!

`@possible_answers`
- 'Command + Option + C' (Mac) or 'Alt + C' (PC)
- 'Command + Option + Y' (Mac) or 'Alt + Y' (PC)
- 'Command + Option + O' (Mac) or 'Alt + O' (PC)

`@hint`
Try each option, which one works?

`@sct`
```{r,eval=FALSE}
msg1 <- "Nice! This is a quick way to make your script look more organized."
msg2 <- "Incorrect, try each option in the console. Which one works?"

test_mc(3, feedback_msgs = c(msg2, msg2, msg1))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_14_my_code.R open

---

## Run scripts

```yaml
type: VideoExercise
key: 5ab952af02
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871433
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v2/hls-3-6.master.m3u8
```


---

## Sourcing an entire file

```yaml
type: RStudioMultipleChoiceExercise
key: d5c19f6df4
skills: 1
```

Which command sources an entire file and runs it in the R console? Go ahead and give it a try on `my_code.R`.

`@possible_answers`
- 'Command + Option + Enter' (Mac) or 'Control + Alt + Enter' (PC)
- 'Command + Shift + Enter' (Mac) or 'Control + Shift + Enter' (PC)
- 'Option + Enter' (Mac) or 'Alt + Enter' (PC)
- 'Command + Option' (Mac) or 'Control + Alt' (PC)

`@hint`
Try each option, which one works?

`@sct`
```{r,eval=FALSE}
msg1 <- "Nice! This is a quick way to source an entire file!"
msg2 <- "Incorrect, try each option in the console. Which one works?"

test_mc(2, feedback_msgs = c(msg2, msg1, msg2, msg2))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_16_my_code.R open

---

## Refreshing your R session

```yaml
type: RStudioMultipleChoiceExercise
key: 70877f7f5b
skills: 1
```

Which command refreshes your R session? Remember to press the 'fn' (function) key if your keyboard requires it.

`@possible_answers`
- 'Command + Shift + F11' (Mac) or 'Control + Shift + F11' (PC)
- 'Command + Option + F11' (Mac) or 'Control + Alt + F11' (PC)
- 'Command + Shift + F10' (Mac) or 'Control + Shift + F10' (PC)
- 'Command + Shift + F8' (Mac) or 'Control + Alt + F8' (PC)

`@hint`
Try each option in the console. Which one works?

`@sct`
```{r,eval=FALSE}
msg1 <- "Nice! This can allow you to test whether your R code relies on packages or objects that are not created within the code itself. This is important when you are testing to make sure your results are reproducible and when you are sharing your work with others. To ensure that RStudio does not reload objects into the new R session, uncheck the global options 'Restore .RData into workspace at startup' and 'Save workspace to .RData on exit'."
msg2 <- "Incorrect, try each option in the console. Which one works?"

test_mc(3, feedback_msgs = c(msg2, msg2, msg1, msg2))
```

`@attachments`
.RData: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_17_.RData 

---

## Traceback

```yaml
type: VideoExercise
key: 9f676365ca
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871430
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v2/hls-3-7.master.m3u8
```


---

## Finding an error

```yaml
type: RStudioMultipleChoiceExercise
key: 852bc13096
skills: 1
```

In the console, call each of the following functions on `x`. Which function throws an error?

`@possible_answers`
- `center()`
- `manipulate()`
- `rescale()`

`@hint`
Try each option. Which one throws and error and why?

`@sct`
```{r,eval=FALSE}
msg1 <- "Nope!"
msg2 <- "Incorrect. Execute the code in the console and see where the error is occuring."
msg3 <- "Yes! There was an error within the `rescale()` function because `ss()` is not a valid function. It is supposed to be `sd()`!"

test_mc(3, feedback_msgs = c(msg1, msg2, msg3))
```

`@attachments`
.RData: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_19_.RData 

---

## Debugger mode

```yaml
type: VideoExercise
key: 519a06043b
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871432
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v2/hls-3-8.master.m3u8
```


---

## Getting comfortable with debugger mode

```yaml
type: RStudioMultipleChoiceExercise
key: 2958a7f17c
skills: 1
```

Execute the code in `my_code.R` as-is. When you receive an error, click "Rerun with Debug" in the console, which will cause you to enter debugger mode.

What exactly is highlighted as the source of the error within the editor?

`@possible_answers`
- `(x * 2) + meen(x)`
- `meen(x)`
- `meen`
- `manipulate`

`@hint`
Make sure you are looking at the editor, not the 'Traceback' window. You will need to enter debugger mode to see the correct answer.

`@sct`
```{r,eval=FALSE}
msg1 <- "Nice job!"
msg2 <- "Incorrect. What does the error message say?"

test_mc(1, feedback_msgs = c(msg1, msg2, msg2, msg2))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_21_my_code.R open

---

## Exiting debugger mode

```yaml
type: RStudioMultipleChoiceExercise
key: e70aac2f04
skills: 1
```

Execute the code in `my_code.R` as-is and enter debugger mode. What letter can you type in the console to exit debugger mode?

`@possible_answers`
- R
- E
- F
- Q

`@hint`
Try each option. Which one works? Note that case matters!

`@sct`
```{r,eval=FALSE}
msg1 <- "Nice job!"
msg2 <- "Incorrect. Try each option in the console and see which one works!"

test_mc(4, feedback_msgs = c(msg2, msg2, msg2, msg1))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_21_my_code.R open

---

## Debugger mode: breakpoints

```yaml
type: VideoExercise
key: 1a9bcd97c5
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/143871434
video_hls: //videos.datacamp.com/transcoded/1078_rstudio_ide_1/v2/hls-3-9.master.m3u8
```


---

## Programmatically opening debugger mode (1)

```yaml
type: RStudioMultipleChoiceExercise
key: 5d683d7fee
skills: 1
```

RStudio will enter debugger mode anytime it encounters a call to `browser()`.

True or false?

`@possible_answers`
- True
- False

`@hint`
Try adding `browser()` to one of the functions in the editor and execute the code.

`@sct`
```{r,eval=FALSE}
msg1 <- "Nice job!"
msg2 <- "Incorrect, RStudio will enter debugger mode whenever it encounters a call for `browser()`."

test_mc(1, feedback_msgs = c(msg1, msg2))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_21_my_code.R open

---

## Programmatically entering debugger mode (2)

```yaml
type: RStudioMultipleChoiceExercise
key: 3e3003b296
skills: 1
```

The command `options(error = browser)` will automatically enter debugger mode whenever an error occurs. What command will stop this?

`@possible_answers`
- `options(error = browser) <- NULL`
- `options(error = NULL)`
- `options(browser = NULL)`

`@hint`
You can experiment in the console or rewatch the video to determine the correct answer!

`@sct`
```{r,eval=FALSE}
msg1 <- "Nice job!"
msg2 <- "Incorrect. You can experiment in the console or rewatch the video to determine the correct answer!"

test_mc(2, feedback_msgs = c(msg2, msg1, msg2))
```

`@attachments`
my_code.R: https://s3.amazonaws.com/assets.datacamp.com/production/course_944/datasets/ex2_21_my_code.R open
