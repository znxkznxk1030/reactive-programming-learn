# Reactive Programming with Reactor 3

[tech.io](https://tech.io/playgrounds/929/reactive-programming-with-reactor-3)

## 2. Flux instances

### Return an empty Flux

```java
Flux<String> emptyFlux() {
 return Flux.empty();
}
```

### Return a Flux that contains 2 values "foo" and "bar" without using an array or a collection

```java
Flux<String> fooBarFluxFromValues() {
 return Flux.just("foo", "bar");
}
```

### Create a Flux from a List that contains 2 values "foo" and "bar"

```java
Flux<String> fooBarFluxFromList() {
 return Flux.fromIterable(Arrays.asList(new String[] {"foo", "bar"}));
}
```

### Create a Flux that emits an IllegalStateException

```java
Flux<String> errorFlux() {
 return Flux.error(new IllegalStateException());
}
```

### Create a Flux that emits increasing values from 0 to 9 each 100ms

```java
Flux<Long> counter() {
 return Flux.interval(Duration.ofMillis(100)).take(10);
}
```

## 3. Mono instances

### Return an empty Mono

```java
Mono<String> emptyMono() {
 return Mono.empty();
}
```

### Return a Mono that never emits any signal

```java
Mono<String> monoWithNoSignal() {
 return Mono.never();
}
```

### Return a Mono that contains a "foo" value

```java
Mono<String> fooMono() {
 return Mono.just("foo");
}
```

### Create a Mono that emits an IllegalStateException

```java
Mono<String> errorMono() {
 return Mono.error(new IllegalStateException());
}
```

## 4. StepVerifier

### Use StepVerifier to check that the flux parameter emits "foo" and "bar" elements then completes successfully

```java
void expectFooBarComplete(Flux<String> flux) {
  StepVerifier.create(flux)
            .expectNext("foo")
            .expectNext("bar")
            .verifyComplete();
}
```

### Use StepVerifier to check that the flux parameter emits "foo" and "bar" elements then a RuntimeException error

```java
void expectFooBarError(Flux<String> flux) {
  StepVerifier.create(flux)
            .expectNext("foo")
            .expectNext("bar")
            .expectError(RuntimeException.class);
}
```

### Use StepVerifier to check that the flux parameter emits a User with "swhite"username and another one with "jpinkman" then completes successfully

``` java
void expectSkylerJesseComplete(Flux<User> flux) {
        StepVerifier.create(flux)
            .assertNext(user -> {
                assertThat(user.getUsername()).isEqualTo("swhite");
            })
            .assertNext(user -> {
                assertThat(user.getUsername()).isEqualTo("jpinkman");
            })
            .verifyComplete();
}
```

### Expect 10 elements then complete and notice how long the test takes

``` java
void expect10Elements(Flux<Long> flux) {
  StepVerifier.create(flux)
            .expectNextCount(10)
            .verifyComplete();
}
```

### Expect 3600 elements at intervals of 1 second, and verify quicker than 3600s by manipulating virtual time thanks to StepVerifier#withVirtualTime, notice how long the test takes

``` java
void expect3600Elements(Supplier<Flux<Long>> supplier) {
  StepVerifier.withVirtualTime(supplier)
            .thenAwait(Duration.ofSeconds(3600))
            .expectNextCount(3600)
            .verifyComplete();
}
```

## 4. Transform

### Capitalize the user username, firstname and lastname

```java
Mono<User> capitalizeOne(Mono<User> mono) {
 return mono.map(user -> {
  return new User(user.getUsername().toUpperCase(), user.getFirstname().toUpperCase(), user.getLastname().toUpperCase());
 });
}
```

### Capitalize the user username, firstname and lastname

```java
Flux<User> capitalizeMany(Flux<User> flux) {
 return flux.map(user -> {
  return new User(user.getUsername().toUpperCase(), user.getFirstname().toUpperCase(), user.getLastname().toUpperCase());
 });
}
```

### Capitalize the users username, firstName and lastName using #asyncCapitalizeUser

```java
Flux<User> asyncCapitalizeMany(Flux<User> flux) {
 return flux.flatMap(this::asyncCapitalizeUser);
}

Mono<User> asyncCapitalizeUser(User u) {
 return Mono.just(new User(u.getUsername().toUpperCase(), u.getFirstname().toUpperCase(), u.getLastname().toUpperCase()));
}
```
