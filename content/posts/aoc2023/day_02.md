---
title: AOC 2023 - Day 2 Cube Conundrum
date: 2023-12-02
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
You can look up the tasks for day two on the [AOC 2023 website](https://adventofcode.com/2023/day/2). My puzzle input for day two is included in my [AOC 2023 GitHub Repository](https://github.com/daviddiener/AdventOfCode2023/blob/master/src/inputs/day_02.txt).

## Part One: Detecting possible games, by parsing and checking the input file on constant max values.

In this part of the puzzle, the task is to determine all possible game ids, by checking if the shown amount of cubes of each color exceeds the limit of 12 red cubes, 13 green cubes, and 14 blue cubes.

- We read the contents of the input file line by line using BufReader 
- Each line is split first by ':' to seperate the game id from the payload of the line. The game id is then obtained by splitting by whitespace. The payload is split first by ';' and then by ',', giving us each cube info (color, amount) of the line. It doesn't really matter which cube info belongs to which set, because we break the loop on the first exceeded cube amount anyways and consider the whole game not possible.
- If such an event occures, the game is flagged as impossible and not pushed into the possible game ids array at the end of the loop.
- Finally, we calculate the sum of all combined ids and print the result.

The final answer obtained for part one of the puzzle is *3099*.

```rust
pub fn star_one() -> Result<(), std::io::Error> {
    let file = File::open("src/inputs/day_02.txt")?;
    let reader = BufReader::new(file);

    let max_red_cubes = 12;
    let max_green_cubes = 13;
    let max_blue_cubes = 14;

    let mut possible_game_ids: Vec<i32> = Vec::new();

    for line in reader.lines() {
        if let Ok(game) = line {
            
            let info: Vec<&str> = game.split(':').collect();

            let game_part: Vec<&str> = info[0].split_whitespace().collect();
            let game_id = game_part[1].parse::<i32>().unwrap();
            let mut game_possible: bool = true;
            
            let parts: Vec<&str> = info[1].split(';').map(|s| s.trim()).collect();

            for part in parts {
                let cubes: Vec<&str> = part.split(',').map(|s| s.trim()).collect();

                for cube in cubes {
                    let cube_info: Vec<&str> = cube.split_whitespace().collect();

                    if cube_info[1] == "red" && cube_info[0].parse::<i32>().unwrap() > max_red_cubes {
                        println!("Detected impossible cube amount: {:?} with GameId: {}", cube_info, game_id);
                        game_possible = false;
                        break;
                    }

                    if cube_info[1] == "green" && cube_info[0].parse::<i32>().unwrap() > max_green_cubes {
                        println!("Detected impossible cube amount: {:?} with GameId: {}", cube_info, game_id);
                        game_possible = false;
                        break;
                    }

                    if cube_info[1] == "blue" && cube_info[0].parse::<i32>().unwrap() > max_blue_cubes {
                        println!("Detected impossible cube amount: {:?} with GameId: {}", cube_info, game_id);
                        game_possible = false;
                        break;
                    }
                }
            }

            if game_possible {
                possible_game_ids.push(game_id);
            }

        }
    }

    println!("Possible Game Ids: {:?}", possible_game_ids);
    let sum: i32 = possible_game_ids.iter().sum();
    println!("Answer: {}", sum);

    Ok(())
}
```

## Part Two: Detecting the minimum amount of allowed cubes per color.

For the second part of the puzzle, the goal is to determine the fewest number of cubes of each color which would have been possible for the game.

- The parsing of the input file is similar to part one.
- Instead of checking against a constant max value as in part one, we save the first amount of cubes of each color, and then check against this value in the following iterations. If the value is higher, we update the max value, if not we check the next cube info.
- Lastly we multiply the max values for each color of each game and sum up the results.

The final answer obtained for part two of the puzzle is *72970*.

```rust
pub fn star_two() -> Result<(), std::io::Error> {
    let file = File::open("src/inputs/day_02.txt")?;
    let reader = BufReader::new(file);

    let mut power_of_sets: Vec<i32> = Vec::new();

    for line in reader.lines() {
        if let Ok(game) = line {

            let mut max_red_cube_amount = 0;
            let mut max_green_cube_amount = 0;
            let mut max_blue_cube_amount = 0;
            
            let info: Vec<&str> = game.split(':').collect();

            let game_part: Vec<&str> = info[0].split_whitespace().collect();
            let game_id = game_part[1].parse::<i32>().unwrap();
            
            let parts: Vec<&str> = info[1].split(';').map(|s| s.trim()).collect();

            for part in parts {
                let cubes: Vec<&str> = part.split(',').map(|s| s.trim()).collect();

                for cube in cubes {
                    let cube_info: Vec<&str> = cube.split_whitespace().collect();

                    println!("Cube Info: {:?}", cube_info);

                    if cube_info[1] == "red" && cube_info[0].parse::<i32>().unwrap() > max_red_cube_amount {
                        println!("Detected new max red cube amount: {}", cube_info[0]);
                        max_red_cube_amount = cube_info[0].parse::<i32>().unwrap();
                        continue;
                    }

                    if cube_info[1] == "green" && cube_info[0].parse::<i32>().unwrap() > max_green_cube_amount {
                        println!("Detected new max green cube amount: {}", cube_info[0]);
                        max_green_cube_amount = cube_info[0].parse::<i32>().unwrap();
                        continue;
                    }

                    if cube_info[1] == "blue" && cube_info[0].parse::<i32>().unwrap() > max_blue_cube_amount {
                        println!("Detected new max blue cube amount: {}", cube_info[0]);
                        max_blue_cube_amount = cube_info[0].parse::<i32>().unwrap();
                        continue;
                    }
                }
            }

            println!("Game: {}, Max Red: {}, Max Green: {}, Max Blue: {}", game_id, max_red_cube_amount, max_green_cube_amount, max_blue_cube_amount);
            power_of_sets.push(max_red_cube_amount * max_green_cube_amount * max_blue_cube_amount);
        }
    }

    let sum: i32 = power_of_sets.iter().sum();
    println!("Answer: {}", sum);

    Ok(())
}
```

Both parts of the challenge have been solved, earning two gold stars! 🌟🌟
