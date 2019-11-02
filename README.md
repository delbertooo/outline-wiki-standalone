# Running a standalone outline wiki with docker

This projects aims to simplify the setup of a demo environment of the [outline wiki](https://github.com/outline/outline).

The original project needs
- a external, configured database,
- an external authentication provider (only Slack or Google) and
- an external S3 blob store.



## How to set up

As I couldn't find a quick way to mock Slack or Google Auth, you'll need to use either Slack or Google (or both) to authenticate. From my point of view, using Slack is more straight forward, as Google Auth only works with professional accounts (I don't know why).

1.  Setup [Slack OAuth](https://api.slack.com/apps) or Google Auth and register the corresponding callback URL
    - http://localhost:8000/auth/slack.callback
    - http://localhost:8000/auth/google.callback
1.  Make a copy of `.env.dist` and make modifications (e.g. your slack secrets)
2.  Initialize the database
    - Start the database for the first time
      ```
      docker-compose up -d postgres
      ```
      and wait until the init is finished
      ```
      docker-compose logs -f postgres
      ```
    - Run migrations
      ```
      docker-compose run outline yarn sequelize db:migrate
      ```
3. Bring up the stack, e.g.: `docker-compose up -d`
