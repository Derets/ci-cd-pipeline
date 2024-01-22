# Deploy on Docker host server using Jenkins

# _Jenkins Job name:_ `Deploy_on_Docker_Host`

### Pre-requisites

1. Jenkins server
1. Docker-host Server

### Integration between Docker-host and Jenkins

Install "publish Over SSH"

- `Manage Jenkins` > `Manage Plugins` > `Available` > `Publish over SSH`

Enable connection between Docker-host and Jenkins

- `Manage Jenkins` > `Configure System` > `Publish Over SSH` > `SSH Servers`

  - SSH Servers: - Name: `docker-host`
    - Hostname:`<ServerIP>`
    - username: `dockeradmin`
      - `Advanced` > chose `Use password authentication, or use a different key`
    - password: `*******`

### Steps to create "Deploy_on_Docker_Host" Jenkin job

#### From Jenkins home page select "New Item"

- Enter an item name: `Deploy_on_Docker_Host`
  - Copy from: `Deploy_on_Tomcat_Server`
- _Source Code Management:_
  - Repository: `https://github.com/derets/hello-world.git`
  - Branches to build : `*/master`
- _Poll SCM_ : - `* * * *`

- _Build:_

  - Root POM:`pom.xml`
  - Goals and options: `clean install package`

- _Post-build Actions_
  - Send build artifacts over SSH
    - _SSH Publishers_
    - SSH Server Name: `docker-host`
    - `Transfers` > `Transfer set`
      - Source files: `webapp/target/*.war`
      - Remove prefix: `webapp/target`
      - Remote directory: `//home//ansadmin` or `.`

Save and run the job now.
