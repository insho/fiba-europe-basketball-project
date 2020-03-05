# Fiba Basketball Machine Learning Project
Data analysis and machine learning with fiba europe basketball play-by-play files


## The Project:

To better understand both predictive modeling using machine learning, as well as some basics about building react web apps, build predictive models using publicly available play by play data for Fiba Europe basketball games, and display some info/results about those findings here, in a react web app. Besides a variety of fun statistics, the end result should be algorythms that predict outcomes of a given game based on play by play information from the same game (at any given minute during the match). 

The process can be divided into four parts: 

1. **[Acquiring the Data](fiba_part1_acquiring_data.ipynb)**
2. **[Processing the Data](fiba_part2_process_data.ipynb)**
3. **[Finding Additional Metadata](fiba_part3_finding_additional_metadata.ipynb)**
4. **[Creating, Testing and Comparing Machine Learning Algorithms](fiba_part4_making_algs.ipynb)**


**Extra SQL DEMO:** 

I've also included a 'sql demo' notebook showcasing some sql-based analysis and (somewhat) complicated querying of the fiba europe files. 

It's not particularly easy to get sql syntax to render properly in an ipython notebook on github. I've found the best option is to launch the notebook via binder [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/insho/fiba-europe-basketball-project/master?filepath=fiba_europe_sql_demo.ipynb). 

Here's the best way to do it:
* [Launch the notebook in Binder](https://mybinder.org/v2/gh/insho/fiba-europe-basketball-project/master?filepath=fiba_europe_sql_demo.ipynb)
* Click on the cell with the javasript snippet, that looks like so:
    ```javascript
    require(['notebook/js/codecell'], function(codecell) {
    codecell.CodeCell.options_default.highlight_modes['magic_text/x-mssql'] = {'reg':[/^%%sql/]} ;
    Jupyter.notebook.events.one('kernel_ready.Kernel', function(){
    Jupyter.notebook.get_cells().map(function(cell){
        if (cell.cell_type == 'code'){ cell.auto_highlight(); } }) ;
    ```
and press CTR+ENTER (or CMD+ENTER) to run the cell
* Then, if you click on any of the subsequent cells which contain sql code, the syntax should transform into the proper sql highlighting, etc.


Other, Less Fun Options:
* [nbviewer works, but won't highlight syntax](https://nbviewer.jupyter.org/github/insho/fiba-europe-basketball-project/blob/master/fiba_europe_sql_demo.ipynb) 
* [Github maaay work, but sometimes fails to load](fiba_europe_sql_demo.ipynb)  


## Web App

In addition to this, I have created a web app to both visualize and better explain my Fiba basketball machine learning project. Besides a nice companion to the machine learning project, it was an excellent opportunity to gain some experience building and deploying a react web app for the first time.

**[The app is hosted here](http://fiba-app-dot-bold-vortex-250420.appspot.com/welcome)**


## Process Overview

1. **[Acquiring the Data](fiba_part1_acquiring_data.ipynb)**
Fiba basketball matches could, until late 2019, be found in two places:
* "www.fibalivestats.com" - fiba basketball matches from around the world, available in json format
* "http://live.fibaeurope.com" - fiba basketball matches from european leagues, available in xml format

This project focuses on the latter "live.fibaeurope.com" data. The notebook has functions for downloading and storing these files. 

Unfortunately, as of late 2019, the site no longer seems to be up, and I am not sure how one can find the xml files. I have included a few in the repo for example purposes. 

2. **[Processing the Data](fiba_part2_process_data.ipynb)**

The raw xml files are "processed" into a format conducive to data analysis and machine learning. The result is a single dataframe with a columns for each action/event of interest. A few examples:
* team_fouls_committed_awayteam
* avg_time_between_scoring_events_overall_hometeam
* current_lead_hometeam
* cumulative_lead_changes_game
* cumulative_possessions_overall_hometeam
* starting_five_in_play_hometeam
* top_five_scorers_in_play_hometeam
* points_scored_by_players_in_play_awayteam
* percent_of_total_points_scored_by_players_in_play_awayteam
* points_scored_period1_combined
* current_score_combined
* cumulative_possessions_overall_combined
* starting_five_in_play_combined
* top_five_scorers_in_play_combined
* percent_of_total_stat_count_by_players_in_play_combined
* cumulative_player_personal_fouls_hometeam
* players_with_one_foul_hometeam

3. **[Finding Additional Metadata](fiba_part3_finding_additional_metadata.ipynb)**

While the raw match files contain some metadata, like competition name, location of the match, team names, etc, there are **NO DATES** associated with these matches.

So, when did the matches occur, and in what sequence? This is not strictly necessary to create algorithms that rely on in-game information to predict outcomes, but I found it an annoyance.

Of more immediate importance, there is also **no clean metadata regarding the age group and sex of the players**. Matches include all age groups, from U14 to professional, for both male and female athletes.

To remedy this I scrape a site with archival fiba tournament schedule and boxscore information, and pair that metadata with my fiba matches (based on team names, final scores, and some other logic and ranking). This last bit is done in sql.

4. **[Creating, Testing and Comparing Machine Learning Algorithms](fiba_part4_making_algs.ipynb)**

I use sklearn GradientBoostingRegressor and GradientBoostingClassifier to build some predictive algorithms based on the compiled and processed play-by-play match files. 

As an example, I walkthrough the process of predicting the winner (hometeam or awayteam) based on play-by-play information available in the 1st period with 3 minutes of play remaining. I make three algs, with varying sets of input features. The algs are tested on a test dataset, and the results recorded and compared. The best-performing of these (which also happens to have the most inputs), correctly predicts the winner roughly 67% of the time.

Example training and test sets are included in the repo.

