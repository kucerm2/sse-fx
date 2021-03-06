# sse-fx

`sse-fx` provides a set of [re-frame](https://github.com/Day8/re-frame) [fx](https://github.com/Day8/re-frame/blob/master/docs/Effects.md) and [cofx](https://github.com/Day8/re-frame/blob/master/docs/Coeffects.md) handlers for working with [EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource) objects and [Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events).

## Usage

```clojure
(ns re-frame-app.events
  (:require [re-frame.core :as re-frame]
            [com.yetanalytics.sse-fx.events :refer [register-all!]]
            [com.yetanalytics.sse-fx.event-source :as event-source]
            ))

;; Register all fx/cofx
(register-all!)

;; Just the event source handlers
;; (event-source/register!)

;; Use one in a handler
(re-frame/reg-event-fx
 ::init-event-source
 (fn [{:keys [db] :as ctx} _]
   {::event-source/init
    {:uri "/query"
     ;; handle-* args are re-frame event handlers
     :handle-open [::sse-open] ;; re-frame app handler to dispatch
     ;; messages are transit-decoded by default
     ;; supply your own with :decode-msg-fn
     :handle-message [::sse-message]
     ;; Errors can be actual EventSource errors, or handling/dispatch errors.
     ;; either way, they're wrapped in an ex-info
     :handle-error [::sse-error]
     ;; This isn't an event, but rather fires when you close the EventSource with the ::event-source/close fx handler
     :handle-close [::sse-close]}}))

```
## License

Copyright © 2018 Yet Analytics Inc.

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
