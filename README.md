# Election_Analysis

## Project Overview
Seth and Tom had hired me to support their election campaign data analysis for the state of Colorado. We needed to prepare the election results and data to the state so they could perform an audit to announce the official winner. The stat were looking for the following information with in analysis: 

1. Total votes across the counties.

2. The total votes broke out be county and the % each county had in relation to the total votes within all counties.

3. Which county had the most voters participate.

4. Candidate breakout showing how many votes each candidate received and what their total votes percentage aligned to the total votes captured.

5. The winner based on all votes from the different counties showing the winners name, total votes they received and the percentage of votes to the total votes for all candidates.

![Election Results](https://user-images.githubusercontent.com/101777677/162597580-e453bddc-4846-4a0c-a7d0-bbe61e9e6425.JPG)

## Resources
[election_results.csv](https://github.com/WillS78/Election_Analysis/files/8458043/election_results.csv)
[election_analysis.txt](https://github.com/WillS78/Election_Analysis/files/8458045/election_analysis.txt)
- Visual Studio
- Git Bash
- Python 3.7.6
- Google
- Python for dummies book
- Class instructions and pratice codes

## Summary
As we review the data from the election we can conclude the following:

We received a total of 369,711 votes across all counties.

The three counties were:

- Jefferson

- Denver – Largest county with 82.8% for the total votes collected

- Arapahoe

The three candidates and their results were:

 - Charles Casper Stockham - He received 23% of the total votes or 85,213 in total.

  - Diana DeGette - She received 73.8% of the total votes or 272,892 in total.

  - Raymon Anthony Doane - He received 3.1% of the total votes or 11,606 in total.

The official winner was Diana DeGette winning 73.8% of the total votes collected


## Challenge Overview

Dear election commission thanks for allowing us to build a program that collects several data points to help quickly analyze the data and calculate the results. The benefit of using our program is it’s not a one time use. You’ll be able to use it for future elections by making small adjustments to the code. One example of how you can modify the code is to adjust the row it pulls data from based on the specific file you’re utilizing. Another example to modify the code is to add additional list or add/remove what data is being tracked by adjusting your list and what information your trackers will pull. You can also make adjustments in the print sections to change up how the data gets formatted.

## Challenge Summary

In summary this is my first time working in Python. I still have a lot to learn but did really like how the code flows. It took a little to get use to but by the end of week 1 I’m starting to fell better about it. I plan to continue to go back through the challenges, the codes to keep growing my understanding of how everything links together. 

Final Code:

# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge Solution."""

# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.
file_to_load = os.path.join("Resources", "election_results.csv")
# Add a variable to save the file to a path.
file_to_save = os.path.join("analysis", "election_analysis.txt")

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_options = []
county_votes = {}

# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
largest_turnout_county = ""


# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    headers = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes += 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_options:

            # 4b: Add the existing county to the list of counties.
            county_options.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1


# Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    for county in county_votes:
        # 6b: Retrieve the county vote count.
        votes = county_votes[county]
        # 6c: Calculate the percentage of votes for the county.
        vote_percentage = float(votes) / float(total_votes) * 100
        county_results = (
            f"{county}: {vote_percentage:.1f}% ({votes:,})\n")

         # 6d: Print the county results to the terminal.
        print(county_results)
         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)
         # 6f: Write an if statement to determine the winning county and get its vote count.
        if (votes > winning_count) or (county == ""):
            winning_count = votes
            largest_turnout_county = county

    # 7: Print the county with the largest turnout to the terminal.
    winning_county_summary = (
        f"-------------------------\n"
        f"Largest County Turnout: {largest_turnout_county}\n"
        f"-------------------------\n")
    print(winning_county_summary)

    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(winning_county_summary)

    winning_count = 0

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
