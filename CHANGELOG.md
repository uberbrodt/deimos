# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## UNRELEASED

## 1.8.1-beta1 - 2020-07-22

### Fixes :wrench:
- Retry deleting messages without resending the batch if the
  delete fails (fixes [#34](https://github.com/flipp-oss/deimos/issues/34))
- Delete messages in batches rather than all at once to
  cut down on the chance of a deadlock.

### Features :star:
- Add `last_processed_at` to `kafka_topic_info` to ensure
  that wait metrics are accurate in cases where records
  get created with an old `created_at` time (e.g. for
  long-running transactions).
- Add generator for ActiveRecord models and migrations (fixes [#6](https://github.com/flipp-oss/deimos/issues/6))

## 1.8.0-beta2 - 2020-07-08

### Fixes :wrench:
- Fix crash with batch consumption due to not having ActiveSupport::Concern

### Features :star:
- Add `first_offset` to the metadata sent via the batch

## 1.8.0-beta1 - 2020-07-06
### Features :star:
- Added `ActiveRecordConsumer` batch mode

### Fixes :wrench:
- Lag calculation can be incorrect if no messages are being consumed.
- Fixed bug where printing messages on a MessageSizeTooLarge
  error didn't work.

### Roadmap
- Moved SignalHandler and Executor to the `sigurd` gem.

## 1.7.0-beta1 - 2020-05-12
### Features :star:
- Added the DB Poller feature / process.

## 1.6.4 - 2020-05-11
- Fixed the payload logging fix for errored messages as well.

## 1.6.3 - 2020-05-04
### Fixes :wrench:
- Fixed the payload logging fix.

## 1.6.2 - 2020-05-04
### Fixes :wrench:
- When saving records via `ActiveRecordConsumer`, update `updated_at` to today's time
  even if nothing else was saved.
- When logging payloads and metadata, decode them first.
- Fixes bug in `KafkaSource` that crashes when importing a mix of existing and new records with the `:on_duplicate_key_update` option.

## [1.6.1] - 2020-04-20
### Fixes :wrench:
- Re-consuming a message after crashing would try to re-decode message keys.

# [1.6.0] - 2020-03-05
### Roadmap :car:
- Removed `was_message_sent?` method from `TestHelpers`.

# [1.6.0-beta1] - 2020-02-05
### Roadmap :car:
- Updated dependency for Phobos to 1.9.0-beta3. This ensures compatibility with
  Phobos 2.0.
### Fixes :wrench:
- Fixed RSpec warning when using `test_consume_invalid_message`.

# [1.5.0-beta2] - 2020-01-17
### Roadmap :car:
- Added schema backends, which should simplify Avro encoding and make it
  more flexible for unit tests and local development.
### Features :star:
- Add `:test` producer backend which replaces the existing TestHelpers
  functionality of writing messages to an in-memory hash.

# [1.4.0-beta7] - 2019-12-16
### Fixes :wrench:
- Clone loggers when assigning to multiple levels.

# [1.4.0-beta6] - 2019-12-16
### Features :star:
- Added default for max_bytes_per_partition.

# [1.4.0-beta4] - 2019-11-26
### Features :star:
- Added `define_settings` to define settings without invoking callbacks.

# [1.4.0-beta2] - 2019-11-22
### Fixes :wrench:
- Settings with default_proc were being called immediately
  instead of being lazy-evaluated.

# [1.4.0-beta1] - 2019-11-22
### Roadmap :car:
- Complete revamp of configuration method.

# [1.3.0-beta5] - 2020-01-14
### Features :star:
- Added `db_producer.insert` and `db_producer.process` metrics.

# [1.3.0-beta4] - 2019-12-02
### Fixes :wrench:
- Fixed bug where by running `rake deimos:start` without
  specifying a producer backend would crash.

# [1.3.0-beta3] - 2019-11-26
### Fixes :wrench:
- Fixed bug in TestHelpers where key_decoder was not stubbed out.

# [1.3.0-beta2] - 2019-11-22
### Fixes :wrench:
- Fixed bug where consumers would require a key config in all cases
  even though it's optional if they don't use keys.

# [1.3.0-beta1] - 2019-11-21
### Features :star:
- Added `fetch_record` and `assign_key` methods to ActiveRecordConsumer.

# [1.2.0-beta1] - 2019-09-12
### Features :star:
- Added `fatal_error` to both global config and consumer classes.
- Changed `pending_db_messages_max_wait` metric to send per topic.
- Added config to compact messages in the DB producer.
- Added config to log messages in the DB producer.
- Added config to provide a separate logger to the DB producer.

# [1.1.0-beta2] - 2019-09-11
### Fixes :wrench:
- Fixed bug where ActiveRecordConsumer was not using `unscoped` to update
  via primary key and causing duplicate record errors.

# [1.1.0-beta1] - 2019-09-10
### Features :star:
- Added BatchConsumer.

## [1.0.0] - 2019-09-03
### Roadmap :car:
- Official release of Deimos 1.0!

## [1.0.0-beta26] - 2019-08-29
- Recover from Kafka::MessageSizeTooLarge in the DB producer.
- Shut down sync producers correctly when persistent_connections is true.
- Notify when messages fail to produce in the DB producer.
- Delete messages on failure and rely on notification.

## [1.0.0-beta25] - 2019-08-28
- Fix bug where crashing would cause producers to stay disabled

## [1.0.0-beta24] - 2019-08-26
- Reconnect DB backend if database goes away.
- Sleep only 5 seconds between attempts instead of using exponential backoff.
- Fix for null payload being Avro-encoded.

## [1.0.0-beta23] - 2019-08-22
- Fix bug where nil payloads were not being saved to the DB.
- Fix DB producer rake task looking at THREADS env var instead of THREAD_COUNT.
- Debug messages in the DB producer if debug logs are turned on.
- Changed logger in specs to info.

## [1.0.0-beta22] - 2019-08-09
- Add `pending_db_messages_max_wait` metric for the DB producer.
- Fix mock metrics to allow optional option hashes.

## [1.0.0-beta21] - 2019-08-08
- Handle Phobos `persistent_connections` setting in handling buffer overflows

## [1.0.0-beta20] - 2019-08-07
- Catch buffer overflows when producing via the DB producer and split the
  batch up.

## [1.0.0-beta19] - 2019-08-06
- Fix for DB producer crashing on error in Rails 3.

## [1.0.0-beta18] - 2019-08-02
- Fixed crash when sending metrics in a couple of places.

## [1.0.0-beta17] - 2019-07-31
- Added `rails deimos:db_producer` rake task.
- Fixed the DB producer so it runs inline instead of on a separate thread.
  Calling code should run it on a thread manually if that is the desired
  behavior.

## [1.0.0-beta15] - 2019-07-08
- Initial release.
