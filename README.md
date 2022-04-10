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

import csv
import os

file_to_load = os.path.join("Resources", "election_results.csv")
file_to_save = os.path.join("analysis", "election_analysis.txt")

total_votes = 0

candidate_options = []
candidate_votes = {}

county_options = []
county_votes = {}

winning_candidate = ""
winning_count = 0
winning_percentage = 0

largest_turnout_county = ""

with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    headers = next(reader)

    for row in reader:

        total_votes += 1

        candidate_name = row[2]

        county_name = row[1]

        if candidate_name not in candidate_options:

            candidate_options.append(candidate_name)

            candidate_votes[candidate_name] = 0

        candidate_votes[candidate_name] += 1

        if county_name not in county_options:

            county_options.append(county_name)

            county_votes[county_name] = 0

        county_votes[county_name] += 1

with open(file_to_save, "w") as txt_file:

    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    for county in county_votes:
        votes = county_votes[county]
        vote_percentage = float(votes) / float(total_votes) * 100
        county_results = (
            f"{county}: {vote_percentage:.1f}% ({votes:,})\n")

        print(county_results)
        txt_file.write(county_results)
        if (votes > winning_count) or (county == ""):
            winning_count = votes
            largest_turnout_county = county

    winning_county_summary = (
        f"-------------------------\n"
        f"Largest County Turnout: {largest_turnout_county}\n"
        f"-------------------------\n")
    print(winning_county_summary)

    txt_file.write(winning_county_summary)

    winning_count = 0

    for candidate_name in candidate_votes:

        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        print(candidate_results)
        txt_file.write(candidate_results)

        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    txt_file.write(winning_candidate_summary)
