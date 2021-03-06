## Modified version of calendso

## Requirements
This is the working calendso site with DB already setup:
https://www.dropbox.com/s/mjch6vc5f5qvjvr/calendso.zip?dl=0

I want to skip auth fully and just pass a phone number (as user id) in a URL to login or create user and go straight to the /integration page

example:
https://mystite.com/autologin/9122321121
will create a user with id 9122321121 if id does not exist
and take that user to
https://mystite.com/integrations page

I want to autologin using a phone number string - so it should accept without "at" symbol.
You can login to site with username "vg@vg.com" and password "test", you need to modify it to accept user name strings like "+19122321121"

Also part of requirement is a node app that gets availability and sets bookings:
https://api.docs.calendso.com/docs/calendso/calendso.yaml/paths/~1api~1availability~1:user/get

https://api.docs.calendso.com/docs/calendso/calendso.yaml/paths/~1api~1book~1:user/post

Thanks!

### Built With

* [Next.js](https://nextjs.org/)
* [React](https://reactjs.org/)
* [Tailwind](https://tailwindcss.com/)
* [Prisma](https://prisma.io/)

<!-- GETTING STARTED -->
## Getting Started

To get a local copy up and running, please follow these simple steps.

### Prerequisites

Here is what you need to be able to run Calendso.
* Node.js
* PostgreSQL
* Yarn _(recommended)_

You will also need Google API credentials. You can get this from the [Google API Console](https://console.cloud.google.com/apis/dashboard). More details on this can be found below under the [Obtaining the Google API Credentials section](#Obtaining-the-Google-API-Credentials).

### Development Setup

1. Clone the repo
   ```sh
   git clone https://github.com/calendso/calendso.git
   ```
2. Install packages with yarn
   ```sh
   yarn install
   ```
3. Copy `.env.example` to `.env`
4. Configure environment variables in the .env file. Replace `<user>`, `<pass>`, `<db-host>`, `<db-port>` with their applicable values
   ```
   DATABASE_URL='postgresql://<user>:<pass>@<db-host>:<db-port>'
   GOOGLE_API_CREDENTIALS='secret'
   ```
   <details>
   <summary>If you don't know how to configure the DATABASE_URL, then follow the steps here</summary>
   
   1. Create a free account with [Heroku](https://www.heroku.com/).
  
   2. Create a new app. 
   <img width="306" alt="Google Chrome ??? CleanShotX | 2021-04-20 at 02 01 56" src="https://user-images.githubusercontent.com/16905768/115322780-b3d58c00-a17e-11eb-8a52-b758fb0ea942.png">
   
   3. In your new app, go to `Overview` and next to `Installed add-ons`, click `Configure Add-ons`. We need this to set up our database.
   ![image](https://user-images.githubusercontent.com/16905768/115323232-a53ba480-a17f-11eb-98db-58e2f8c52426.png)

   4. Once you clicked on `Configure Add-ons`, click on `Find more add-ons` and search for `postgres`. One of the options will be `Heroku Postgres` - click on that option. 
   ![image](https://user-images.githubusercontent.com/16905768/115323126-5beb5500-a17f-11eb-8030-7380310807a9.png)

   5. Once the pop-up appears, click `Submit Order Form` - plan name should be `Hobby Dev - Free`. 
   <img width="512" alt="Google Chrome ??? CleanShotX | 2021-04-20 at 02 04 29" src="https://user-images.githubusercontent.com/16905768/115323265-b4baed80-a17f-11eb-99f0-d67f019aa6df.png">
   
   6. Once you completed the above steps, click on your newly created `Heroku Postgres` and go to its `Settings`. 
   ![image](https://user-images.githubusercontent.com/16905768/115323367-e92ea980-a17f-11eb-9ff4-dec95f2ec349.png)
   
   7. In `Settings`, copy your URI to your Calendso .env file and replace the `postgresql://<user>:<pass>@<db-host>:<db-port>` with it. 
   ![image](https://user-images.githubusercontent.com/16905768/115323556-4591c900-a180-11eb-9808-2f55d2aa3995.png)
    ![image](https://user-images.githubusercontent.com/16905768/115323697-7a9e1b80-a180-11eb-9f08-a742b1037f90.png)

   8. To view your DB, once you add new data in Prisma, you can use [Heroku Data Explorer](https://heroku-data-explorer.herokuapp.com/).  
   </details> 

5. Set up the database using the Prisma schema (found in `prisma/schema.prisma`)
   ```sh
   npx prisma db push --preview-feature
   ```
6. Run (in development mode)
   ```sh
   yarn dev
   ```
7. Open [Prisma Studio](https://www.prisma.io/studio) to look at or modify the database content:
   ```
   npx prisma studio
   ```
8. Click on the `User` model to add a new user record.
9.  Fill out the fields (remembering to encrypt your password with [BCrypt](https://bcrypt-generator.com/)) and click `Save 1 Record` to create your first user.
10. Open a browser to [http://localhost:3000](http://localhost:3000) and login with your just created, first user.


## Obtaining the Google API Credentials

1. Open [Google API Console](https://console.cloud.google.com/apis/dashboard). If you don't have a project in your Google Cloud subscription, you'll need to create one before proceeding further. Under Dashboard pane, select Enable APIS and Services.
2. In the search box, type calendar and select the Google Calendar API search result.
3. Enable the selected API.
4. Next, go to the [OAuth consent screen](https://console.cloud.google.com/apis/credentials/consent) from the side pane. Select the app type (Internal or External) and enter the basic app details on the first page.
5. In the second page on Scopes, select Add or Remove Scopes. Search for Calendar.event and select the scope with scope value `.../auth/calendar.events`, `.../auth/calendar.readonly`, `.../auth/calendar` and select Update.
6. In the third page (Test Users), add the Google account(s) you'll using. Make sure the details are correct on the last page of the wizard and your consent screen will be configured.
7. Now select [Credentials](https://console.cloud.google.com/apis/credentials) from the side pane and then select Create Credentials. Select the OAuth Client ID option.
8. Select Web Application as the Application Type.
9. Under Authorized redirect URI's, select Add URI and then add the URI  `<CALENDSO URL>/api/integrations/googlecalendar/callback` replacing CALENDSO URL with the URI at which your application runs.
10. The key will be created and you will be redirected back to the Credentials page. Select the newly generated client ID under OAuth 2.0 Client IDs.
11. Select Download JSON. Copy the contents of this file and paste the entire JSON string in the .env file as the value for GOOGLE_API_CREDENTIALS key.

## Obtaining Microsoft Graph Client ID and Secret
1. Open [Azure App Registration](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) and select New registration
2. Name your application
3. Set **Who can use this application or access this API?** to **Accounts in any organizational directory (Any Azure AD directory - Multitenant)**
4. Set the **Web** redirect URI to `<CALENDSO URL>/api/integrations/office365calendar/callback` replacing CALENDSO URL with the URI at which your application runs.
5. Use **Application (client) ID** as the **MS_GRAPH_CLIENT_ID** attribute value in .env
6. Click **Certificates & secrets** create a new client secret and use the value as the **MS_GRAPH_CLIENT_SECRET** attriubte

