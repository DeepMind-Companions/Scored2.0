# Data retrieval

## Description

One of the most complex parts of this code is how I retrieved data, and placed it in a form that can be used to train our models. This was my first time using pandas at all. 

## Brief overview of the [Resources Folder](../../Resources)

1. [playerData](../../Resources/playerData)
    - This data is the batting and bowling stats of the players.
    - The data was originally in csv files as present in the [rawdata folder](../../Resources/playerData/rawdata)
    - Later it was converted to .pkl files containing dataframes.

2. [MatchData](../../Resources/MatchData)
    - This folder has three main pickle files:
        1. ground_onehot.pkl --> This pickle file is a dataframe that defines the grounds of each match played in one-hot form.
        2. match_player_stats --> This pickle file was an updated dataframe that had player stats of the players in each match.
        3. player_status.pkl --> This pickle file contained a dataframe with values that defined the status of the player in each ball of a match.

3. [FinalData](../../Resources/FinalData)
    - This folder has the divided train, dev and test set that I used to train my models.
    - Each pickle file has a multi-indexed dataframe that contains ball by ball data that I gathered of each type (around 140 columns).
   
4. [Main Folder](../../Resources)
    - In the main directory, file_index is stored. This basically contains the file name of each match stored in a json file in the [t20s json folder](../../t20s_json).
    - Final data was a later modified data that I used for changes in my data.
    - ground_player_lineup.pkl was a pickle file that contained ground name, winning team, batting and bowling team, and the player lineup of each team.
    - match_data.pkl had bowl by bowl data of the whole match.

## Step 0 (previous work)

Some of the work in the [Resources Folder](../../Resources) has been gathered before from my first semester project (scored 1.0).
I had webscraped websites like the [espncricinfo](https://stats.espncricinfo.com/) to get bowling and batting stats of players.

Along with that, I got json files from [cricsheet](https://cricsheet.org) to datascrape the useful data.

## Step 1 - Getting Innings data

The [Scored 2.0 Data Retrieval Innings](./Scored_2_Data_Retrieval_Innings.ipynb) notebook gathers innings data.
- The notebook has code that iterates over each of the t20s json file in the [t20s_json folder](../../t20s_json).
- The code then gathers each innings data, where it records the ball by ball data and stores it in [match_data.pkl](../../Resources)

## Step 2 - Getting Players data

The [PeopleRetrieval_creation](./PeopleRetrieval_creation.ipynb) notebook gathers player stats.
- This notebook uses the csv files [bowling.csv and batting.csv](../../Resources/playerData/rawdata) to store bowling and batting stats in a dataframe.
- That dataframe is then stored in pickle format as [batting.pkl and bowling.pkl](../../Resources/playerData)

## Step 3 - Getting Match data
The [Scored2_Data_Retrieval_Player_Ground](./Scored2_Data_Retrieval_Player_Ground.ipynb) notebook gathers essential match data.
- The notebook has code that iterates over each of the t20s json file in the [t20s_json folder](../../t20s_json).
- The code then gathers the following match data:
    1. Player lineup
    2. Ground name
    3. Winning Team
    4. Batting and Bowling Team
    
## Step 4 - Player Ball by Ball Data
The [player_ball_by_ball](./player_ball_by_ball.ipynb) finally moves towards gathering all the data to a single table.
- The notebook creates 2 dataframes:
    - Player_status and match_player_status, both stored in [MatchData](../../Resources/MatchData).
    
## Step 5 - Final Data
The [Final Data](./final_data.pkl) notebook finally concatenates all of this different data in a single ball by ball dataframe.
- The notebook combines multiple dataframes into a single dataframe.
- **Note that in combining all of this data, the data goes for 400K+ rows to below 300k rows. This is due to data merging issues**

## Step 6 - Output Data
The [Output Data](./output_data.pkl) notebook creates the winning team in binary form. If the batting team wins, y = 1, else it is 0.


