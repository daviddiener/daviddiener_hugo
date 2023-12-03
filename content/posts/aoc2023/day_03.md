---
title: AOC 2023 - Day 3 Gear Ratios
date: 2023-12-03
tags:
  - rust
  - aoc_2023_puzzle
hidden: true
---

<div style="text-align: center;">
    <a href="/posts/aoc2023/challenge">Overview Advent Of Code 2023</a>
</div>

{{< button
text="GitHub Repo: AOC2023" 
link="https://github.com/daviddiener/AdventOfCode2023" 
>}}

## Task Description
You can look up the tasks for day three on the [AOC 2023 website](https://adventofcode.com/2023/day/3). My puzzle input for day three is included in my [AOC 2023 GitHub Repository](https://github.com/daviddiener/AdventOfCode2023/blob/master/src/inputs/day_03.txt).

## Part One: Summing Part Numbers
The code provided for this part involves iterating through the engine schematic, identifying symbols and adjacent numbers. It utilizes a nested loop to traverse the schematic, identifying symbols and then moving in all directions around each symbol to collect adjacent numbers. These adjacent numbers, meeting the defined criteria, are collected and summed up, providing the total sum of part numbers in the engine schematic. 


The coordinates of each number that was found in this process is stored in a checked_coordinates array, so that we can check later if we already added this number, in case there is another adjacent symbol present. My first approach involved just replacing checked values with a dot symbol, but rust borrowing system made this quite difficult, because the schematic_as_array variable is borrowed while iterating, thus not allowing to modify the array by index.

The final answer obtained for part one of the puzzle is *498559*.

```rust
pub fn star_one() -> Result<(), std::io::Error> {
    let file = File::open("src/inputs/day_03.txt")?;
    let reader = BufReader::new(file);

    let schematic_as_array: Vec<Vec<char>> = reader.lines().map(|line| line.unwrap().chars().collect()).collect();
    let mut checked_coordinates: Vec<(usize, usize)> = Vec::new();
    let mut numbers: Vec<Vec<char>> = Vec::new();

    for (i, line) in schematic_as_array.iter().enumerate() {
        for (j, ch) in line.iter().enumerate() {
            if !ch.is_digit(10) && ch != &'.' {
                println!("Symbol {} at position ({}, {}) is not a digit or a dot", ch, i, j);

                // Make a roundabout around the symbol
                for k in -1..=1 {
                    for m in -1..=1 {

                        let c_row = (i as isize + k) as usize;
                        let c_col = (j as isize + m) as usize;

                        if !checked_coordinates.contains(&(c_row, c_col)) && schematic_as_array[c_row][c_col].is_digit(10)  {
                            println!("Symbol {} at position ({}, {}) is a digit", schematic_as_array[c_row][c_col], c_row, c_col);
                            
                            let mut col_offset = 1;
                            let mut c_number = Vec::new();
                            c_number.push(schematic_as_array[c_row][c_col]);

                            // go left and save digits, till there is  another symbol
                            while c_col >= col_offset && schematic_as_array[c_row][c_col - col_offset].is_digit(10) {
                                c_number.insert(0, schematic_as_array[c_row][c_col - col_offset]);
                                checked_coordinates.push((c_row, c_col - col_offset));
                                col_offset += 1;
                            }

                            // go right and save digits, till there is  another symbol
                            col_offset = 1;
                            while schematic_as_array.len() > c_col + col_offset && schematic_as_array[c_row][c_col + col_offset].is_digit(10) {
                                c_number.push(schematic_as_array[c_row][c_col + col_offset]);
                                checked_coordinates.push((c_row, c_col + col_offset));
                                col_offset += 1;
                            }

                            println!("The complete number is {:?}", c_number);
                            numbers.push(c_number);
                        }
                    }
                }

            }
        }

    }

    // combine the chars in schematic_as_array to an int and then sum all up
    let mut sum = 0;
    for line in numbers {
        let line_combined: String = line.into_iter().collect();
        sum += line_combined.parse::<i32>().unwrap();
    }

    println!("Answer: {}", sum);
   
    Ok(())
}
```

## Part Two: Gear ~~Ratios~~ Products
In the second part of the code, the task is to find faulty gears within the engine schematic. A gear is identified by a '*' symbol that is precisely adjacent to two part numbers. 

For this to work, I modified my solution from part one and introduced two staging arrays, holding the numbers and the already checked coordinates for each iteration over a * symbol. After collecting all adjacent numbers, we check if the array has a length of two and only then append the product of the numbers to the main number array and the staging coordinates to the main coordinates array.

The final answer obtained for part two of the puzzle is *72246648*.

```rust
pub fn star_two() -> Result<(), std::io::Error> {
    let file = File::open("src/inputs/day_03.txt")?;
    let reader = BufReader::new(file);

    let schematic_as_array: Vec<Vec<char>> = reader.lines().map(|line| line.unwrap().chars().collect()).collect();
    let mut checked_coordinates: Vec<(usize, usize)> = Vec::new();
    let mut numbers: Vec<i32> = Vec::new();

    for (i, line) in schematic_as_array.iter().enumerate() {
        for (j, ch) in line.iter().enumerate() {
            if !ch.is_digit(10) && ch != &'.' {
                println!("Symbol {} at position ({}, {}) is not a digit or a dot", ch, i, j);

                let mut staging_checked_coordinates: Vec<(usize, usize)> = Vec::new();
                let mut staging_numbers: Vec<Vec<char>> = Vec::new();

                // Make a roundabout around the symbol
                for k in -1..=1 {
                    for m in -1..=1 {

                        let c_row = (i as isize + k) as usize;
                        let c_col = (j as isize + m) as usize;

                        if !staging_checked_coordinates.contains(&(c_row, c_col)) && !checked_coordinates.contains(&(c_row, c_col)) && schematic_as_array[c_row][c_col].is_digit(10)  {
                            println!("Symbol {} at position ({}, {}) is a digit", schematic_as_array[c_row][c_col], c_row, c_col);
                            
                            let mut col_offset = 1;
                            let mut c_number = Vec::new();
                            c_number.push(schematic_as_array[c_row][c_col]);

                            // go left and save digits, till there is  another symbol
                            while c_col >= col_offset && schematic_as_array[c_row][c_col - col_offset].is_digit(10) {
                                c_number.insert(0, schematic_as_array[c_row][c_col - col_offset]);
                                staging_checked_coordinates.push((c_row, c_col - col_offset));
                                col_offset += 1;
                            }

                            // go right and save digits, till there is  another symbol
                            col_offset = 1;
                            while schematic_as_array.len() > c_col + col_offset && schematic_as_array[c_row][c_col + col_offset].is_digit(10) {
                                c_number.push(schematic_as_array[c_row][c_col + col_offset]);
                                staging_checked_coordinates.push((c_row, c_col + col_offset));
                                col_offset += 1;
                            }

                            println!("The complete number is {:?}", c_number);
                            staging_numbers.push(c_number);
                        }
                    }
                }

                if staging_numbers.len() == 2 {
                    let first_value: String = staging_numbers[0].clone().into_iter().collect();
                    let second_value: String = staging_numbers[1].clone().into_iter().collect();
                    println!("For this * there are two numbers ({}, {}) to multiply", first_value, second_value);
                    numbers.push(first_value.parse::<i32>().unwrap() * second_value.parse::<i32>().unwrap());
                    checked_coordinates.append(&mut staging_checked_coordinates);
                }

            }
        }

    }    

    let sum: i32 = numbers.iter().sum(); 
    println!("Answer: {}", sum);
   
    Ok(())
}
```

Both parts of the challenge have been solved, earning two gold stars! 🌟🌟
