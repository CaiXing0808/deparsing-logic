---
title: Laws of probability
description: >-
  In this chapter you'll learn to combine multiple probabilities, such as the
  probability two events both happen or that at least one happens, and confirm
  each with random simulations. You'll also learn some of the properties of
  adding and multiplying random variables.
attachments:
    slides_link: https://s3.amazonaws.com/assets.datacamp.com/production/course_2351/slides/chapter2.pdf
--- type:VideoExercise lang:r xp:50 skills:1 key:e9212c0063
## Probability of event A and event B

*** =video_link
//player.vimeo.com/video/154783078

*** =video_hls
//videos.datacamp.com/transcoded/000_placeholders/v1/hls-temp.master.m3u8

*** =projector_key
d451bf1d19260503164eecc485a44707

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:f69cf68e2f
## Solving for probability of A and B

If events A and B are independent, and A has a 40% chance of happening, and event B has a 20% chance of happening, what is the probability they will both happen?

*** =instructions

- 8%
- 12%
- 20%
- 60%

*** =hint

To find the probability independent events A and B both happen, multiply their probabilities.

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
msg <- "Not exactly. How do you calculate the probability that two independent events both occur?"
test_mc(1,
        feedback_msgs = c("That's right! You're a probability whiz!", msg, msg, msg))

```

--- type:NormalExercise lang:r xp:100 skills:1 key:10ef181aa5
## Simulating the probability of A and B

You can also use simulation to estimate the probability of two events both happening. 

*** =instructions


* Randomly simulate 100,000 flips of coin A, each of which has a 40% chance of being heads. Save this as a variable `A`.
* Randomly simulate 100,000 flips of coin B, each of which has a 20% chance of being heads. Save this as a variable `B`.
* Use the "and" operator (`&`) to combine the variables `A` and `B` to estimate the probability that both A and B are heads.


*** =hint

After combining the simulated vectors `A & B`, you can use `mean` to find the fraction where they are both true.

*** =pre_exercise_code
```{r}
set.seed(2017)
```

*** =sample_code
```{r}
# Simulate 100,000 flips of a coin with a 40% chance of heads
A <- 

# Simulate 100,000 flips of a coin with a 20% chance of heads
B <- 

# Estimate the probability both A and B are heads

```

*** =solution
```{r}
# Simulate 100,000 flips of a coin with a 40% chance of heads
A <- rbinom(100000, 1, .4)

# Simulate 100,000 flips of a coin with a 20% chance of heads
B <- rbinom(100000, 1, .2)

# Estimate the probability both A and B are heads
mean(A & B)
```

*** =sct
```{r}
test_function("rbinom", 1, args = c("n", "size", "prob"), incorrect_msg = "Check your first `rbinom()` call.")

test_function("rbinom", 2, args = c("n", "size", "prob"), incorrect_msg = "Check your second `rbinom()` call.")

test_object("A", incorrect_msg = "Did you save the results of your first simulation in the variable `A`?")
test_object("B", incorrect_msg = "Did you save the results of your second simulation in the variable `B`?")

test_function("mean", args = "x", incorrect_msg = "Find the mean of `A & B` to estimate the probability of A and B.")
success_msg("You're a whiz at this simulation stuff!")

```

--- type:NormalExercise lang:r xp:100 skills:1 key:b7781e3bcf
## Simulating the probability of A, B, and C

Randomly simulate 100,000 flips of A (40% chance), B (20% chance), and C (70% chance). What fraction of the time do all three coins come up heads?

*** =instructions

- You've already simulated A and B. Now simulate 100,000 flips of coin C, where each has a 70% chance of coming up heads.
- Use `A`, `B`, and `C` to estimate the probability that all three coins would come up heads.

*** =hint

You can compute `A & B & C` instead of just `A & B`, using `mean()` to find the fraction where all three are true.

*** =pre_exercise_code
```{r}
set.seed(2017)
```

*** =sample_code
```{r}
# You've already simulated 100,000 flips of coins A and B
A <- rbinom(100000, 1, .4)
B <- rbinom(100000, 1, .2)

