# Cloud Computing<br/><br/>
## Google Cloud & Docker/Kubernetes<br/>

Improve "to-do list" application's design by using web service. To be more specific, leave the application front-end and overall structure unchanged, but provide the data service through a typical web service (API) instead of supporting directly by a database as in the solution posted for the previous homework.
<br/>
<img src="https://github.com/CathyXueqingZhang/Computer-Science/blob/main/Screen%20Shot%202022-06-28%20at%2020.21.49.png" width="1050" height="500" /><br/>
Using Docker container technology and kubernetes, deploy the 'to-do list' app to a cluster on Google Cloud Platform. You need to describe how you create the Docker image, e.g. using a Dockerfile, and how you create the cluster and deploy the containers to it, e.g. using a script (bash script on Linux/Mac, or batch file on Windows) that includes all the relevant commands for this procedure.
Note: To avoid too much complexity, only deploy the web app part using kubernetes, and run API service on a single VM. Use the external IP of API in the app's code.
