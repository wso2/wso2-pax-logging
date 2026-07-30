## Purpose

Upgrade Log4j2 from 2.25.4 to 2.26.0 to fix CVE-2026-49844.

Related: wso2/product-apim#18097

Upstream never shipped a 2.25.5 release for pax-logging — every maintenance branch
(including `pax-logging-2.2.x`, matching our `2.2.9-wso2vx`) jumped straight to 2.26.0,
so this PR follows the same path.

## Changes

Cherry-picked from `ops4j/org.ops4j.pax.logging` commit
[`eb192ff5`](https://github.com/ops4j/org.ops4j.pax.logging/commit/eb192ff553476c5c0342797fe23523fe600476b6)
(Upgrade to Log4j2 2.26.0, backported to `pax-logging-2.2.x`):

- Bump `version.org.apache.logging.log4j` to `2.26.0`
- Sync vendored `AbstractConfiguration.java`, `JdbcDatabaseManager.java`, `PaxLoggingServiceImpl.java`
- Remove `StatusConfiguration.java` (removed upstream in 2.26.0, unused elsewhere here)

No `osgi.bnd` change needed — log4j's own exported package version hasn't moved past
2.20.2, and upstream's commit doesn't touch it either.

## Verification

- Conflict resolution diffed byte-for-byte against upstream's target commit — identical.
- `mvn clean install -DskipTests` builds successfully.
