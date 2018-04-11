# pal-tracker
Platform  Acceleration Lab - Sessions

<blockquote>
cf target -s review
cf create-service ${MYSQL_SERVICE_NAME} ${PLAN_NAME} tracker-database
cf target -s production
cf create-service ${MYSQL_SERVICE_NAME} ${PLAN_NAME} tracker-database

cf target -s review
cf bind-service pal-tracker tracker-database
cf target -s production
cf bind-service pal-tracker tracker-database

flyway -url="jdbc:mysql://localhost:3306/tracker_dev" -locations=filesystem:databases/tracker clean migrate

flyway -url="jdbc:mysql://localhost:3306/tracker_test" -locations=filesystem:databases/tracker clean migrate

fly --target pal-concourse login --concourse-url ${CONCOURSE_URL} --team-name ${TEAM_NAME}

cf create-space review
cf create-space production

fly -t pal-concourse set-pipeline -p pal-tracker --load-vars-from ci/variables.yml -c ci/pipeline.yml

fly -t pal-concourse destroy-pipeline -p pal-tracker

fly -t pal-concourse unpause-pipeline -p pal-tracker

<blockquote>
