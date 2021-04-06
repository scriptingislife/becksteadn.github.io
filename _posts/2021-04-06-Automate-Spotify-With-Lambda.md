---
published: true
layout: post
title: Automate Spotify Playlists with AWS Lambda
---

I'm not one for playlists. My Spotify routine typically involves listening to my Discover Weekly, liking songs, then listening to the Liked Songs playlist. Years of a shared family cellphone plan also traumatized me into saving mobile data. If I'm going to listen to something while I drive, I'll download it. Unfortunately my Liked Songs playlist, like yours, is massive.

I wanted to be able to download the most recently liked songs but not all 1,158. A playlist of just the latest 30 songs would be great. That's what we'll build in this blog post.

## Create a Spotify Application

Authenticating with the Spotify API requires creating an application in the [Developer Dashboard](https://developer.spotify.com/dashboard/). There is only one setting that needs to be changed. Add a Redirect URI with the value `http://localhost:9090`. This lets Spotipy automatically complete the authorization process. Yes, Spotipy **not** Spotify*.* Spotipy is a Python library for the Spotify Web API and will be the core of our script.

You'll see the Client ID and Client Secret on the main page. Remember those. Our code will need them.

## Make a Playlist

We'll need the ID of a playlist to put the songs in. Make an empty playlist with a name of your choice. Mine is creatively titled `Recent Likes`. Spotify makes it very easy to get the ID. In the Share menu select `Copy Spotify URI`.

![Get Playlist ID]({{site.baseurl}}/images/Automate-Spotify-With-Lambda/share.png)

## Run the Code

The code can be found in [this GitHub repo](https://github.com/becksteadn/recent-likes). It authenticates to Spotify, clears the specified playlist, then fills it with the latest liked songs. Some assembly is required.

1. Download the code.

```jsx
git clone https://github.com/becksteadn/recent-likes.git && cd recent-likes
```

2. Install Spotipy.

```jsx
virtualenv venv --python=python3
source venv/bin/activate
pip install spotipy
```

3. Add the environment variables. The file `vars.sh` can export all of the environment variables at once rather than pasting them one line at a time. It can be run with the command `source vars.sh`.

```jsx
export SPOTIPY_CLIENT_ID=''
export SPOTIPY_CLIENT_SECRET=''
export SPOTIPY_REDIRECT_URI='http://localhost:9090'
export SPOTIPY_CLIENT_USERNAME=''
export RECENT_LIKES_PLAYLIST_ID=''
export RECENT_LIKES_PLAYLIST_LEN='30'
```

4. Run the script.

`python update-songs.py`

We could stop here. Run the script locally when you'd like to update the playlist. If you'd like to take it a step further, the next section will create an AWS Lambda function to update it every day.

## Run it in the Cloud

The Serverless Framework is the most convenient way to create and manage serverless applications. Luckily a `serverless.yml` file is already included in the GitHub repo. The content is simple. It defines a single Lambda function which runs the Python code. The `schedule` line sets the function to run every day at 9am UTC using [Scheduled Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html).

![CloudWatch Scheduled Events]({{site.baseurl}}/images/Automate-Spotify-With-Lambda/schedule.png)

A plugin is needed to support Python applications that require additional libraries like Spotipy. We'll be following [this guide](https://www.serverless.com/blog/serverless-python-packaging). In short, install it with `npm install serverless-python-requirements`. Docker is also required to be installed and running.

![Serverless Python Requirements plugin]({{site.baseurl}}/images/Automate-Spotify-With-Lambda/plugin.png)

Run `source vars.sh` to set the environment variables then run `serverless deploy`. You can test the function in the AWS console.

### Security Concerns

The Spotify application ID and secret are stored in plaintext as environment variables. AWS encrypts these at rest, but you may prefer more security. There are several options explained in [this blog post](https://www.serverless.com/blog/aws-secrets-management).

Spotipy writes a `.cache-USERNAME` file which contains the OAuth tokens from Spotify. The file is stored with the function code in an S3 bucket managed by the Serverless Framework. I attempted to store this file separately but it would have required additional resources and potentially extending the Spotipy code.

_Troubleshooting tip: If the playlist is not updating, ensure you've run the code locally and the .cache-USERNAME file is included in the function package. This file is stored at `./.serverless/recent-likes.zip` before uploading._

## Listen and Enjoy

I've realized a few benefits of having a Recent Likes playlist. Shuffle play will not bring up songs from your cringey \<insert genre here\> phase. Also, playing the Playlist Radio will play similar songs you're interested in right now - like an on-demand Discover Weekly.

## Wrap Up

This is a solid base for any kind of automatic updating playlist. For example, Alexa Routines cannot shuffle play playlists. There's no point in having a Good Morning playlist if you wake up to the same song every day like an alarm. The `random.shuffle` function in Python could regularly rearrange the songs in a playlist before the routine starts. The possibilities are endless.

## Resources

[Recent Likes GitHub](https://github.com/becksteadn/recent-likes)

[Spotipy Docs](https://spotipy.readthedocs.io/en/2.17.1/)

[How to Handle your Python packaging in Lambda with Serverless plugins](https://www.serverless.com/blog/serverless-python-packaging)