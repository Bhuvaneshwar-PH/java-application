## Jenkins Pipeline Overview

### Pipeline Configuration

 **Agent and Tools**:
   - Executes on any available agent and uses Maven as a required tool.

 **Parameters**:
   - Provides choices for selecting application versions (`2.1`, `2.2`, `2.3`).
   - Includes a boolean option to toggle test execution.

### Pipeline Stages

 **git-checkout**:
   - Checks out the `main` branch from a specified Git repository.

 **mvn-test**:
   - Conditionally runs Maven tests based on the `ExecuteTest` parameter.

 **mvn-deploy**:
   - Cleans and packages the Maven project using `mvn clean package`.

. **docker-login**:
   - Logs into Docker registry using credentials stored in Jenkins (`usernamePassword`).

 **docker-build**:
   - Builds a Docker image tagged with the selected version (`Versions`).

 **docker-push**:
   - Pushes the built Docker image to a specified registry (`172.17.0.3:5000`).

### Post-Build Actions

 **Email Notification**:
   - Sends an HTML email with job name, build number, and pipeline status (`SUCCESS` or `UNKNOWN`).
   - Includes a styled status banner and a link to the Jenkins console output.


