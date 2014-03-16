# Clojure-mail

```
[clojure-mail "0.1.5"]
```

A clojure library for parsing, downloading and reading
email from Gmail servers.

Possible uses for this library include machine learning corpus generation and
command line mail clients.

## Quickstart

This is a complete example showing how to read the subject of your latest Gmail inbox message

```clojure
(ns myproject.core
  (:require [clojure-mail.core :refer :all]))

(def store (gen-store "user@gmail.com" "password"))

(def inbox-messages (inbox store))

;; to convert a javamail message into a clojure message we need to call read-message

(def latest (read-message (first inbox-messages)))

;; Let's read the subject of our lastest inbox message
(:subject latest))

(keys latest)
;; => (:subject :from :date-recieved :to :multipart? :content-type :sender :date-sent :body)

```

## Use

We need to require clojure-mail.core before we begin.

```clojure
(:require [clojure-mail.core :refer :all])
```

The first thing we need is a mail store which acts as a gateway to our gmail account.
To create store we only need a gmail username and password

```clojure
(def store (gen-store "user@gmail.com" "mypassword"))
```

Now we can fetch email messages from Gmail easily.

```clojure
(def my-inbox-messages (inbox store))
```

## Reading email messages

```clojure

(def javamail-message (first inbox-messages))

(def message (read-message javamail-message))

```
An email message is returned as a Clojure map that looks something like this (with body removed)

```clojure

{:subject "Re: Presents for Dale's baby",
 :from "Someone <someone@aol.com>",
 :date-recieved "Tue Mar 11 12:54:41 GMT 2014",
 :to ("owain@owainlewis.com"),
 :multipart? true,
 :content-type "multipart/ALTERNATIVE",
 :sender "Someone <someone@aol.com>",
 :date-sent "Tue Mar 11 12:54:36 GMT 2014"
 :body [{:content-type "text/plain" :body "..."}
        {:content-type "text/html"  :body "..."}]}

```

## Parser

HTML emails are evil. There is a simple HTML -> Plain text parser provided if you need to
do any machine learning type processing on email messages.

```clojure
(require '[clojure-mail.parser :refer :all])

(html->text "<h1>I HATE HTML EMAILS</h1>")

;; => "I HATE HTML EMAILS"

```

## Reading emails from disk

Clojure mail can be used to parse existing email messages from file. Take a look in test/fixtures to see some example messages. To read one of these messages we can do something like this


```clojure

(def message (read-mail-from-file "test/clojure_mail/fixtures/25"))

(read-message message)

;; =>
;; {:subject "Request to share ContractsBuilder",
;; :from nil, :date-recieved nil,
;; :to "zaphrauk@gmail.com",
;; :multipart? true,
;; :content-type "multipart/alternative; boundary=90e6ba1efefc44ffe804a5e76c56",
;; :sender nil,
;; :date-sent "Fri Jun 17 13:21:19 BST 2011" ..............

```

## License

Copyright © 2014 Owain Lewis

Distributed under the Eclipse Public License, the same as Clojure.
