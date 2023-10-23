---
title: Routing with Ruuter in a Reagent / Re-frame project
slug: routing-with-ruuter-in-a-reagent-re-frame-project
date: 2023-02-27
year: 2023
status: published
---

[Ruuter](https://github.com/askonomm/ruuter), my zero-dependency Clojure(Script) router can be used as a general router, without any HTTP server as well. This is true for both Clojure and ClojureScript, and because the router has no dependencies, also true for [Babashka](https://github.com/babashka/babashka) and [NBB](https://github.com/babashka/nbb), and is exactly what I did in a Reagent / Re-frame project recently, and here’s how I did it.

At the core of it all are your routes, let’s define them as something simple:

```lisp
(def routes
  [{:path "/"
    :response (fn [_]
                [:div "Hello, World"])}
   {:path "/hello/:who"
    :response (fn [{params :params}]
                [:div "Hello, " (:who params)])}])
```

Unlike with a HTTP server such as HTTP-Kit, we don’t need the route to have a `:method`, nor do we need it to return a response map. We can have it return anything we want, which in this case is a Reagent component.

Now let’s create a Re-frame event for setting URI path:

```lisp
(ns events
  (:require
    [re-frame.core :refer [reg-event-fx]]))

(reg-event-fx
  :set-path
  (fn [{db :db} [_ path]]
    (.pushState (.-history js/window) nil "" path)
    {:db (assoc db :path path)}))
```

This allows us to call a `:set-path` event whenever we want to change the current route in-place, and it will also update the URL visible in the browser.

Then let’s create a Re-frame subscription, so we could listen to said path:

```lisp
(ns subs
  (:require
    [re-frame.core :refer [reg-sub]]))

(reg-sub
  :path
  (fn [db _]
    (-> db :path)))
```

And finally let’s put it all to work in our core component:

```lisp
(ns core
  (:require
    [reagent.core :as r]
    [reagent.dom :as rd]
    [re-frame.core :refer [dispatch dispatch-sync subscribe]]
    [ruuter.core :as ruuter]
    [events]
    [subs]))

(def routes
  [{:path "/"
    :response (fn [_]
                [:div "Hello, World"])}
   {:path "/hello/:who"
    :response (fn [{params :params}]
                [:div "Hello, " (:who params)])}])

(defn- app []
  (let [popstate-fn #(dispatch [:set-path (-> js/window .-location .-pathname)])
        path (subscribe [:path])]
    (r/create-class
      {:component-did-mount
       (fn [_]
         (dispatch-sync [:initialise-db])
         (.addEventListener js/window "popstate" popstate-fn))
       :component-will-unmount
       (fn [_]
         (.removeEventListener js/window "popstate" popstate-fn))
       :reagent-render
       (fn []
         (when @path
           (ruuter/route routes {:uri @path})))})))

(defn ^:export init []
  (rd/render [app] (.querySelector js/document "#app")))
```

As you can see, when the Reagent app loads, it adds an event listener for `popstate`, which listens to a URI change by the user. Thus, if the user changes the URL manually, the app will call `:set-path` on its own. Regardless if you call the `:set-path` event yourself manually or whether the `popstate` event promps that call, the end result is the same – it re-renders the `app` component, which then will run Ruuter again, matching against the new path, loading the corresponding component.

So if you now navigate to `/hello/John`, it should render “Hello, John” on the page. Oh and, currently when you visit the page via a link directly, it won’t load the correct component, because the default path isn’t set, so I recommend you set it via your Re-frame db initialisation, like so:

```lisp
(ns events
  (:require
    [re-frame.core :refer [reg-event-fx]]))

(def default-db
  {:path (-> js/window .-location .-pathname)})

(reg-event-fx
  :initialise-db
  (fn [_ _]
    {:db db/default-db}))
```

And that’s how you can use [Ruuter](https://github.com/askonomm/ruuter) to do any type of routing, whether that would be in Clojure side, ClojureScript or even in [Babashka](https://github.com/babashka/babashka) and [NBB](https://github.com/babashka/nbb).