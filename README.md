# docker-jenkins
More fun with Docker but this time Jenkins has come to play.

## Jenkins Setup
Jenkins will require a one time setup that is not included in the project.  Perhaps that can be added later.

### Unlock jenkins
The first time you spin up the project you will need to unlock Jenkins.  The initail password is written to /var/jenkins_home/secrets/initialAdminPassword.  The following command can be used to retrieve the password using docker:

```bash
docker exec -it ${PWD##*/}_jenkins_1 cat /var/jenkins_home/secrets/initialAdminPassword
```

### Install Plugins
* Self-Organizing Swarm Plug-in Modules

   This will allow the slave nodes to auto-register with the master Jenkins instance.  There is no configuration with this plugin aside from creating a user that the slave nodes can use to connect.
* Role-based Authorization Strategy (optional)

### Add user and roles
1. Manage Jenkins > Manage Users > Create User

   Fill in all the required fields.  Note: the username/password as it will need to match the settings in the docker-compose.yml file.

2. Manage Jenkins > Configure Global Security

   Check "Role-Based Security" and click Save.

3. Manage Jenkins > Manage and Assign Roles

   Click Manage Roles and add a new role to the Global roles section called 'autoslave' (or whatever name you wish). Check the 'Read' option under Overall and everything under Agent.

4. Manage Jenkins > Mange and Assign Roles

   Click Assign Roles and add the user from step 1 to the Global roles and check the checkbox in the autoslave column.

## Adding Slaves
More slaves can be added by simply scalling the number of Docker slave containers.  The following command will create slave containers until there are a total of 5:

```bash
docker-compose scale slave=5
```


