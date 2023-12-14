# ant design hook in reagent

- react 18
- ant design 5.12.2
- reagent 1.2.0

antd.cljs
```clojurescript
(ns antd.antd
  (:refer-clojure :exclude [list empty comment])
  (:require ["antd" :as antd]
            [reagent.core :as r]))

(def button (r/adapt-react-class antd/Button))

(defn message-use-message [] (apply (.-useMessage antd/message) (clj->js nil)))
(defn message-info [& args] (apply (.-info antd/message) (clj->js args)))

(def app (r/adapt-react-class antd/App))
(defn use-app [] (apply (.-useApp antd/App) (clj->js nil)))
```

view.cljs
```
(ns ui.view
  (:require [antd.antd :as ad]))

(defn message []
  (let [[message-api ctx] (ad/message-use-message)
        {:keys [info ]} (js->clj message-api :keywordize-keys true)]
    [:<>
     ctx
     [ad/button {:on-click (fn [_] (info "Hello, clojurescript!"))} "click me"]]))

 [:<>
  [:div "message hooks usage"]
  [:f> message]]
```


view2.clj (susing App)
```
(ns ui.view2
  (:require [antd.antd :as ad]
            [ui.util :as util))
(defn init-antd-api []
  (let [{{message-destroy :destroy
          message-error :error
          message-info :info
          message-loading :loading
          message-open :open
          message-success :success
          message-warning :warning} :message
         {modal-confirm :confirm
          modal-error :error
          modal-info :info
          modal-success :success
          modal-warning :warning} :modal
         {notification-destroy :destroy
          notification-error :error
          notification-info :info
          notification-open :open
          notification-success :success
          notification-warning :warning} :notification} (js->clj (ad/use-app) :keywordize-keys true)]
    (js/console.log message-error)
    (reset! util/message-info message-info)
    [:<>]))

(defn root []
  [:div
   [:f> init-antd-api]
   [ad/button {:on-click (fn [_] (util/message-info "Hello, clojurescript!"))} "click me"]])
```

or you can use the global static methods:
```
(ad/message-info "hi clojurescript.")
```


- https://ant.design/components/app
- https://ant.design/components/message#message-demo-hooks
