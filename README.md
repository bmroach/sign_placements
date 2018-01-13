# Optimal Placement of Speed Feedback Signs in the City of Boston
<img src='https://www.boston.gov/sites/default/files/speed-limit-3.jpg' height='200' width='auto'><br>
*source: City of Boston*


## The Problem
#### Introduction and Motivation
Our team set out to determine an optimal placement of Speed Feedback Signs in the City of Boston. To do this, we analyzed where accident hotspots were around the city (further referred to as clusters), and where vulnerable areas are. Despite a plethora of types of potentially vulnerable areas, we decided to focus on schools, hospitals, and open spaces, such as parks. We used these locations collectively as 'triggers', or sites of equal weight in our scoring algorithm. We also clustered accidents to get locations where accidents frequently occur, and used these locations as another trigger of equal weight. We hope to solve this problem by placing signs so that the average distance between each sign and its nearby triggers is minimized, in order to protect vulnerable areas. We also have a constraint that each sign must be at least half a mile apart, so our signs don't clump in one area. This problem was previously hosted as an [open data challenge](https://www.boston.gov/calendar/vision-zero-data-challenge-final-presentations). 
#### Explanation of Process -Placement of Speed Feedback Signs
* Phase 1:<br>
Cluster accidents into accident hot spots via k-means, where the number of means is proportional to the number of input nodes. <br>
* Phase 2: <br>
Filter intersections by proximity to accident clusters. For an intersection to be a candidate placement site, the intersection must be in the lower 50th percentile with regards to distance to closest accident cluster. This ensures that final placements are not skewed by proximity to vulnerable sights alone, but must also be close to where accidents are known to occur. 
* Phase 3:<br>
We consider school, hospital, and open space locations, as well as accident cluster locations, as equally weighted 'triggers', or data points. We then run k-means on this data set, with k = 30 (for example, but variable, imagining 30 of these signs are available). We then find the candidate intersection closest to each of these determined means (output of k-means), and output these 30 intersections as the sites of the speed feedback sign placements.
<br>



#### Technical Details
* Each unique portion of our process is its own extension of the dml library's algorithm class, and intermediate data is stored using MongoDB.


## Datasets in Use
* Motor Vehicle Accidents (Analyze Boston)
* Hospital Locations (Analyze Boston)
* Street Intersections (Boston Open Data - opendata.arcgis.com)
* Open Spaces (Boston Open Data - opendata.arcgis.com)
* Schools (boston.opendatasoft.com)

## Scripts
* *fetch_accidents.py* 
* *fetch_hospitals.py*
* $$ *fetch_nodes.py*
* *fetch_open_space.py*
* *fetch_schools.py*
<br><br>
* $$ *get_accident_clusters.py* - Performs k-means on the input accidents to reduce accidents into accident clusters, which are later used as points of influence as to where feedback signs should be placed.
* $$ *get_signal_placements.py* - Consumes the triggers produced by clean_triggers (below) to determine the optimal placement of speed 
* *get_speed_stats.py*
* *get_avgs.py* - Gets average distances between signs and each trigger
* *get_avg_distance.py* - Finds the optimal number of accident clusters and graphs all the average distances for each trigger
<br><br>
* *clean_triggers.py* - Collects and cleans accident clusters, schools, open spaces, hospitals, candidate intersections for placement for use as points in the k-means clustering done in get_signal_placements.
<br><br>
* *make_graph.py* - Plots the determined locations for speed feedback sign placements.

$$ - denotes a script with variable parameters for experimental outputs
## Usage
* To run the project, run server.py and head to localhost:5000.
<br/><br/>
* For deployment on a remote server, we use [Gunicorn](http://gunicorn.org).
<br/><br/>
* The resource libspacialindex is required to run this set of scripts. On macOS, it can be installed with Homebrew: brew install spatialindex. 

### Python modules in use not typically included in standard Python distributions 
* dml
* flask
* geojson
* geoleaflet
* geoql
* matplotlib
* numpy
* pandas
* protoql
* prov
* scipy
* sklearn 
<br>

### Original source
This project was originally a group project at Boston University (see team members below) as part of a grad-level course [Data Mechanics for Pervasive Systems & Urban Applications](http://datamechanics.org). Currently, the repo is under expansion by [Brian Roach](https://github.com/bmroach).<br/>
Additional information regarding the statistical findings of our experiment can be found in the [original repository](https://github.com/Data-Mechanics/course-2017-fal-proj/tree/master/adsouza_bmroach_mcaloonj_mcsmocha).

Original Team Members:
* Adriana D'Souza .......... adsouza@bu.edu
* Brian Roach ................. bmroach@bu.edu
* Jessica McAloon ......... mcaloonj@bu.edu
* Monica Chiu ................ mcsmocha@bu.edu


