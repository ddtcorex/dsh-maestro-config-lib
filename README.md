# @ddtcorex/dsh-maestro-config-lib

Embedded settings-store library shared by every `dsh-maestro-*` plugin: ONE atomic
namespaced JSON store at `~/.dsh/maestro/settings.json` (chmod 600). No Cordis
surface — install it as a plain dependency so any subset of plugins works standalone.

## API

```ts
defineDomain(name, validator)          // owner registers a schema (validated on write)
load({ dshHome? })                     // memoized read; runs one-shot legacy migration
get(domain, { dshHome? })              // domain value or undefined
set(domain, patch, { dshHome? })       // exclusive-lock write, deep-merge patch, 600 mode
onChange(cb)                           // fires after writes made through this instance
definedDomains()                       // schema-registered names
```

Legacy migration: first load with no modern store transforms
`~/.dsh/dsh-maestro-{harness,remote,review}/config.json` into the namespaced store,
renames consumed files `.bak`, drops runtime state (`lastTunnelRunning`), parks
unknown keys under `_legacy`. Never destructive on corrupt input.
