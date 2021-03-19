# Installation of Apache Kafka 
- use Strimzi
- Schema Registry can keep a list of schemas to ensure that data conforms to correct standards
- Zookeeper keeps alist of all teh Kafka topics
- We will need a Rails Kafka client library to interact with Zookeeper and get a list of all the topics
- https://blog.blueapron.io/developing-with-kafka-and-rails-applications-783799e13489
- good tutorial on interacting with Kafka from the rails app
- essentially we should have figured out teh architecture ahead of time and we will create the topics in advance
- Rails would then basically have specialist consumer Classes to listen for updates e.g. Has an ebook been opened by 
the lead, it would then trigger whatever knock on actions are required as per business logic within our app
- The names of the Topics which the app is listening out for 
- So the whole point of pub/sub is that we are making our architecture event driven
- Therefore we will want to define the Events that we are looking out for as they are first class citizens
- https://blog.arkency.com/2016/09/minimal-decoupled-subsystems-of-your-rails-app/
- We will likely then have specialist Service objects who are supposed to respond to a given event e.g.
Update CRM with Email Engagement Activity.
- This Service would be specifically listening out for an event of Email Engagement Activity which may originate from
the Email service or perhaps the Lead Tracker Service.
- This Service will then Create an Event as Defined by its own Schema (this could be in whatever Language that specific
service has been written in). 
- The Event Schema will be language agnostic in either Avro, Protofbuffs or JSON
- It will be sent to the Kafka topic e.g. newEmailEngagementEvent
- perhaps we can define all of the Kafka topics in an initializer file so that they only have to be defined once and can easily be changed
if th topic names need to change.
- the Rails Service UpdateCRM with Email Engagement Activity will have a consumer on it listening out to ENV[KAFKA_NEW_EMAIL_ACITIVTY_TOPIC]
- so if the Email Service decides to teh change the name of the event it creates or topic then we only have to update te Kafka intiailizer in one place.