# Simulate 100,000 flips of coin C (70% chance of heads)


# Estimate the probability A, B, and C are all heads

```

*** =solution
```{r}
# You've already simulated 100,000 flips of coins A and B
A <- rbinom(100000, 1, .4)
B <- rbinom(100000, 1, .2)

# Simulate 100,000 flips of coin C (70% chance of heads)
C <- rbinom(100000, 1, .7)

# Estimate the probability A, B, and C are all heads
mean(A & B & C)
```

*** =sct
```{r}
msg <- "Check your %s call of `rbinom()`."

test_function("rbinom", 1, args = c("n", "size", "prob"), incorrect_msg = sprintf(msg, "first"))

test_function("rbinom", 2, args = c("n", "size", "prob"), incorrect_msg = sprintf(msg, "second"))

test_function("rbinom", 3, args = c("n", "size", "prob"), incorrect_msg = sprintf(msg, "third"))

msg <- "Did you save the results of your %s simulation in the variable `%s`?"
test_object("A", incorrect_msg = sprintf(msg, "first", "A"))
test_object("B", incorrect_msg = sprintf(msg, "second", "B"))
test_object("C", incorrect_msg = sprintf(msg, "third", "C"))

test_function("mean", args = "x", incorrect_msg = "Use the `mean()` function to estimate the probability of `A & B & C`.")

test_error()

success_msg("Great job! That was much easier than using a nasty formula.")
```

--- type:VideoExercise lang:r xp:50 skills:1 key:c3bcd571cd
## Probability of A or B

*** =video_link
//player.vimeo.com/video/154783078

*** =video_hls
//videos.datacamp.com/transcoded/000_placeholders/v1/hls-temp.master.m3u8

*** =projector_key
4c8715a274e1cb0151bdda37c4203448

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:66d3fde0a4
## Solving for probability of A or B

If coins A and B are independent, and A has a 60% chance of coming up heads, and event B has a 10% chance of coming up heads, what is the probability either A or B will come up heads?

*** =instructions

- 6%
- 50%
- 64%
- 70%

*** =hint

Recall that the probability of A or B happening (when A and B are independent, as they are here) is P(A) + P(B) - P(A) * P(B).

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
msg <- "Remember the formula $Pr(A | B) = Pr(A) + Pr(B) - Pr(A \\text{ and } B)$"
test_mc(3, feedback_msgs = c(msg, msg, "That's right, great job!", msg))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:7c4c5361ba
## Simulating probability of A or B

In the last multiple choice exercise you found that there was a 64% chance that either coin A (60% chance) or coin B (10% chance) would come up heads. Now you'll confirm that answer using simulation.

*** =instructions

- Use `rbinom()` to simulate 100,000 flips of coin A, each having a 60% chance of being heads.
- Use `rbinom()` to simulate 100,000 flips of coin B, each having a 10% chance of being heads.
- Use these to estimate the probability that A or B is heads.

*** =hint

This is similar to the exercise where you estimated the probability of both A and B happening, except you'll use `|` ("or") instead of `&` ("and") to combine A and B.

*** =pre_exercise_code
```{r}
set.seed(2017)
```


*** =sample_code
```{r}
# Simulate 100,000 flips of a coin with a 60% chance of heads
A <- 

# Simulate 100,000 flips of a coin with a 10% chance of heads
B <- 

# Estimate the probability either A or B is heads

```

*** =solution
```{r}
# Simulate 100,000 flips of a coin with a 60% chance of heads
A <- rbinom(100000, 1, .6)

# Simulate 100,000 flips of a coin with a 10% chance of heads
B <- rbinom(100000, 1, .1)

# Estimate the probability either A or B is heads
mean(A | B)
```

*** =sct
```{r}
test_function("rbinom", 1, args = c("n", "size", "prob"), incorrect_msg = "Check your first `rbinom()` call.")

test_function("rbinom", 2, args = c("n", "size", "prob"), incorrect_msg = "Check your second `rbinom()` call.")

test_object("A", incorrect_msg = "Did you save your first simulation as the variable `A`?")
test_object("B", incorrect_msg = "Did you save your second simulation as the variable `B`?")

