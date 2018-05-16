# BaazEye

### Features
* Node down - Node box will blink with RED background - checked via ping
* Client down - Node box will blink with Orange background - checked via IP of connected client in RabbitMQ
* BaazEye middleware down - <span style="color:red">"BaazEye ●"</span> logo text will blink RED, otherwise <span style="color:green">"BaazEye ●"</span> will be GREEN
* Apache web server down - Dashboard will not open

### Components
* Spring Boot app - to schedule health check jobs by parsing timing details stored in DB as Unix cron expression (with second precision, e.g. */10 * * * * * this will schedule the job every 10 seconds), this app is also responsible to serve various database tables as REST endpoints. Jobs are posted to a MQTT subject which client is listening to.
* Client app - which is running on each servers to be monitored. This app is continuously listening to the feed where Spring Boot app is publishing jobs to be executed. Client app is also responsible for checking the command if any harmful command is being executed, in that case it will reject the job. Client will also check for long running jobs like if one job is running more than 30 seconds (configurable), then client app will kill the job and respond as "Execution timeout" to the Spring Boot app. This is to ensure health check jobs are not getting piled up. Continuous 3 "Execution timeout" will disable the client and mark the server status as "GREY". In that case someone needs to manually check the job which is causing the timeout and fix it and then enable the client for that server.
* RabbitMQ MQTT Broker - RabbitMQ is being used as MQTT broker to enable pub/sub feature of this app.
* Database - Oracle 12c is being used as default DB, however it can be easily switched to any other database like PostgreSQL or MySQL

### Main Dashboard

![Main DashBoard](https://github.com/ashimloves/BaazEye/raw/master/baazeye1.PNG)

### Individual alerts

![Individual Alerts](https://github.com/ashimloves/BaazEye/raw/master/baazeye2.PNG)
