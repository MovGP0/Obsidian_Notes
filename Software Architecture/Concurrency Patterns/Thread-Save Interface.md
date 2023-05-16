Protects access to a non-thread-save item.

- All public methods use a lock object or mutex for thread synchronization
- Public methods can only access private/protected methods
- private/protected methods are not allowed to lock the lock object
