
```mermaid

classDiagram
    direction TB

    class as#colon;Object {
        +id: IRI
        +type
        +name
        +attachment
        +attributedTo
        +audience
        +content
        +context
        +endTime
        +generator
        +icon
        +image
        +inReplyTo
        +location
        +preview
        +published
        +replies
        +startTime
        +summary
        +tag
        +updated
        +url
        +to
        +bto
        +cc
        +bcc
        +mediaType
        +duration
        
    }

    class Activity {
        +actor
        +object
        +target
        +result
        +origin
        +instrument
    }

    class IntransitiveActivity {
        +actor
        +target
        +result
        +origin
        +instrument
    }

    class Collection {
        +totalItems
        +current
        +first
        +last
        +items
    }

    class OrderedCollection {
        +orderedItems
    }

    class CollectionPage {
        +partOf
        +next
        +prev
    }

    class OrderedCollectionPage

    class Link {
        +href
        +rel
        +mediaType
        +name
        +hreflang
    }

    class Actor {
        <<conceptual>>
        +inbox
        +outbox
        +following
        +followers
        +liked
        +preferredUsername
    }

    class Person
    class Application
    class Group
    class Organization
    class Service

    class Note
    class Article
    class Document
    class Image
    class Video
    class Event
    class Place
    class Profile
    class Relationship
    class Tombstone

    Object <|-- Activity
    Object <|-- IntransitiveActivity
    Object <|-- Collection
    Collection <|-- OrderedCollection
    Collection <|-- CollectionPage
    OrderedCollection <|-- OrderedCollectionPage

    Object <|-- Actor
    Actor <|-- Person
    Actor <|-- Application
    Actor <|-- Group
    Actor <|-- Organization
    Actor <|-- Service

    Object <|-- Note
    Object <|-- Article
    Object <|-- Document
    Document <|-- Image
    Document <|-- Video
    Object <|-- Event
    Object <|-- Place
    Object <|-- Profile
    Object <|-- Relationship
    Object <|-- Tombstone

    Activity --> Object : object
    Activity --> Actor : actor
    Object --> Actor : attributedTo
    Object --> Collection : replies

```
