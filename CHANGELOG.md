# Changelog

## 0.7.0 (ffs-utils/lyra, Fullflower fork)
* Lock refreshes that error are retried every 2 seconds for as long as the lock is still held. Before, one failed retry cycle (about 15 seconds with DSS locking) gave up the lock while about 15 seconds of it were still left.
* Added `LockHandle.beforeLockLost`. When less than 15 seconds are left on a lock whose refreshes keep failing, these callbacks run while the lock is still owned, before `onLockLost`. Sessions use it to make a final save. When another holder has taken the lock, it is lost immediately with no final save.
* Lock acquisition keeps trying for the lock duration plus 20 seconds, with backoff capped at 5 seconds, so a stale lock left by a crashed server can be taken soon after it expires.
* Added `lockDuration`, `lockRefreshInterval` and `autosaveInterval` to `createStore` and `createPlayerStore`. The defaults are unchanged: 90, 60 and 300 seconds.
* Removed the Studio-only bypass that ignored existing locks.
* Removed an empty `useDSSLocking` branch in `acquireLock`.

## 0.6.0
* Added `updateImmutable`, `updateImmutableAsync`, `txImmutable`, `txImmutableAsync` APIs
  * This 'immutable' flavor of API lets you avoid deep copying, but forces you to handle copy-on-write semantics yourself. Instead of returning `true` to commit a change, you return a new copy of the data containing the desired changes.
* Removed `splitUtf8String` implementation in favor of a simpler implementation
* BREAKING: Removed `disableReferenceProtection` in favor of smarter utilization of frozen tables
* Changed changedCallbacks to reconcile mutable changes into a copy-on-write table, making nested change detection easier
* Fixed t absolute requires

## 0.5.0-rc.0
* Commented (almost) the entire codebase
* Expanded Moonwave generated API docs
* Added .luaurc ([#5](https://github.com/paradoxum-games/lyra/issues/5), thanks [@ffrostfall](https://github.com/ffrostfall)!)
* Change changedCallbacks to be -> () instead of -> () -> () ([#4](https://github.com/paradoxum-games/lyra/issues/4), thanks [@ffrostfall](https://github.com/ffrostfall)!)
* Fixed a race condition with locks and added tests for it
* Fixed Promise absolute require ([#3](https://github.com/paradoxum-games/lyra/issues/3), thanks [@ffrostfall](https://github.com/ffrostfall)!)

## 0.4.1
* Added tests for changedCallbacks
* Simplified orphaned file cleanup implementation
* Added file cleanup integration tests

## 0.4.0
* Added `PlayerStore:peek(userId)`, which returns a player's data without loading it into the store
* Added `disableReferenceProtection` option to `PlayerStore.new()`, which improves performance by omitting a deep copy during updates
* Changed sharding to use JSON encoded buffers for compression
* Fixed a bug where buffers wouldn't be copied for atomic updates

## 0.3.3
* Initial release
