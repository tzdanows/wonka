# wonka

Run giveaways from the set of attendees checked in for an Eventbrite event

## Local Setup

1. Create a `.env` file with the following keys. You can use [this documentation](https://www.eventbrite.com/platform/docs/organizations) to find your Eventbrite keys.
  ```
  EVENTBRITE_ORGANIZATION_ID=
  EVENTBRITE_API_KEY=
  ```

1. To find your api key, log into your eventbrite account and navigate to account settings.
2. Open the developer links dropdown and click on api keys.
3. From here, your `private token` will be whats required for `EVENTBRITE_API_KEY`.
4. You can also plug this private token into this [endpoint](https://www.eventbriteapi.com/v3/users/me/organizations/) to find your `EVENTBRITE_ORGANIZATION_ID`. 
5. Putting the private token into the `OAUTH token` field will return a json with your organization details. 
#### Sample response
```json
{
    "organizations": [
        {
            "_type": "organization",
            "name": "Atlantis Keyboard Meetup",
            "vertical": "default",
            "parent_id": null,
            "locale": "en_OCEAN",
            "created": "1478-09-15T10:29:05",
            "image_id": null,
            "id": "1234567891011" <-- YOU WANT THIS
        },
    ]
}
```

### Running the app and making users

1. Wonka runs locally using Docker and Docker Compose. Make sure Docker is installed and running.
2. `make dev` will run database setup/migrations, spin up the necessary containers, and the application will be available at http://localhost:5000
3. In docker, under the containers section, click the dropdown on the Wonka container.
4. Under the actions column, click on the 3 dots and select open terminal.
5. Run `rails c` to open a rails console.
6. In the rails console, make a user with: `User.create!(email: "YOUR_EMAIL_HERE", password: "YOUR_PASSWORD_HERE")`

### Additionals Comments
Assuming you have the application running using `make dev` and you've done all the above, you should be able to log in to your user account. If your events are not visible upon syncing or error out, you likely have an incorrectly configured .env file. If your attendees are not syncing, you probably need to check them in to the event. I highly suggest making a test event and checking in sample attendees to test the application prior to running real giveaways.

## Storage for images

In a local environment, uploaded images will be stored on disk. In production, you must configure an AWS S3 Bucket ([guide](https://dev.to/nickmendez/how-to-configure-active-storage-with-amazon-aws-s3-cloud-storage-h)). Set the appropriate AWS environment variables as listed in `config/storage.yml`.

## Accessing a shell

`make console` will bring up a bash prompt inside the web container. From there you can run `rails console` to access a console.

## Running tests

`make test`

## Running linters

Rubocop and Brakeman are run automatically by GitHub Actions. If you want to run them locally, you can run

`make lint`

[![Coverage Status](https://coveralls.io/repos/github/nycmkm/wonka/badge.svg)](https://coveralls.io/github/nycmkm/wonka)
