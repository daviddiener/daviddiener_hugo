---
title: AOC 2023 - Day 1 Trebuchet?!
date: 2023-12-01
tags:
  - rust
hidden: true
---

## [Overview Advent Of Code 2023]({{< ref "/posts/aoc2023/challenge" >}})

{{< button
text="Check out my solutions on GitHub" 
link="https://github.com/daviddiener/AdventOfCode2023" 
>}}

## Task Description
You can look up the tasks for day one on the [AOC 2023 website](https://adventofcode.com/2023/day/1). My puzzle input for day one is included in my [AOC 2023 GitHub Repository](https://github.com/daviddiener/AdventOfCode2023/blob/master/src/days/inputs/day_01.txt).

## Part One: Recover calibration values by combining the first and last digits on each line.

In this part of the puzzle, the task is to recover calibration values by combining the first and last digits present on each line of the input file. The Rust code achieves this by performing the following steps:

- We reads the contents of the input file line by line using BufReader and extract the digits from each line.
- We filters out non-digit characters, leaving only numeric digits.
- We take the first and last digits, convert them into numbers, and combine them to form a new number.
- These new combined numbers are stored in a vector named combined_numbers.
- Finally, we calculate the sum of all combined numbers and print the result.

The final answer obtained for Part One of the puzzle is **55816*.

```rust
pub fn star_one() -> Result<(), std::io::Error> {
    let file = File::open("src/inputs/day_01.txt")?;
    let reader = BufReader::new(file);

    let mut combined_numbers: Vec<u32> = Vec::new();

    for line in reader.lines() {
        if let Ok(digits) = line {
            let filtered_digits: String = digits.chars().filter(|c| c.is_digit(10)).collect();

            if let (Some(first), Some(last)) = (filtered_digits.chars().next(), filtered_digits.chars().last()) {
                if let (Some(first_digit), Some(last_digit)) = (first.to_digit(10), last.to_digit(10)) {
                    let combined = first_digit * 10 + last_digit;
                    combined_numbers.push(combined);
                }
            }
        }
    }

    println!("Answer: {:?}", combined_numbers.iter().sum::<u32>());

    Ok(())
}
```

## Part Two: Discover the real digits, even when spelled out with letters, and compute the sum of calibration values.

For the second part of the puzzle, the goal is to handle scenarios where numbers are spelled out in words (e.g., "zero," "one," "two") within the input. I addressed this by doing the following:

- Setting up up a dictionary spelled_to_int_dict that pairs spelled-out numbers with their respective representations as digits. To cover edge cases, where the spelled out numbers overlap (e.g. eighthree), I include the first and last character of the digit in the replacement value. This was something I overlooked on my first attempt to solve this puzzle.
- Then we proceed similar, as in part one.
- The final answer obtained for Part Two of the puzzle is **54980**.

```rust
pub fn star_two() -> Result<(), std::io::Error> {
    let file = File::open("src/inputs/day_01.txt")?;
    let reader = BufReader::new(file);

    let spelled_to_int_dict: Vec<(String, String)> = vec![
        ("zero".to_string(), "z0o".to_string()),
        ("one".to_string(), "o1e".to_string()),
        ("two".to_string(), "t2o".to_string()),
        ("three".to_string(), "t3e".to_string()),
        ("four".to_string(), "f4r".to_string()),
        ("five".to_string(), "f5e".to_string()),
        ("six".to_string(), "s6x".to_string()),
        ("seven".to_string(), "s7n".to_string()),
        ("eight".to_string(), "e8t".to_string()),
        ("nine".to_string(), "n9e".to_string()),
    ];

    let mut combined_numbers: Vec<u32> = Vec::new();

    for line in reader.lines() {
        if let Ok(digits) = line {
            // check if the line contains any of the spelled out numbers and convert it to a digit
            let mut converted_digits: String = digits.clone();
            for (spelled, digit) in spelled_to_int_dict.iter() {
                converted_digits = converted_digits.replace(spelled, &digit.to_string());
            }
           
            let filtered_digits: String = converted_digits.chars().filter(|c| c.is_digit(10)).collect();

            if let (Some(first), Some(last)) = (filtered_digits.chars().next(), filtered_digits.chars().last()) {
                if let (Some(first_digit), Some(last_digit)) = (first.to_digit(10), last.to_digit(10)) {
                    let combined = first_digit * 10 + last_digit;
                    combined_numbers.push(combined);
                }
            }
        }
    }

    println!("Answer: {:?}", combined_numbers.iter().sum::<u32>());

    Ok(())
}
```

Both parts of the challenge have been solved, earning two gold stars! 🌟🌟
