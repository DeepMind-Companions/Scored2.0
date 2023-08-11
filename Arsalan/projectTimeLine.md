
  
  
  

# Iteration 1

  

Even though initially I planned on making a dense neural network in numpy, I believe this project will be best implemented using RNNs, either LSTM or even GRU.

  

## Preparing Data

  

In order to start the initial project, I am going to build a basic set of features. The first iteration model can be divided into two main types of features:

  

- Features Fetched Initially by the RNN network.

1. Match details can be ground etc.

2. Possible Lineup?

3. Country Stats Possibly.

- Features Fetched during each LSTM cell by the RNN network.

1. Runs Scored

2. Wickets

3. Current Over/Ball (possibly as we will already be implementing this in a sequence model)

  
  

## Issues to be Solved right now:

  

So I do not know how to handle the remaining players data. How do I tell my neural network, these are the players that are left. Another idea that comes to mind is another neural network connected perpendicularly to this neural network so that the Neural Network realises the player lineup as well.

  

- Issues with this approach:

1. I believe that even if we use this approach, neural network would have no way to connect wickets with this.

  

Another possible Approach IMO, can be to use encoding to decide each players status with the following statuses:

  

- Player Status key:

	| Status | Definition |
	| --- | --- |
	| (0) | Player is out |
	| (1) | Player is in game |
	| (5) | Player is non striker |
	| (10) | Player is baller |
	| (10) | Player is batsman |

  

- Issues with this approach:

- Even though I like this idea more, it has some issues:

1. Feature Engineering is a lot harder to implement here, especially taking track of who is in the lineup right now, who got wicketed and it means more additional inputs as well.

2. It can possibly be harder for the neural network to learn what each feature means.

  
  

## Initial Scraping data and building features

  

The data we currently have is only ball by ball data and we need to write code to get all the additional features:

  

| S.no | Data we need | Why we need it |
| ---- | ------------ | -------------- |
| 1 | Ground Data | Ground Variation on outcome|
| 2 | Player lineup | Tell Neural Network arrangement and stats of each player


However, getting both of these data can be hard. The proposed initial solution for the ground data is using one hot coding initially and later we use featured embedding/encoding.

As for the player lineup, it will also be extracted from the data. We will use further feature engineering to implement the player's status accordingly.






# Starting to work with these details

## Step 1: Getting Data

### Working on getting player data

The following work is done in [The PeopleRetrieval_creation notebook](Notebooks/PeopleRetrieval_creation.ipynb)

I had the old data I had scraped from cricket websites to use in my previous scored project. I am using the same data to get bowling and batting stats. 

Here are the details: 
- I am replacing all the "-" signs with np.NaN in order to make computations easier, although I am not sure if it will help much.
- I added another player named "Average" and used average values to fill in all the Inns, overs, mdns etc. 
  - I did this using pd functions like df.mean() and df.fillna(**Inserted an Series here**)
  - Also note the argument inplace -- It helps replace the current df instead of creating a new one

Updates:
- I had boosted my batting and bowling records further
- This made sure I have minimum missing players for training
- I have about 3500 bowlers and batters details now. Their name are same as the ones from the match data as well
- I now have to develop an algorithm that takes this data and processes it accordingly

The work is finally stored in [The playerData folder](Resources/playerData)

### Getting the player lineup and ground name

The following work is done in [The second Data Retrieval notebook](Notebooks/Scored2_Data_Retrieval_Player_Ground.ipynb)

Another important part in this neural network architecture is to obtain data in such a way that player status is recorded. In order to make this, I am making another ball by ball data for each match.

In order to get this data, I am taking the following steps:
- I am getting the ground name, and player lineup from the json files.

the work is finally stored in [the resources folder](Resources/) as ground_player_lineup.pkl

### Making the Final Data

Finally, we are going to make the final data. This will be the most complex data processing I have done yet IMO. The work is done in [The player_ball_by_ball notebook](Notebooks/player_ball_by_ball.ipynb)

I demonstrated in the notebook what we really need. Here I will draft the pseudocode of how to achieve all that.

| S. no | What we need | How we will get it |
| ----- | ------------ | ------------------ |
| 1. | Stats of current playing and those on bench | We will use the player lineup data and combine that with the player stats to get each player's stats. |
| 2. | Score uptil now | This has already been stored in the innings DF |
| 3. | Total Wickets | Already in Innings DF |
| 4. | Score this bal | Already in Innings DF |
| 5. | Who is playing and who is in bench | From the already made series / DF, we will add additional encoding to record who is playing and who is on bench. We will use the innings DF along with the DF/Series we made in point 1. |
| 6. | One hot encoding of ground | We will be using the data from the match data. |

**New Issue:**
Another thing I have to resolve now is that I will have to deal with both team's lineups.