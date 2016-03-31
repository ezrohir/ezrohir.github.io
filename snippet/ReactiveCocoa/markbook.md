
```
// RACObserve(self, username) creates a new RACSignal that sends the current
// value of self.username, then the new value whenever it changes.
[RACObserve(self, username) subscribeNext:^(NSString *newName) {
    NSLog(@"%@", newName);
}];
```

```
// RAC() is a macro that makes the binding look nicer
// +combineLatest:reduce: takes an array of signals, executes the block with the
// latest value from each signal whenever any of them changes, and returns a new
// RACSignal that sends the return value of that block as values.
RAC(self, createEnabled) = [RACSignal
    combineLatest:@[ RACObserve(self, password), RACObserve(self, passwordConfirmation) ]
    reduce:^(NSString *password, NSString *passwordConfirm) {
        return @([passwordConfirm isEqualToString:password]);
    }];
```

```
// RACCommand creates signals to represent UI actions.
self.button.rac_command = [[RACCommand alloc] initWithSignalBlock:^(id _) {
    NSLog(@"button was pressed!");
    return [RACSignal empty];
}];
```

### Patterns
There are three basic patterns to using ReactiveCocoa: chaining, splitting, and combining. I covered the first two last time, but let’s go back for an in-depth look.
