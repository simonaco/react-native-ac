+++
title = "Live Coding"
date = 2020-10-26T19:09:32+01:00
weight = 3
draft = false
+++

Now that we talked quite a bit about the system under the
hood we want to get to write some code. We want to pull in
some data from yelp for a specific location and get the coffee shops
in that area.

Let's create a developer account for the [yelp fusion api](https://www.yelp.com/fusion).
The nice thing about the yelp API is that it has a pretty generous free tier
and the real-time usage measuring is very transparent which saves you of any hidden costs, yes
I'm talking about you google.

We will define a `(lat, long)` tuple to have a central location for fetching our brew fix in the morning,
afternoon...oh well...coffee is a timeless treat


{{< lazy-image image="coffee.gif" lightbox=false />}}

## We have an app started so let's fetch some data and show it in our app.

While writing code, because you can't always rely on data fetching to work as intended we
will sometimes want to use fake data so I would advise to install [faker](https://github.com/marak/Faker.js/)
and use it to generate your fixtures in the apps you are developing.

```bash
npm i faker
```

{{% notice warn %}}
When keeping security in mind all your API keys should not be hosted on your app but rather in the server side app
to prevent any shady business practices such as MITM for sniffing out the keys and overusing or API accounts.
That said our app is educational in nature so we are safe to use API keys directly in the app, but we want to defend
ourselves from accidentally pushing the keys to github.
We will do this by using a `.gitignore` file and `.env` to load any specific variables that are not meant for githistory
{{% /notice %}}

The set up is a little bit more verbose than it is in a web environment

```bash
npm i dotenv
npm i -D babel-plugin-inline-dotenv
```

By default when you create an expo app via `expo init <app-name>` expo will create a file `app.json` we want to move
that into a file called `app.config.js` and add `extra` key.

```js
import 'dotenv/config'

export default {
    ...
    extra: {
        yelpApiKey: process.env.YELP_API_KEY
    }
    ...
}
```

Once we have it wired up we can use the environment variables from our app like so:

```js
...
import Constants from 'expo-constants'

const App = () => {
  return (
    <View>
      <Text>{Constants.manifest.extra.fact}</Text>
      <Text>{Constants.manifest.extra.yelpApiKey}</Text>
    </View>
  )
}

...

export default App
```

We will want to display a list of coffeshops in the specified location, so we can choose or rate it. In earlier versions
of `react-native` lists were rendered at the js layer so there were huge performance drawbacks but nowadays there are
two types of [lists](https://reactnative.dev/docs/using-a-listview/) that are both linked into the native side and are performant enough to behave well in most cases. We
will also add a [component library](https://reactnativeelements.com/) to use in order to have a bit fancier UI.

Now that we configured our environment vars we should create an account for the yelp API. We will use it to fetch coffee
roasters from the location. Nuff said though, let's dive into code.

{{< lazy-image image="matrix_code.gif" lightbox=false />}}

