# 5. State

{% hint style="info" %}
This "Learning Frontity" guide is intended to be read in order so please start from the [first section](settings.md) if you haven't done so already.
{% endhint %}

The next thing we should look at is the `state`.

We have defined it previously as _**"**A javascript object containing all the state exposed by your package"_. For example:

{% code-tabs %}
{% code-tabs-item title="/packages/my-awesome-theme/src/index.js" %}
```javascript
export default {
  state: {
    theme: {
      menu: [
        ["Home", "/"],
        ["About", "/about"]
      ],
      featuredImage: {
        showOnList: true,
        showOnPost: false
      },
      isMenuOpen: false,
    }
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

As you can see here, this theme needs some settings like the `menu` or settings to define if it should show featured images or not, and then some state that is useful while the app is running, like `isMenuOpen`. 

You can access the state in the client console with:

```text
> frontity.state
```

State is a proxy, so you can see the original object clicking on `[[Target]]` :

![Expand \[\[Target\]\] to see the real object behind the proxy.](../.gitbook/assets/screen-shot-2019-07-03-at-10.56.34.png)

### Why not separate settings and state?

First, here at Frontity we think the less concepts the better. Second, imagine a `notifications` package wants to add an item to the `menu` only when the browser actually supports notifications. That's super easy to do by just using the `state`:

{% code-tabs %}
{% code-tabs-item title="/packages/my-notifications-package/src/index.js" %}
```javascript
export default {
  actions: {
    notifications: {
      init: ({ state }) => {
        // Only run this in the browser:
        if (state.frontity.platform === "client") { 
          // Only add item to the menu if browser support notifications:
          if ("Notification" in window) {
            state.theme.menu.push(["Notifications", "/notification-settings"]);
          }
        }
      }
    }
  } 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

As you can see, packages can access the state exposed by other packages.

Finally, what if you decide that the app should be run with the menu open by default? Then you'd only have to set `isMenuOpen` to `true` in your `frontity.settings.js` file. Yes, I know, that makes no sense, but I hope it gives you a sense of how flexible this pattern is.

Another good example of `state` is `tiny-router`. It exposes three props:

{% code-tabs %}
{% code-tabs-item title="/packages/tiny-router/src/index.js" %}
```javascript
export default {
  state: {
    router: {
      link: "/",
      autoFetch: true,
    }
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Here `link` represents the current url of your app and they change when you use the action `actions.router.set("/other-url")`in your theme.

If we were to create an analytics package, we could use `state.router.link` when sending pageviews:

{% code-tabs %}
{% code-tabs-item title="/packages/my-analytics-package/src/index.js" %}
```javascript
export default {
  actions: {
    analytics: {
      sendPageView: ({ state }) => {
        ga('send', {
          hitType: 'pageview',
          page: state.router.link
        });    
      }
    }
  } 
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Finally, `tiny-router` exposes a third prop called `autoFetch`. This is a setting and by default is `true`. If it's active, it fetches the data you need each time you navigate to a new route using: `actions.router.set(link)`.

Here the most common scenario is that you will use your `frontity.settings.js` file to set `autoFetch`  to `false` when you want to control the fetching yourself:

{% code-tabs %}
{% code-tabs-item title="frontity.settings.js" %}
```javascript
export default {
  packages: [
    ...
    {
      name: "@frontity/tiny-router",
      state: {
        router: {
          autoFetch: false
        }
      }
    }
  ]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

These are most important things you need to know about the **Frontity** state:

### 1. State should be serializable

Only objects, arrays and primitives \(strings, numbers...\) are allowed in the `state` because it must be serializable. No circular dependencies are allowed either. The best way to think about it is: **it's a JSON**.

Actually, it is converted to a JSON when it's sent to the client. We'll talk later about how Server Side Rendering works, but it is something like this:

![](../.gitbook/assets/screen-shot-2019-06-03-at-12.37.12.png)

First, this is what Frontity does in the server:

1. It gets the settings of the current site from `frontity.settings.js`.
2. It merges the state exposed by each package with the state from `frontity.settings.js`. 
3. It gives each package the opportunity to populate `state` with an async `beforeSSR` action. SSR stands for Server Side Rendering. This is usually used to fetch content from the WP REST API.
4. It renders React using that initial state.
5. It sends both the HTML generated by React and the initial state to the client.

The client browser paints the HTML received from the server. Then, this is what Frontity does once the JavaScript is run:

1. It loads the `state` in the client using the initial state received from the server. This guarantees that when we render React again we will be in very same place where we left on the server.
2. It renders React again. It should produce the very same HTML we've sent from the server.
3. It gives each package the opportunity to run code with a `afterCSR` action. CSR stands for Client Side Rendering.

### 2. All the state is merged together

As we've seen in the previous point, all the state from either `frontity.settings.js` and your packages is merged together. 

Let's imagine we have this setting file:

{% code-tabs %}
{% code-tabs-item title="frontity.settings.js" %}
```javascript
export default {
  state: {
    frontity: {
      url: "https://my-site.com",
    }
  },
  packages: [
    "@frontity/wp-source",
    {
      name: "@frontity/my-awesome-theme",
      state: {
        theme: {
          featuredImage: {
            showOnList: true,
          }
        }
      }
    },
    {
      name: "@frontity/tiny-router",
      state: {
        router: {
          autoFetch: false
        }
      }
    }
  ]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

First, `my-awesome-theme`, `tiny-router` and `wp-source` state get merged:

```javascript
state: {
  theme: {
    isMenuOpen: false,
    featuredImage: {
      showOnList: false,
      showOnPost: false
    }
  },
  router: {
    link: "/",
    autoFetch: true,
  },
  source: {
    data: {},
    post: {},
    ... // source contains more objects for categories, tags, pages...
  }
}
```

Then, the state from `frontity.settings.js` file gets merged:

```javascript
state: {
  frontity: {
    url: "https://my-site.com", // <- this was added in frontity.settings.js
  },
  theme: {
    isMenuOpen: false,
    featuredImage: {
      showOnList: true, // <- this was modified in frontity.settings.js
      showOnPost: false
    }
  },
  router: {
    link: "/",
    autoFetch: false, // <- this was modified in frontity.settings.js
  },
  source: {
    data: {},
    post: {},
    ...
  }
}
```

Then Frontity executes `beforeSSR` to give the opportunity to each package to modify the state. For example, the theme could use it to fetch content from the REST API:

{% code-tabs %}
{% code-tabs-item title="/packages/my-awesome-theme/src/index.js" %}
```javascript
actions: {
    theme: {
      beforeSSR: async ({ state, actions }) => {
        await actions.source.fetch(state.router.link);
      }
    }
  }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

This populates `source` with some data. For example, if the url is `/my-post`:

```javascript
state: {
  ...,
  source: {
    data: {
      "/my-post": {
        type: "post",
        id: 123,
        isPost: true
      }  
    },
    post: {
      123: {
        id: 60,
        date: "2016-11-25T18:31:11",
        title: "..."
        content: "..."
        ...
      }
    },
    ...
  }
}
```

Now everything is ready for the React render in the server!

### 3. State should be minimal

There are two reasons for this:

1. The initial state is sent to the client, so the smaller the better.
2. It's easier to cause out-of-sync bugs when the state exists in two different places.

For that reason, Frontity supports **derived state**. 

Remember I told you that `state` must be serializable and cannot contain functions? Well, that's still technically true, but you can include **derived state** functions. Let's see them with an example:

```javascript
state: {
  share: {
    data: {
      "/my-first-post": {
        "facebook": 15,
        "twitter": 12,
      },
      "/my-second-post": {
        "facebook": 25,
        "twitter": 32,
      }
    },
    totalCount: 84
    ...
  }
}
```

Here we have a `totalCount` field that represents the sum of all the shares we have in our posts. It looks great, but what happens if we update the shares of our second post?

```javascript
state: {
  share: {
    data: {
      "/my-first-post": {
        "facebook": 15,
        "twitter": 12,
      },
      "/my-second-post": {
        "facebook": 43,
        "twitter": 64,
      }
    },
    totalCount: 84 // <- now totalCount is out of sync
    ...
  }
}
```

Wouldn't it be much easier if `totalCount` could be calculated reactively each time their dependencies change? That's precisely what **derived state** is for:

```javascript
state: {
  share: {
    data: {
      "/my-first-post": {
        "facebook": 15,
        "twitter": 12,
      },
      "/my-second-post": {
        "facebook": 43,
        "twitter": 64,
      }
    },
    totalCount: ({ state }) => {
      let totalCount = 0;
      const shareData = Object.values(data);
      for (let i = 0; i < shareData.length; i +=1) {
        totalCount += shareData[i].facebook;
        totalCount += shareData[i].twitter;
      }
      return totalCount;
    }
    ...
  }
}
```

That's it, now when you use `state.share.totalCount` in React everything will be always updated without anything else to do on your side.

You can also use them with parameters like this:  

```javascript
state: {
  share: {
    data: {
      "/my-first-post": {
        "facebook": 15,
        "twitter": 12,
      },
      "/my-second-post": {
        "facebook": 43,
        "twitter": 64,
      }
    },
    totalCountByRoute: ({ state }) => route => {
      let totalCount = 0;
      totalCount += data[route].facebook;
      totalCount += data[route].twitter;
      return totalCount;
    }
    ...
  }
}
```

And then consumed like this: `state.share.totalCountByRoute("/my-first-post")`, so you should be able to create **derived state** pretty much for anything.

These **derived state functions** are stripped out from the initial state we send to the client but don't worry, they are reinstantiated later in the client by Frontity to ensure everything is back to normal :\)
