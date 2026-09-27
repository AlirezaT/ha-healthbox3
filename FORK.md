# Installing the stability fork

This fork retains the `healthbox3` integration domain and entity identifiers.
It replaces the TrojanHorsePower integration; do not install both copies at
once. The older rmassch integration uses a different domain (`healthbox`), so
its entities and automations are not automatically migrated by this fork.

## Manual replacement of an existing healthbox3 installation

1. Back up Home Assistant, including `config/custom_components/healthbox3`.
2. Download the `0.3.4-beta.2` release archive from this repository.
3. Replace `config/custom_components/healthbox3` with that folder from the
   archive. Do not delete your Healthbox entry in Devices & services.
4. Restart Home Assistant and check that the integration reports
   `0.3.4-beta.2`. Existing configuration, entities, and history are retained.
5. Avoid an upstream HACS update while testing: it would overwrite this fork.

For a fresh HACS installation, add `https://github.com/AlirezaT/ha-healthbox3`
as an Integration custom repository and enable pre-release versions for
this repository. Select `0.3.4-beta.2`. HACS may already track the upstream
repository for the same domain; use the manual replacement above for an
initial trial rather than installing duplicate integrations.

## API key

If the device already reports its key as valid, leave the optional key field
blank. Activation belongs to the device, and the integration detects existing
advanced access automatically. Reconfigure preserves a stored key when the
field is blank. If entering a new key, the integration uploads it once and
checks status up to six times, with two-second delays between checks.
A connection failure is now reported as a connection failure, not a bad key.

## Checking stability

Confirm that sensor and boost entities recover after a brief interruption.
If unavailable periods continue, capture the integration's debug log, and
record whether the device uses Wi-Fi or Ethernet and whether another
integration or script polls it. Request serialization and a single read retry
cannot repair a network outage or an unresponsive device. A complete poll is
limited to 60 seconds; unfinished requests are cancelled before the next poll.

The regression tests simulate timeouts, connection errors, delayed key
activation, plain-text acknowledgements, cancelled requests, and failure
classification in all three configuration flows. They also verify writes
are not replayed. These tests do not establish long-term device reliability.

## Rollback

Restore the backed-up `custom_components/healthbox3` folder and restart Home
Assistant, or redownload upstream 0.3.3 through HACS. This fork does not change
the configuration schema or entity identifiers.