test_function("mean", args = "x", incorrect_msg = "Use the `mean()` function to estimate the probability of `A | B`.")

success_msg("Great work! How does the simulated probability compare to the one you calculated in the last exercise?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:e4532be4c0
## Probability either variable is less than or equal to 4

Suppose X is a random Binom(10, .6) variable (10 flips of a coin with 60% chance of heads) and Y is a random Binom(10, .7) variable (10 flips of a coin with a 70% chance of heads), and they are independent.

What is the probability that either of the variables is less than or equal to 4?

*** =instructions

- Simulate 100,000 draws from each of X (10 coins, 60% chance of heads) and Y (10 coins, 70% chance of heads) binomial variables, saving them as `X` and `Y` respectively.
- Use these simulations to estimate the probability that either X or Y is less than or equal to 4.
- Use the `pbinom()` function to calculate the exact probability that X is less than or equal to 4, then the probability that Y is less than or equal to 4.
- Combine these two exact probabilities to calculate the exact probability that either variable is less than or equal to 4.

*** =hint

Recall that the probability of events A or B is P(A) + P(B) - P(A) * P(B). You can use the probability X <= 4 as P(A) and the probability Y <= 4 as P(B).

*** =pre_exercise_code
```{r}
set.seed(2017)
```

*** =sample_code
```{r}
# Use rbinom to simulate 100,000 draws from each of X and Y
X <- 
Y <- 

# Estimate the probability either X or Y is <= to 4


# Use pbinom to calculate the probabilities separately
prob_X_less <- 
prob_Y_less <- 

# Combine these to calculate the exact probability either <= 4

```

*** =solution
```{r}
# Use rbinom to simulate 100,000 draws from each of X and Y
X <- rbinom(100000, 10, .6)
Y <- rbinom(100000, 10, .7)

# Estimate the probability either X or Y is <= to 4
mean(X <= 4 | Y <= 4)

# Use pbinom to calculate the probabilities separately
prob_X_less <- pbinom(4, 10, .6)
prob_Y_less <- pbinom(4, 10, .7)

# Combine these to calculate the exact probability either <= 4
prob_X_less + prob_Y_less - prob_X_less * prob_Y_less
```

*** =sct
```{r}
test_function("rbinom", 1, args = c("n", "size", "prob"), incorrect_msg = "Check your first `rbinom()` call.")
test_function("rbinom", 2, args = c("n", "size", "prob"), incorrect_msg = "Check your second `rbinom()` call.")

test_object("X", incorrect_msg = "Did you save the results of your first simulation in the variable `X`?")
test_object("Y", incorrect_msg = "Did you save the results of your second simulation in the variable `Y`?")

test_function("mean", args = "x", incorrect_msg = "Use the `mean()` function to estimate the probability that `X <= 4` or `Y <= 4`.")

test_function("pbinom", 1, args = c("q", "size", "prob"), incorrect_msg = "Check your first `pbinom()` call.")
test_function("pbinom", 2, args = c("q", "size", "prob"), incorrect_msg = "Check your second `pbinom()` call.")

test_object("prob_X_less", incorrect_msg = "Did you save the output of your first `pbinom()` call as `prob_X_less`?")
test_object("prob_Y_less", incorrect_msg = "Did you save the output of your second `pbinom()` call as `prob_Y_less`?")

test_output_contains("prob_X_less + prob_Y_less - prob_X_less * prob_Y_less", incorrect_msg = "Did you correctly calculate the exact probability that either `X` or `Y` are less than or equal to 4? Take a second look at the formula in the previous video.")

success_msg("Awesome! Simulation is a great way of avoiding difficult calculations.")
```

--- type:VideoExercise lang:r xp:50 skills:1 key:d878209f6b
## Multiplying random variables

*** =video_link
//player.vimeo.com/video/154783078

*** =video_hls
//videos.datacamp.com/transcoded/000_placeholders/v1/hls-temp.master.m3u8

*** =projector_key
a2ca2fe57b1b1d94cce3bd42e76b7bc5

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:e4bbf0c82c
## Expected value of multiplying a random variable

If X is a binomial with size 50 and p = .4, what is the expected value of 3*X?

*** =instructions

- 1.2
- 20
- 60
- 150

*** =hint

The expected value of a binomial is size * p, and the expected value of k * X is k * E[X].

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
msg <- "Don't forget that $E[k \\cdot X] = k \\cdot E[x].$"
test_mc(3, feedback_msgs = c(msg, msg, "Great, you got it!", msg))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:20220f7b46
## Simulating multiplying a random variable

In this exercise you'll use simulation to confirm the rule you just learned about how multiplying a random variable by a constant effects its expected value.

*** =instructions
- Simulate 100,000 draws of X, a binomial random variable with size 20 and p = .1. Save this as `X`
- Use this simulation to estimate the expected value of X.
- Use this simulation to estimate the expected value of 5*X, as well.

*** =hint

You can find the expected value of X with `mean(X)`, and by changing `X` to `5 * X` you can find that expected value as well.

*** =pre_exercise_code
```{r}
set.seed(2017)
```

*** =sample_code
```{r}
# Simulate 100,000 draws of a binomial with size 20 and p = .1
X <- ___

# Estimate the expected value of X


# Estimate the expected value of 5 * X

```

*** =solution
```{r}
# Simulate 100,000 draws of a binomial with size 20 and p = .1
X <- rbinom(100000, 20, .1)

# Estimate the expected value of X
mean(X)

# Estimate the expected value of 5 * X
mean(5 * X)
```

*** =sct
```{r}
test_function("rbinom", args = c("n", "size", "prob"), incorrect_msg = "Double check your call to `rbinom()`.")
test_object("X", incorrect_msg = "Did you save the results of you simulation as the variable `X`?")

test_function("mean", 1, args = "x", incorrect_msg = "Use the `mean()` function to estimate the expected value of `X`.")

test_function("mean", 2, args = "x", incorrect_msg = "Use the `mean()` function to estimate the expected value of `5 * X`.")

success_msg("Fantastic! You're a pro at this simulation stuff!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:ddec8f1ed8
## Variance of a multiplied random variable

In the last exercise you simulated X from a binomial with size 20 and p = .1 and now you'll use this same simulation to explore the variance.

*** =instructions

- Use this simulation to estimate the variance of X.

- Estimate the variance of 5 * X

*** =hint

Use `var()` to find the variance of each simulated variable.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# X is simulated from 100,000 draws of a binomial with size 20 and p = .1
X <- rbinom(100000, 20, .1)

# Estimate the variance of X


# Estimate the variance of 5 * X
```

*** =solution
```{r}
# X is simulated from 100,000 draws of a binomial with size 20 and p = .1
X <- rbinom(100000, 20, .1)

# Estimate the variance of X
var(X)

# Estimate the variance of 5 * X
var(5 * X)
```

*** =sct
```{r}
test_function("rbinom", args = c("n", "size", "prob"), incorrect_msg = "Double check your call to `rbinom()`.")
test_object("X", incorrect_msg = "Did you save the results of you simulation as the variable `X`?")

test_function("var", 1, args = "x", incorrect_msg = "Use the `var()` function to estimate the variance of `X`.")

test_function("var", 2, args = "x", incorrect_msg = "Use the `var()` function to estimate the variance of `5 * X`.")

success_msg("Stellar work! How does the variance compare to the mean?")
```

--- type:VideoExercise lang:r xp:50 skills:1 key:85636a04d3
## Adding two random variables

*** =video_link
//player.vimeo.com/video/154783078

*** =video_hls
//videos.datacamp.com/transcoded/000_placeholders/v1/hls-temp.master.m3u8

*** =projector_key
03d6a6937a3219af4d73f6ffd2277b40

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:d49c4279bd
## Solving for the sum of two binomial variables

If X is drawn from a binomial with size 20 and p = .3, and Y from size 40 and p = .1, what is the expected value (mean) of X + Y?

*** =instructions

- 4
- 6
- 10
- 60

*** =hint

Compute the expected value of X and the expected value of Y separately, then add them together.

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
test_mc(3, feedback_msgs = c("That's the expected value of Y",
                             "That's the expected value of X",
                             "Great work!",
                             "Double check your calculation."))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:08d706e3fc
## Simulating adding two binomial variables

In the last multiple choice exercise, you found the expected value of the sum of two binomials. In this problem you'll use a simulation to confirm your answer.

*** =instructions

- Simulate 100,000 draws from X, a binomial with size 20 and p = .3, and Y, with size 40 and p = .1.
- Use this simulation to estimate the expected value of X + Y.

*** =hint

You can take the mean of `X + Y` to find the expected value of the sum.

*** =pre_exercise_code
```{r}
set.seed(2017)
```

*** =sample_code
```{r}
# Simulate 100,000 draws of X (size 20, p = .3) and Y (size 40, p = .1)
X <-
Y <-

# Estimate the expected value of X + Y

```

*** =solution
```{r}
# Simulate 100,000 draws of X (size 20, p = .3) and Y (size 40, p = .1)
X <- rbinom(100000, 20, .3) 
Y <- rbinom(100000, 40, .1)

# Estimate the expected value of X + Y
mean(X + Y)
```

*** =sct
```{r}
test_function("rbinom", 1, args = c("n", "size", "prob"), incorrect_msg = "Double check your first call of `rbinom()`.")
test_function("rbinom", 2, args = c("n", "size", "prob"), incorrect_msg = "Double check your second call of `rbinom()`.")

test_object("X", incorrect_msg = "Did you save your first simulation as `X`?")
test_object("Y", incorrect_msg = "Did you save your second simulation as `Y`?")

test_or(
    test_function("mean", args = "x"),
    ex() %>% 
        override_solution("mean(X) + mean(Y)") %>% 
        {
            check_function(., "mean", 1) %>% 
            check_arg("x") %>% 
            check_equal(incorrect_msg = "Make sure you've found the mean of both `X` and `Y`.")
            
            check_function(., "mean", 2) %>% 
            check_arg("x") %>% 
            check_equal(incorrect_msg = "Make sure you've found the mean of both `X` and `Y`.")
        }
)

success_msg("Great job! The fact that you can add the means makes stuff much simpler.")

```

--- type:NormalExercise lang:r xp:100 skills:1 key:b5c35d8632
## Simulating variance of sum of two binomial variables

In the last multiple choice exercise, you examined the expected value of the sum of two binomials. Here you'll estimate the variance.

*** =instructions

- Use your simulation of the variables X and Y to estimate the variance of `X + Y`.
- Use your simulation to estimate the variance of `3 * X + Y`.

*** =hint

You can find the variance of X + Y with `var(X + Y)`, and can change that formula to find the second variance.

*** =pre_exercise_code
```{r}
set.seed(2017)
```

*** =sample_code
```{r}
# Simulation from last exercise of 100,000 draws from X and Y
X <- rbinom(100000, 20, .3) 
Y <- rbinom(100000, 40, .1)

# Find the variance of X + Y


# Find the variance of 3 * X + Y

```

*** =solution
```{r}
# Simulation from last exercise of 100,000 draws from X and Y
X <- rbinom(100000, 20, .3) 
Y <- rbinom(100000, 40, .1)

# Find the variance of X + Y
var(X + Y)

# Find the variance of 3 * X + Y
var(3 * X + Y)
```

*** =sct
```{r}
test_function("rbinom", 1, args = c("n", "size", "prob"), incorrect_msg = "Double check your first call of `rbinom()`.")
test_function("rbinom", 2, args = c("n", "size", "prob"), incorrect_msg = "Double check your second call of `rbinom()`.")

test_object("X", incorrect_msg = "Did you save your first simulation as `X`?")
test_object("Y", incorrect_msg = "Did you save your second simulation as `Y`?")

test_function("var", 1, args = "x", incorrect_msg = "Use the `var()` function to estimate the variance of `X + Y`.")
test_function("var", 2, args = "x", incorrect_msg = "Use the `var()` function to estimate the variance of `3 * X + Y`.")

success_msg("Great simulating! Remember this rule only works when X and Y are independent.")
```
