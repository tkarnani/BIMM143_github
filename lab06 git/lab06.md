# Lab 6: Gradebook
Tusha (A17339806)

The below is my automated gradebook project for lab06:

``` r
# Reading in the csv file for the gradebook
data <- read.csv(file="student_homework.csv", row.names = 1)

# Other gradebook vectors for testing
student1 <- c(100, 100, 100, 100, 100, 100, 100, 90)
student2 <- c(100, NA, 90, 90, 90, 90, 97, 80)
student3 <- c(90, NA, NA, NA, NA, NA, NA, NA)
```

***Question 1***

The grade function:

``` r
grade <- function(scores)
{
  # Making a copy of the vector to modify
  scores_for_grading <- scores
  
  # is.na() creates a logical vector for scores in the input vector that are missing
  # We then replace those scores with -1 (lower than any possible score) to make sure that it is not included in our calculations
  scores_for_grading[is.na(scores_for_grading)] <- -1
  
  # Check which score is the lowest
  lowest_index <- which.min(scores_for_grading)
  scores_without_lowest <- scores_for_grading[-lowest_index]
  
  # If there are any unresolved NAs, we remove them
  scores_without_lowest <- scores_without_lowest[scores_without_lowest != -1]
  
  # Compute the mean
  mean(scores_without_lowest)
}
```

Testing:

``` r
student3_grade <- grade(student3)
print(student3_grade)
```

    [1] 90

***Question 2***

To find out who has the top scoring student in the class is, we have to
calculate average scores for everyone and determine who has the highest
one.

``` r
# This applies the grade function to each row of the gradebook, i.e. once for every student, and saves the returned scores as a vector
final_grades <- apply(data, 1, grade)
print(final_grades)
```

     student-1  student-2  student-3  student-4  student-5  student-6  student-7 
         91.75      82.50      84.25      84.25      88.25      89.00      94.00 
     student-8  student-9 student-10 student-11 student-12 student-13 student-14 
         93.75      87.75      79.00      86.00      91.75      92.25      87.75 
    student-15 student-16 student-17 student-18 student-19 student-20 
         78.75      89.50      88.00      94.50      82.75      82.75 

``` r
top_scorer_index <- which.max(final_grades)
print(top_scorer_index)
```

    student-18 
            18 

Therefore, the top scoring student in the class is Student 18.

***Question 3***

To find out which homework was the toughest, we have to calculate
average scores for each and determine which led to students performing
the worst.

``` r
# This applies the grade function to each column of the gradebook, i.e. once for every homework, and saves the returned scores as a vector
final_grades_per_hw <- apply(data, 2, grade)
print(final_grades_per_hw)
```

         hw1      hw2      hw3      hw4      hw5 
    89.36842 80.88889 81.21053 89.63158 83.42105 

``` r
lowest_score_index <- which.min(final_grades_per_hw)
print(lowest_score_index)
```

    hw2 
      2 

Therefore, the toughest homework was Homework 2.
