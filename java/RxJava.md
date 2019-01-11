# Java: RxJava


```java


    public static void main(String[] args) {
        Observable<String> myStrings =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        myStrings.subscribe(s -> System.out.println(s));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> myStrings =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        myStrings.map(s -> s.length()).subscribe(s ->
                System.out.println(s));
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> secondIntervals =
                Observable.interval(1, TimeUnit.SECONDS);
        secondIntervals.subscribe(s -> System.out.println(s));
/* Hold main thread for 5 secondsso Observable above has chance to fire */
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source = Observable.create(emitter -> {
            emitter.onNext("Alpha");
            emitter.onNext("Beta");
            emitter.onNext("Gamma");
            emitter.onNext("Delta");
            emitter.onNext("Epsilon");
            emitter.onComplete();
        });
        source.subscribe(s -> System.out.println("RECEIVED: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        source.map(String::length).filter(i -> i >= 5)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
//first observer
        source.subscribe(s -> System.out.println("Observer 1 Received: " + s));
//second observer
        source.subscribe(s -> System.out.println("Observer 2 Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
//first observer
        source.subscribe(s -> System.out.println("Observer 1 Received: " + s));
//second observer
        source.map(String::length).filter(i -> i >= 5)
                .subscribe(s -> System.out.println("Observer 2 Received: " + s));
    }

```


```java


    @Override
    public void start(Stage stage) throws Exception {
        ToggleButton toggleButton = new ToggleButton("TOGGLE ME");
        Label label = new Label();
        Observable<Boolean> selectedStates =
                valuesOf(toggleButton.selectedProperty());
        selectedStates.map(selected -> selected ? "DOWN" : "UP")
                .subscribe(label::setText);
        VBox vBox = new VBox(toggleButton, label);
        stage.setScene(new Scene(vBox));
        stage.show();
    }
    private static <T> Observable<T> valuesOf(final
                                              ObservableValue<T> fxObservable) {
        return Observable.create(observableEmitter -> {
//emit initial state
            observableEmitter.onNext(fxObservable.getValue());
//emit value changes uses a listener
            final ChangeListener<T> listener = (observableValue, prev,
                                                current) -> observableEmitter.onNext(current);
            fxObservable.addListener(listener);
        });
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
                        .publish();
//Set up observer 1
        source.subscribe(s -> System.out.println("Observer 1: " + s));
//Set up observer 2
        source.map(String::length)
                .subscribe(i -> System.out.println("Observer 2: " + i));
//Fire!
        source.connect();
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 10)
                .subscribe(s -> System.out.println("RECEIVED: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.range(5, 10)
                .subscribe(s -> System.out.println("RECEIVED: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.interval(1, TimeUnit.SECONDS)
                .subscribe(s -> System.out.println(s + " Mississippi"));
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> seconds = Observable.interval(1,
                TimeUnit.SECONDS);
//Observer 1
        seconds.subscribe(l -> System.out.println("Observer 1: " + l));
//sleep 5 seconds
        sleep(5000);
//Observer 2
        seconds.subscribe(l -> System.out.println("Observer 2: " + l));
//sleep 5 seconds
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<Long> seconds =
                Observable.interval(1, TimeUnit.SECONDS).publish();
//observer 1
        seconds.subscribe(l -> System.out.println("Observer 1: " + l));
        seconds.connect();
//sleep 5 seconds
        sleep(5000);
//observer 2
        seconds.subscribe(l -> System.out.println("Observer 2: " + l));
//sleep 5 seconds
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source = Observable.create(emitter -> {
            try {
                emitter.onNext("Alpha");
                emitter.onNext("Beta");
                emitter.onNext("Gamma");
                emitter.onNext("Delta");
                emitter.onNext("Epsilon");
                emitter.onComplete();
            } catch (Throwable e) {
                emitter.onError(e);
            }
        });
        source.subscribe(s -> System.out.println("RECEIVED: " + s),
                Throwable::printStackTrace);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> empty = Observable.empty();
        empty.subscribe(System.out::println,
                Throwable::printStackTrace,
                () -> System.out.println("Done!"));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> empty = Observable.never();
        empty.subscribe(System.out::println,
                Throwable::printStackTrace,
                () -> System.out.println("Done!"));
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.error(new Exception("Crash and burn!"))
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        Throwable::printStackTrace,
                        () -> System.out.println("Done!"));
    }

```


```java


    public static void main(String[] args) {
        Observable.error(() -> new Exception("Crash and burn!"))
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        Throwable::printStackTrace,
                        () -> System.out.println("Done!"));
    }

```


```java


    private static int start = 1;
    private static int count = 5;
    public static void main(String[] args) {
        Observable<Integer> source = Observable.range(start, count);
        source.subscribe(i -> System.out.println("Observer 1: " + i));
//modify count
        count = 10;
        source.subscribe(i -> System.out.println("Observer 2: " + i));
    }

```


```java


    private static int start = 1;
    private static int count = 5;
    public static void main(String[] args) {
        Observable<Integer> source = Observable.defer(() ->
                Observable.range(start, count));
        source.subscribe(i -> System.out.println("Observer 1: " + i));
//modify count
        count = 10;
        source.subscribe(i -> System.out.println("Observer 2: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(1 / 0)
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("Error Captured: " + e));
    }

```


```java


    public static void main(String[] args) {
        Observable.fromCallable(() -> 1 / 0)
                .subscribe(i -> System.out.println("Received: " + i),
                        e -> System.out.println("Error Captured: " + e));
    }

```


```java


    public static void main(String[] args) {
        Single.just("Hello")
                .map(String::length)
                .subscribe(System.out::println,
                        Throwable::printStackTrace);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma");
        source.first("Nil") //returns a Single
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source = Observable.create(emitter -> {
            try {
                emitter.onNext("Alpha");
                emitter.onNext("Beta");
                emitter.onNext("Gamma");
                emitter.onNext("Delta");
                emitter.onNext("Epsilon");
                emitter.onComplete();
            } catch (Throwable e) {
                emitter.onError(e);
            }
        });
        Observable<Integer> lengths = source.map(String::length);
        Observable<Integer> filtered = lengths.filter(i -> i >= 5);
        filtered.subscribe(s -> System.out.println("RECEIVED: " +
                s));
    }

```


```java


    public static void main(String[] args) {
// has emission
        Maybe<Integer> presentSource = Maybe.just(100);
        presentSource.subscribe(s -> System.out.println("Process 1 received:" + s),
                Throwable::printStackTrace,
                () -> System.out.println("Process 1 done!"));
//no emission
        Maybe<Integer> emptySource = Maybe.empty();
        emptySource.subscribe(s -> System.out.println("Process 2 received:" + s),
                Throwable::printStackTrace,
                () -> System.out.println("Process 2 done!"));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
        source.firstElement().subscribe(
                s -> System.out.println("RECEIVED " + s),
                Throwable::printStackTrace,
                () -> System.out.println("Done!"));
    }

```


```java


    public static void main(String[] args) {
        Completable.fromRunnable(() -> runProcess())
                .subscribe(() -> System.out.println("Done!"));
    }
    public static void runProcess() {
//run process here
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> seconds =
                Observable.interval(1, TimeUnit.SECONDS);
        Disposable disposable =
                seconds.subscribe(l -> System.out.println("Received: " + l));
//sleep 5 seconds
        sleep(5000);
//dispose and stop emissions
        disposable.dispose();
//sleep 5 seconds to prove
//there are no more emissions
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> source =
                Observable.interval(1, TimeUnit.SECONDS);
        ResourceObserver<Long> myObserver = new
                ResourceObserver<Long>() {
                    @Override
                    public void onNext(Long value) {
                        System.out.println(value);
                    }
                    @Override
                    public void onError(Throwable e) {
                        e.printStackTrace();
                    }
                    @Override
                    public void onComplete() {
                        System.out.println("Done!");
                    }
                };
//capture Disposable
        Disposable disposable = source.subscribeWith(myObserver);
    }

```


```java


    private static final CompositeDisposable disposables
            = new CompositeDisposable();
    public static void main(String[] args) {
        Observable<Long> seconds =
                Observable.interval(1, TimeUnit.SECONDS);
//subscribe and capture disposables
        Disposable disposable1 =
                seconds.subscribe(l -> System.out.println("Observer 1: " +
                        l));
        Disposable disposable2 =
                seconds.subscribe(l -> System.out.println("Observer 2: " +
                        l));
//put both disposables into CompositeDisposable
        disposables.addAll(disposable1, disposable2);
//sleep 5 seconds
        sleep(5000);
//dispose all disposables
        disposables.dispose();
//sleep 5 seconds to prove
//there are no more emissions
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> source =
                Observable.create(observableEmitter -> {
                    try {
                        for (int i = 0; i < 1000; i++) {
                            while (!observableEmitter.isDisposed()) {
                                observableEmitter.onNext(i);
                            }
                            if (observableEmitter.isDisposed())
                                return;
                        }
                        observableEmitter.onComplete();
                    } catch (Throwable e) {
                        observableEmitter.onError(e);
                    }
                });
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source = Observable.create(emitter -> {
            try {
                emitter.onNext("Alpha");
                emitter.onNext("Beta");
                emitter.onNext("Gamma");
                emitter.onNext("Delta");
                emitter.onNext("Epsilon");
                emitter.onComplete();
            } catch (Throwable e) {
                emitter.onError(e);
            }
        });
        source.map(String::length)
                .filter(i -> i >= 5)
                .subscribe(s -> System.out.println("RECEIVED: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        source.map(String::length).filter(i -> i >= 5)
                .subscribe(s -> System.out.println("RECEIVED: " + s));
    }

```


```java


    public static void main(String[] args) {
        List<String> items =
                Arrays.asList("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
        Observable<String> source = Observable.fromIterable(items);
        source.map(String::length).filter(i -> i >= 5)
                .subscribe(s -> System.out.println("RECEIVED: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        Observer<Integer> myObserver = new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {
//do nothing with Disposable, disregard for now
            }
            @Override
            public void onNext(Integer value) {
                System.out.println("RECEIVED: " + value);
            }
            @Override
            public void onError(Throwable e) {
                e.printStackTrace();
            }
            @Override
            public void onComplete() {
                System.out.println("Done!");
            }
        };
        source.map(String::length).filter(i -> i >= 5)
                .subscribe(myObserver);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        source.map(String::length).filter(i -> i >= 5)
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        Throwable::printStackTrace,
                        () -> System.out.println("Done!"));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        source.map(String::length).filter(i -> i >= 5)
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        Throwable::printStackTrace);
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
                .filter(s -> s.length() != 5)
                .subscribe(s -> System.out.println("RECEIVED: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Zeta", "Eta", "Gamma",
                "Delta")
                .distinctUntilChanged(String::length)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Zeta", "Eta", "Gamma",
                "Delta")
                .elementAt(3)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("M/d/yyyy");
        Observable.just("1/3/2016", "5/9/2016", "10/12/2016")
                .map(s -> LocalDate.parse(s, dtf))
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> menu =
                Observable.just("Coffee", "Tea", "Espresso", "Latte");
//print menu
        menu.startWith("COFFEE SHOP MENU")
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> menu =
                Observable.just("Coffee", "Tea", "Espresso", "Latte");
//print menu
        menu.startWithArray("COFFEE SHOP MENU", "----------------")
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> items =
                Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
        items.filter(s -> s.startsWith("Z"))
                .defaultIfEmpty("None")
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .filter(s -> s.startsWith("Z"))
                .switchIfEmpty(Observable.just("Zeta", "Eta", "Theta"))
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(6, 2, 5, 7, 1, 4, 9, 8, 3)
                .sorted()
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.just(6, 2, 5, 7, 1, 4, 9, 8, 3)
                .sorted(Comparator.reverseOrder())
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .sorted((x, y) -> Integer.compare(x.length(), y.length()))
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
                .take(3)
                .subscribe(s -> System.out.println("RECEIVED: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .delay(3, TimeUnit.SECONDS)
                .subscribe(s -> System.out.println("Received: " + s));
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .repeat(2)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 3, 7, 10, 2, 14)
                .scan((accumulator, next) -> accumulator + next)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .scan(0, (total, next) -> total + 1)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .count()
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 3, 7, 10, 2, 14)
                .reduce((total, next) -> total + next)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 3, 7, 10, 2, 14)
                .reduce("", (total, next) -> total + (total.equals("") ?
                        "" :
                        ",") + next)
                .subscribe(s -> System.out.println("Received: " +
                        s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 3, 7, 11, 2, 14)
                .all(i -> i < 10)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("2016-01-01", "2016-05-02", "2016-09-12",
                "2016-04-03")
                .map(LocalDate::parse)
                .any(dt -> dt.getMonthValue() >= 6)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 10000)
                .contains(9563)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.interval(300, TimeUnit.MILLISECONDS)
                .take(2, TimeUnit.SECONDS)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .toList()
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 1000)
                .toList(1000)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .toList(CopyOnWriteArrayList::new)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(6, 2, 5, 7, 1, 4, 9, 8, 3)
                .toSortedList()
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .toMap(s -> s.charAt(0))
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .toMap(s -> s.charAt(0), String::length)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .toMap(s -> s.charAt(0), String::length,
                        ConcurrentHashMap::new)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .toMap(String::length)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .toMultimap(String::length)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .collect(HashSet::new, HashSet::add)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 100)
                .skip(90)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .collect(ImmutableList::builder,
                        ImmutableList.Builder::add)
                .map(ImmutableList.Builder::build)
                .subscribe(s -> System.out.println("Received: " + s));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> 10 / i)
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> 10 / i)
                .onErrorReturnItem(-1)
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> 10 / i)
                .onErrorReturn(e -> -1)
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> {
                    try {
                        return 10 / i;
                    } catch (ArithmeticException e) {
                        return -1;
                    }
                })
                .subscribe(i -> System.out.println("RECEIVED: " +
                                i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> 10 / i)
                .onErrorResumeNext(Observable.just(-1).repeat(3))
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> 10 / i)
                .onErrorResumeNext(Observable.empty())
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> 10 / i)
                .onErrorResumeNext((Throwable e) ->
                        Observable.just(-1).repeat(3))
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> 10 / i)
                .retry()
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .map(i -> 10 / i)
                .retry(2)
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 100)
                .takeWhile(i -> i < 5)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .doOnNext(s -> System.out.println("Processing: " + s))
                .map(String::length)
                .subscribe(i -> System.out.println("Received: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .doOnComplete(() -> System.out.println("Source is done emitting!"))
                .map(String::length)
                .subscribe(i -> System.out.println("Received: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 2, 4, 0, 3, 2, 8)
                .doOnError(e -> System.out.println("Source failed!"))
                .map(i -> 10 / i)
                .doOnError(e -> System.out.println("Division failed!"))
                .subscribe(i -> System.out.println("RECEIVED: " + i),
                        e -> System.out.println("RECEIVED ERROR: " + e)
                );
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .doOnSubscribe(d -> System.out.println("Subscribing!"))
                .doOnDispose(() -> System.out.println("Disposing!"))
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(5, 3, 7, 10, 2, 14)
                .reduce((total, next) -> total + next)
                .doOnSuccess(i -> System.out.println("Emitting: " + i))
                .subscribe(i -> System.out.println("Received: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 100)
                .skipWhile(i -> i <= 95)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .map(String::length)
                .distinct()
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .distinct(String::length)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable.just(1, 1, 1, 2, 2, 3, 3, 2, 1, 1)
                .distinctUntilChanged()
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        Observable<String> source2 =
                Observable.just("Zeta", "Eta", "Theta");
        Observable.merge(source1, source2)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
//emit every second, but only take 2 emissions
        Observable<String> source1 =
                Observable.interval(1, TimeUnit.SECONDS)
                        .take(2)
                        .map(l -> l + 1) // emit elapsed seconds
                        .map(l -> "Source1: " + l + " seconds");
//emit every 300 milliseconds
        Observable<String> source2 =
                Observable.interval(300, TimeUnit.MILLISECONDS)
                        .map(l -> (l + 1) * 300) // emit elapsed milliseconds
                        .map(l -> "Source2: " + l + " milliseconds");
        Observable.concat(source1, source2)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
//keep application alive for 5 seconds
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        source.concatMap(s -> Observable.fromArray(s.split("")))
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
//emit every second
        Observable<String> source1 =
                Observable.interval(1, TimeUnit.SECONDS)
                        .take(2)
                        .map(l -> l + 1) // emit elapsed seconds
                        .map(l -> "Source1: " + l + " seconds");
//emit every 300 milliseconds
        Observable<String> source2 =
                Observable.interval(300, TimeUnit.MILLISECONDS)
                        .map(l -> (l + 1) * 300) // emit elapsed milliseconds
                        .map(l -> "Source2: " + l + " milliseconds");
//emit Observable that emits first
        Observable.amb(Arrays.asList(source1, source2))
                .subscribe(i -> System.out.println("RECEIVED: " +
                        i));
//keep application alive for 5 seconds
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        Observable<Integer> source2 = Observable.range(1, 6);
        Observable.zip(source1, source2, (s, i) -> s + "-" + i)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> strings =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        Observable<Long> seconds =
                Observable.interval(1, TimeUnit.SECONDS);
        Observable.zip(strings, seconds, (s, l) -> s)
                .subscribe(s ->
                        System.out.println("Received " + s +
                                " at " + LocalTime.now())
                );
        sleep(6000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> source1 =
                Observable.interval(300, TimeUnit.MILLISECONDS);
        Observable<Long> source2 =
                Observable.interval(1, TimeUnit.SECONDS);
        Observable.combineLatest(source1, source2,
                (l1, l2) -> "SOURCE 1: " + l1 + " SOURCE 2: " + l2)
                .subscribe(System.out::println);
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> source1 =
                Observable.interval(300, TimeUnit.MILLISECONDS);
        Observable<Long> source2 =
                Observable.interval(1, TimeUnit.SECONDS);
        source2.withLatestFrom(source1,
                (l1, l2) -> "SOURCE 2: " + l1 + " SOURCE 1: " + l2
        ).subscribe(System.out::println);
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon");
        Observable<GroupedObservable<Integer, String>> byLengths =
                source.groupBy(s -> s.length());
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        Observable<GroupedObservable<Integer, String>> byLengths =
                source.groupBy(s -> s.length());
        byLengths.flatMapSingle(grp -> grp.toList())
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        Observable<GroupedObservable<Integer, String>> byLengths =
                source.groupBy(s -> s.length());
        byLengths.flatMapSingle(grp ->
                grp.reduce("", (x, y) -> x.equals("") ? y : x + ", " + y)
                        .map(s -> grp.getKey() + ": " + s)
        ).subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 =
                Observable.just("Alpha", "Beta");
        Observable<String> source2 =
                Observable.just("Gamma", "Delta");
        Observable<String> source3 =
                Observable.just("Epsilon", "Zeta");
        Observable<String> source4 =
                Observable.just("Eta", "Theta");
        Observable<String> source5 =
                Observable.just("Iota", "Kappa");
        Observable.mergeArray(source1, source2, source3, source4,
                source5)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 =
                Observable.just("Alpha", "Beta");
        Observable<String> source2 =
                Observable.just("Gamma", "Delta");
        Observable<String> source3 =
                Observable.just("Epsilon", "Zeta");
        Observable<String> source4 =
                Observable.just("Eta", "Theta");
        Observable<String> source5 =
                Observable.just("Iota", "Kappa");
        List<Observable<String>> sources =
                Arrays.asList(source1, source2, source3, source4,
                        source5);
        Observable.merge(sources)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
//emit every second
        Observable<String> source1 = Observable.interval(1,
                TimeUnit.SECONDS)
                .map(l -> l + 1) // emit elapsed seconds
                .map(l -> "Source1: " + l + " seconds");
//emit every 300 milliseconds
        Observable<String> source2 =
                Observable.interval(300, TimeUnit.MILLISECONDS)
                        .map(l -> (l + 1) * 300) // emit elapsed milliseconds
                        .map(l -> "Source2: " + l + " milliseconds");
//merge and subscribe
        Observable.merge(source1, source2)
                .subscribe(System.out::println);
//keep alive for 3 seconds
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        source.flatMap(s -> Observable.fromArray(s.split("")))
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("521934/2342/FOXTROT", "21962/12112/78886/TANGO", "283242/4542/WHISKEY/2348562");
        source.flatMap(s -> Observable.fromArray(s.split("/")))
                .filter(s -> s.matches("[0-9]+")) //use regex to filter integers
                .map(Integer::valueOf)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> intervalArguments =
                Observable.just(2, 3, 10, 7);
        intervalArguments.flatMap(i ->
                Observable.interval(i, TimeUnit.SECONDS)
                        .map(i2 -> i + "s interval: " + ((i + 1) * i) + " seconds elapsed")
        ).subscribe(System.out::println);
        sleep(12000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        source.flatMap(s -> Observable.fromArray(s.split("")),
                (s, r) ->
                        s + "-" + r)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon");
        Observable<String> source2 =
                Observable.just("Zeta", "Eta", "Theta");
        Observable.concat(source1, source2)
                .subscribe(i -> System.out.println("RECEIVED: " + i));
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> threeIntegers = Observable.range(1, 3);
        threeIntegers.subscribe(i -> System.out.println("Observer One: " + i));
        threeIntegers.subscribe(i -> System.out.println("Observer Two: " + i));
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<Integer> threeInts =
                Observable.range(1, 3).publish();
        Observable<Integer> threeRandoms = threeInts.map(i ->
                randomInt());
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
        threeRandoms.subscribe(i -> System.out.println("Observer 2: " + i));
        threeInts.connect();
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<Integer> threeRandoms =
                Observable.range(1, 3)
                        .map(i -> randomInt()).publish();
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
        threeRandoms.subscribe(i -> System.out.println("Observer 2: " + i));
        threeRandoms.connect();
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<Integer> threeRandoms =
                Observable.range(1, 3)
                        .map(i -> randomInt()).publish();
//Observer 1 - print each random integer
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
//Observer 2 - sum the random integers, then print
        threeRandoms.reduce(0, (total, next) -> total + next)
                .subscribe(i -> System.out.println("Observer 2: " + i));
        threeRandoms.connect();
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> threeRandoms = Observable.range(1, 3)
                .map(i -> randomInt())
                .publish()
                .autoConnect(2);
        //Observer 1 - print each random integer
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
        //Observer 2 - sum the random integers, then print
                threeRandoms.reduce(0, (total, next) -> total + next)
                        .subscribe(i -> System.out.println("Observer 2: " +
                                i));
        //Observer 3 - receives nothing
        threeRandoms.subscribe(i -> System.out.println("Observer 3:" + i));
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> seconds =
                Observable.interval(1, TimeUnit.SECONDS)
                        .publish()
                        .autoConnect();
//Observer 1
        seconds.subscribe(i -> System.out.println("Observer 1: " +
                i));
        sleep(3000);
//Observer 2
        seconds.subscribe(i -> System.out.println("Observer 2: " +
                i));
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> seconds =
                Observable.interval(1, TimeUnit.SECONDS)
                        .publish()
                        .refCount();
//Observer 1
        seconds.take(5)
                .subscribe(l -> System.out.println("Observer 1: " +
                        l));
        sleep(3000);
//Observer 2
        seconds.take(2)
                .subscribe(l -> System.out.println("Observer 2: " +
                        l));
        sleep(3000);
//there should be no more Observers at this point
//Observer 3
        seconds.subscribe(l -> System.out.println("Observer 3: " +
                l));
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> seconds =
                Observable.interval(1, TimeUnit.SECONDS)
                        .replay()
                        .autoConnect();
//Observer 1
        seconds.subscribe(l -> System.out.println("Observer 1: " +
                l));
        sleep(3000);
//Observer 2
        seconds.subscribe(l -> System.out.println("Observer 2: " +
                l));
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source =
                Observable.just("Alpha", "Beta", "Gamma",
                        "Delta", "Epsilon")
                        .replay(1)
                        .autoConnect();
//Observer 1
        source.subscribe(l -> System.out.println("Observer 1: " +
                l));
//Observer 2
        source.subscribe(l -> System.out.println("Observer 2: " +
                l));
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> seconds =
                Observable.interval(300, TimeUnit.MILLISECONDS)
                        .map(l -> (l + 1) * 300) // map to elapsed milliseconds
                .replay(1, TimeUnit.SECONDS)
                .autoConnect();
//Observer 1
        seconds.subscribe(l -> System.out.println("Observer 1: " +
                l));
        sleep(2000);
//Observer 2
        seconds.subscribe(l -> System.out.println("Observer 2: " +
                l));
        sleep(1000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> cachedRollingTotals =
                Observable.just(6, 2, 5, 7, 1, 4, 9, 8, 3)
                        .scan(0, (total, next) -> total + next)
                        .cache();
        cachedRollingTotals.subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<Integer> threeIntegers =
                Observable.range(1, 3).publish();
        threeIntegers.subscribe(i -> System.out.println("Observer One:" + i));
        threeIntegers.subscribe(i -> System.out.println("Observer Two:" + i));
        threeIntegers.connect();
    }

```


```java


    public static void main(String[] args) {
        Subject<String> subject = PublishSubject.create();
        subject.map(String::length)
                .subscribe(System.out::println);
        subject.onNext("Alpha");
        subject.onNext("Beta");
        subject.onNext("Gamma");
        subject.onComplete();
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 =
                Observable.interval(1, TimeUnit.SECONDS)
                        .map(l -> (l + 1) + " seconds");
        Observable<String> source2 =
                Observable.interval(300, TimeUnit.MILLISECONDS)
                        .map(l -> ((l + 1) * 300) + " milliseconds");
        Subject<String> subject = PublishSubject.create();
        subject.subscribe(System.out::println);
        source1.subscribe(subject);
        source2.subscribe(subject);
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Subject<String> subject = PublishSubject.create();
        subject.onNext("Alpha");
        subject.onNext("Beta");
        subject.onNext("Gamma");
        subject.onComplete();
        subject.map(String::length)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Subject<String> subject =
                BehaviorSubject.create();
        subject.subscribe(s -> System.out.println("Observer 1: " +
                s));
        subject.onNext("Alpha");
        subject.onNext("Beta");
        subject.onNext("Gamma");
        subject.subscribe(s -> System.out.println("Observer 2: " +
                s));
    }

```


```java


    public static void main(String[] args) {
        Subject<String> subject =
                ReplaySubject.create();
        subject.subscribe(s -> System.out.println("Observer 1: " +
                s));
        subject.onNext("Alpha");
        subject.onNext("Beta");
        subject.onNext("Gamma");
        subject.onComplete();
        subject.subscribe(s -> System.out.println("Observer 2: " +
                s));
    }

```


```java


    public static void main(String[] args) {
        Subject<String> subject =
                AsyncSubject.create();
        subject.subscribe(s ->
                        System.out.println("Observer 1: " + s),
                Throwable::printStackTrace,
                () -> System.out.println("Observer 1 done!")
        );
        subject.onNext("Alpha");
        subject.onNext("Beta");
        subject.onNext("Gamma");
        subject.onComplete();
        subject.subscribe(s ->
                        System.out.println("Observer 2: " + s),
                Throwable::printStackTrace,
                () -> System.out.println("Observer 2 done!")
        );
    }

```


```java


    public static void main(String[] args) {
        Subject<String> subject =
                UnicastSubject.create();
        Observable.interval(300, TimeUnit.MILLISECONDS)
                .map(l -> ((l + 1) * 300) + " milliseconds")
                .subscribe(subject);
        sleep(2000);
        subject.subscribe(s -> System.out.println("Observer 1: " +
                s));
        sleep(2000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Subject<String> subject =
                UnicastSubject.create();
        Observable.interval(300, TimeUnit.MILLISECONDS)
                .map(l -> ((l + 1) * 300) + " milliseconds")
                .subscribe(subject);
        sleep(2000);
//multicast to support multiple Observers
        Observable<String> multicast =
                subject.publish().autoConnect();
//bring in first Observer
        multicast.subscribe(s -> System.out.println("Observer 1: "
                + s));
        sleep(2000);
//bring in second Observer
        multicast.subscribe(s -> System.out.println("Observer 2: "
                + s));
        sleep(1000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> threeRandoms = Observable.range(1, 3)
                .map(i -> randomInt());
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
        threeRandoms.subscribe(i -> System.out.println("Observer 2: " + i));
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<Integer> threeInts =
                Observable.range(1, 3).publish();
        Observable<Integer> threeRandoms = threeInts.map(i ->
                randomInt());
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
        threeRandoms.subscribe(i -> System.out.println("Observer 2: " + i));
        threeInts.connect();
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<Integer> threeRandoms =
                Observable.range(1, 3)
                        .map(i -> randomInt()).publish();
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
        threeRandoms.subscribe(i -> System.out.println("Observer 2: " + i));
        threeRandoms.connect();
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        ConnectableObservable<Integer> threeRandoms =
                Observable.range(1, 3)
                        .map(i -> randomInt()).publish();
        //Observer 1 - print each random integer
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
        //Observer 2 - sum the random integers, then print
        threeRandoms.reduce(0, (total, next) -> total + next)
                .subscribe(i -> System.out.println("Observer 2: " + i));
        threeRandoms.connect();
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> threeRandoms = Observable.range(1, 3)
                .map(i -> randomInt())
                .publish()
                .autoConnect(2);
//Observer 1 - print each random integer
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
//Observer 2 - sum the random integers, then print
                threeRandoms.reduce(0, (total, next) -> total + next)
                        .subscribe(i -> System.out.println("Observer 2: " + i));
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> threeRandoms = Observable.range(1, 3)
                .map(i -> randomInt()).publish().autoConnect(2);
        //Observer 1 - print each random integer
        threeRandoms.subscribe(i -> System.out.println("Observer 1: " + i));
        //Observer 2 - sum the random integers, then print
                threeRandoms.reduce(0, (total, next) -> total + next)
                        .subscribe(i -> System.out.println("Observer 2: " +
                                i));
        //Observer 3 - receives nothing
        threeRandoms.subscribe(i -> System.out.println("Observer 3:" + i));
    }
    public static int randomInt() {
        return ThreadLocalRandom.current().nextInt(100000);
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> seconds =
                Observable.interval(1, TimeUnit.SECONDS)
                        .publish()
                        .autoConnect();
//Observer 1
        seconds.subscribe(i -> System.out.println("Observer 1: " + i));
        sleep(3000);
//Observer 2
        seconds.subscribe(i -> System.out.println("Observer 2: " + i));
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.interval(1, TimeUnit.SECONDS)
                .map(i -> i + " Mississippi")
                .subscribe(System.out::println);
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.fromCallable(() ->
                getResponse("https://api.github.com/users/thomasnield/starred")
        ).subscribeOn(Schedulers.io())
                .subscribe(System.out::println);
        sleep(10000);
    }
    private static String getResponse(String path) {
        try {
            return new Scanner(new URL(path).openStream(),
                    "UTF-8").useDelimiter("\\A").next();
        } catch (Exception e) {
            return e.getMessage();
        }
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.interval(1, TimeUnit.SECONDS,
                Schedulers.newThread())
                .subscribe(i -> System.out.println("Received " + i
                        +
                        " on thread " +
                        Thread.currentThread().getName()));
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .subscribeOn(Schedulers.computation())
                .filter(s -> s.length() == 5)
                .subscribeOn(Schedulers.io())
                .subscribe(i -> System.out.println("Received " + i
                        +
                        " on thread " +
                        Thread.currentThread().getName()));
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
//Happens on IO Scheduler
        Observable.just("WHISKEY/27653/TANGO", "6555/BRAVO",
                "232352/5675675/FOXTROT")
                .subscribeOn(Schedulers.io())
                .flatMap(s -> Observable.fromArray(s.split("/")))
//Happens on Computation Scheduler
                .observeOn(Schedulers.computation())
                .filter(s -> s.matches("[0-9]+"))
                .map(Integer::valueOf)
                .reduce((total, next) -> total + next)
                .subscribe(i -> System.out.println("Received " + i
                        + " on thread "
                        + Thread.currentThread().getName()));
        sleep(1000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
//Happens on IO Scheduler
        Observable.just("WHISKEY/27653/TANGO", "6555/BRAVO",
                "232352/5675675/FOXTROT")
                .subscribeOn(Schedulers.io())
                .flatMap(s -> Observable.fromArray(s.split("/")))
                .doOnNext(s -> System.out.println("Split out " + s
                        + " on thread "
                        + Thread.currentThread().getName()))
//Happens on Computation Scheduler
                .observeOn(Schedulers.computation())
                .filter(s -> s.matches("[0-9]+"))
                .map(Integer::valueOf)
                .reduce((total, next) -> total + next)
                .doOnSuccess(i -> System.out.println("Calculated sum" + i + " on thread"
                                + Thread.currentThread().getName()))
//Switch back to IO Scheduler
                .observeOn(Schedulers.io())
                .map(i -> i.toString())
                .doOnSuccess(s -> System.out.println("Writing " + s
                        + " to file on thread "
                        + Thread.currentThread().getName()))
                .subscribe(s ->
                        write(s, "/home/thomas/Desktop/output.txt"));
        sleep(1000);
    }
    public static void write(String text, String path) {
        BufferedWriter writer = null;
        try {
//create a temporary file
            File file = new File(path);
            writer = new BufferedWriter(new FileWriter(file));
            writer.append(text);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                writer.close();
            } catch (Exception e) {
            }
        }
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    @Override
    public void start(Stage stage) throws Exception {
        VBox root = new VBox();
        ListView<String> listView = new ListView<>();
        Button refreshButton = new Button("REFRESH");
        JavaFxObservable.actionEventsOf(refreshButton)
                .observeOn(Schedulers.io())
                .flatMapSingle(a ->
                        Observable.fromArray(getResponse("https://goo.gl/S0xuOi")
                                .split("\\r?\\n")
                        ).toList()
                ).observeOn(JavaFxScheduler.platform())
                .subscribe(list ->
                        listView.getItems().setAll(list));
        root.getChildren().addAll(listView, refreshButton);
        stage.setScene(new Scene(root));
        stage.show();
    }
    private static String getResponse(String path) {
        try {
            return new Scanner(new URL(path).openStream(),
                    "UTF-8").useDelimiter("\\A").next();
        } catch (Exception e) {
            return e.getMessage();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 10)
                .map(i -> intenseCalculation(i))
                .subscribe(i -> System.out.println("Received " + i +
                        " "
                        + LocalTime.now()));
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 10)
                .flatMap(i -> Observable.just(i)
                        .subscribeOn(Schedulers.computation())
                        .map(i2 -> intenseCalculation(i2))
                )
                .subscribe(i -> System.out.println("Received " + i +
                        " "
                        + LocalTime.now() + " on thread "
                        + Thread.currentThread().getName()));
        sleep(20000);
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        int coreCount = Runtime.getRuntime().availableProcessors();
        AtomicInteger assigner = new AtomicInteger(0);
        Observable.range(1, 10)
                .groupBy(i -> assigner.incrementAndGet() %
                        coreCount)
                .flatMap(grp -> grp.observeOn(Schedulers.io())
                        .map(i2 -> intenseCalculation(i2))
                )
                .subscribe(i -> System.out.println("Received " + i +
                        " "
                        + LocalTime.now() + " on thread "
                        + Thread.currentThread().getName()));
        sleep(20000);
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Disposable d = Observable.interval(1, TimeUnit.SECONDS)
                .doOnDispose(() -> System.out.println("Disposing on thread"
                                + Thread.currentThread().getName()))
                .subscribe(i -> System.out.println("Received " +
                        i));
        sleep(3000);
        d.dispose();
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .map(s -> intenseCalculation((s)))
                .subscribe(System.out::println);
        Observable.range(1, 6)
                .map(s -> intenseCalculation((s)))
                .subscribe(System.out::println);
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Disposable d = Observable.interval(1, TimeUnit.SECONDS)
                .doOnDispose(() -> System.out.println("Disposing on thread"
                                + Thread.currentThread().getName()))
                .unsubscribeOn(Schedulers.io())
                .subscribe(i -> System.out.println("Received " +
                        i));
        sleep(3000);
        d.dispose();
        sleep(3000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .subscribeOn(Schedulers.computation())
                .map(s -> intenseCalculation((s)))
                .subscribe(System.out::println);
        Observable.range(1, 6)
                .subscribeOn(Schedulers.computation())
                .map(s -> intenseCalculation((s)))
                .subscribe(System.out::println);
        sleep(20000);
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon")
                        .subscribeOn(Schedulers.computation())
                        .map(s -> intenseCalculation((s)));
        Observable<Integer> source2 =
                Observable.range(1, 6)
                        .subscribeOn(Schedulers.computation())
                        .map(s -> intenseCalculation((s)));
        Observable.zip(source1, source2, (s, i) -> s + "-" + i)
                .subscribe(System.out::println);
        sleep(20000);
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.interval(1, TimeUnit.SECONDS)
                .map(l -> intenseCalculation((l)))
                .subscribe(System.out::println);
        sleep(Long.MAX_VALUE);
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .subscribeOn(Schedulers.computation())
                .map(Ch6_6::intenseCalculation)
                .blockingSubscribe(System.out::println,
                        Throwable::printStackTrace,
                        () -> System.out.println("Done!"));
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        int numberOfThreads = 20;
        ExecutorService executor =
                Executors.newFixedThreadPool(numberOfThreads);
        Scheduler scheduler = Schedulers.from(executor);
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .subscribeOn(scheduler)
                .doFinally(executor::shutdown)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> lengths =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon")
                        .subscribeOn(Schedulers.computation())
                        .map(Ch6_8::intenseCalculation)
                        .map(String::length);
        lengths.subscribe(i ->
                System.out.println("Received " + i + " on thread " +
                        Thread.currentThread().getName()));
        lengths.subscribe(i ->
                System.out.println("Received " + i + " on thread " +
                        Thread.currentThread().getName()));
        sleep(10000);
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> lengths =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon")
                        .subscribeOn(Schedulers.computation())
                        .map(Ch6_9::intenseCalculation)
                        .map(String::length)
                        .publish()
                        .autoConnect(2);
        lengths.subscribe(i ->
                System.out.println("Received " + i + " on thread " +
                        Thread.currentThread().getName()));
        lengths.subscribe(i ->
                System.out.println("Received " + i + " on thread " +
                        Thread.currentThread().getName()));
        sleep(10000);
    }
    public static <T> T intenseCalculation(T value) {
        sleep(ThreadLocalRandom.current().nextInt(3000));
        return value;
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 50)
                .buffer(8)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.interval(300, TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 300) // map to elapsed time
                .window(1, TimeUnit.SECONDS)
                .flatMapSingle(obs -> obs.reduce("", (total,
                                                      next) -> total
                        + (total.equals("") ? "" : "|") + next))
                .subscribe(System.out::println);
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> cutOffs =
                Observable.interval(1, TimeUnit.SECONDS);
        Observable.interval(300, TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 300) // map to elapsed time
                .window(cutOffs)
                .flatMapSingle(obs -> obs.reduce("", (total, next) ->
                        total
                                + (total.equals("") ? "" : "|") + next))
                .subscribe(System.out::println);
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 = Observable.interval(100,
                TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 100) // map to elapsed time
                .map(i -> "SOURCE 1: " + i)
                .take(10);
        Observable<String> source2 = Observable.interval(300,
                TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 300) // map to elapsed time
                .map(i -> "SOURCE 2: " + i)
                .take(3);
        Observable<String> source3 = Observable.interval(2000,
                TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 2000) // map to elapsed time
                .map(i -> "SOURCE 3: " + i)
                .take(2);
        Observable.concat(source1, source2, source3)
                .subscribe(System.out::println);
        sleep(6000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> source1 = Observable.interval(100,
                TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 100) // map to elapsed time
                .map(i -> "SOURCE 1: " + i)
                .take(10);
        Observable<String> source2 = Observable.interval(300,
                TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 300) // map to elapsed time
                .map(i -> "SOURCE 2: " + i)
                .take(3);
        Observable<String> source3 = Observable.interval(2000,
                TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 2000) // map to elapsed time
                .map(i -> "SOURCE 3: " + i)
                .take(2);
        Observable.concat(source1, source2, source3)
                .throttleWithTimeout(1, TimeUnit.SECONDS)
                .subscribe(System.out::println);
        sleep(6000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> items = Observable.just("Alpha", "Beta",
                "Gamma", "Delta", "Epsilon",
                "Zeta", "Eta", "Theta", "Iota");
//delay each String to emulate an intense calculation
        Observable<String> processStrings = items.concatMap(s ->
                Observable.just(s)
                        .delay(randomSleepTime(),
                                TimeUnit.MILLISECONDS)
        );
        processStrings.subscribe(System.out::println);
//keep application alive for 20 seconds
        sleep(20000);
    }
    public static int randomSleepTime() {
//returns random sleep time between 0 to 2000 milliseconds
        return ThreadLocalRandom.current().nextInt(2000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<String> items = Observable.just("Alpha", "Beta",
                "Gamma", "Delta", "Epsilon",
                "Zeta", "Eta", "Theta", "Iota");
//delay each String to emulate an intense calculation
        Observable<String> processStrings = items.concatMap(s ->
                Observable.just(s)
                        .delay(randomSleepTime(),
                                TimeUnit.MILLISECONDS)
        );
//run processStrings every 5 seconds, and kill eachprevious instance to start next
        Observable.interval(5, TimeUnit.SECONDS)
                .switchMap(i ->
                        processStrings
                                .doOnDispose(() ->
                                        System.out.println("Disposing! Starting next"))
                ).subscribe(System.out::println);
//keep application alive for 20 seconds
        sleep(20000);
    }
    public static int randomSleepTime() {
//returns random sleep time between 0 to 2000 milliseconds
        return ThreadLocalRandom.current().nextInt(2000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    @Override
    public void start(Stage stage) throws Exception {
        VBox root = new VBox();
        Label counterLabel = new Label("");
        ToggleButton startStopButton = new ToggleButton();
// Multicast the ToggleButton's true/false selected state
        Observable<Boolean> selectedStates =
                JavaFxObservable.valuesOf(startStopButton.selectedProperty())
                        .publish()
                        .autoConnect(2);
// Using switchMap() with ToggleButton's selected state willdrive
// whether to kick off an Observable.interval(),
// or dispose() it by switching to empty Observable
        selectedStates.switchMap(selected -> {
            if (selected)
                return Observable.interval(1,
                        TimeUnit.MILLISECONDS);
            else
                return Observable.empty();
        }).observeOn(JavaFxScheduler.platform()) // Observe on JavaFX UI thread
                .map(Object::toString)
                .subscribe(counterLabel::setText);
// Change ToggleButton's text depending on its state
        selectedStates.subscribe(selected ->
                startStopButton.setText(selected ? "STOP" :
                        "START")
        );
        root.getChildren().addAll(counterLabel, startStopButton);
        stage.setScene(new Scene(root));
        stage.show();
    }

```


```java


    @Override
    public void start(Stage stage) throws Exception {
        VBox root = new VBox();
        root.setMinSize(200, 100);
        Label typedTextLabel = new Label("");
        root.getChildren().addAll(typedTextLabel);
        Scene scene = new Scene(root);
// Multicast typed keys
        Observable<String> typedLetters =
                JavaFxObservable.eventsOf(scene,
                        KeyEvent.KEY_TYPED)
                        .map(KeyEvent::getCharacter)
                        .share();
// Signal 300 milliseconds of inactivity
        Observable<String> restSignal =
                typedLetters
                        .throttleWithTimeout(500,
                                TimeUnit.MILLISECONDS)
                        .startWith(""); //trigger initial
// switchMap() each period of inactivity to
// an infinite scan() concatenating typed letters
        restSignal.switchMap(s ->
                typedLetters.scan("", (rolling, next) -> rolling +
                        next)
        ).observeOn(JavaFxScheduler.platform())
                .subscribe(s -> {
                    typedTextLabel.setText(s);
                    System.out.println(s);
                });
        stage.setScene(scene);
        stage.show();
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 10)
                .buffer(2, 3)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 10)
                .buffer(3, 1)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 10)
                .buffer(2, 1)
                .filter(c -> c.size() == 2)
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.interval(300, TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 300) // map to elapsed time
                .buffer(1, TimeUnit.SECONDS)
                .subscribe(System.out::println);
        sleep(4000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.interval(300, TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 300) // map to elapsed time
                .buffer(1, TimeUnit.SECONDS, 2)
                .subscribe(System.out::println);
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Long> cutOffs =
                Observable.interval(1, TimeUnit.SECONDS);
        Observable.interval(300, TimeUnit.MILLISECONDS)
                .map(i -> (i + 1) * 300) // map to elapsed time
                .buffer(cutOffs)
                .subscribe(System.out::println);
        sleep(5000);
    }
    public static void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 50)
                .window(8)
                .flatMapSingle(obs -> obs.reduce("", (total,
                                                      next) -> total
                        + (total.equals("") ? "" : "|") + next))
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 50)
                .window(2, 3)
                .flatMapSingle(obs -> obs.reduce("", (total,
                                                      next) -> total
                        + (total.equals("") ? "" : "|") + next))
                .subscribe(System.out::println);
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 999_999_999)
                .map(MyItem::new)
                .subscribe(myItem -> {
                    sleep(50);
                    System.out.println("Received MyItem " +
                            myItem.id);
                });
    }
    static void sleep(long milliseconds) {
        try {
            Thread.sleep(milliseconds);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    static final class MyItem {
        final int id;
        MyItem(int id) {
            this.id = id;
            System.out.println("Constructing MyItem " + id);
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> source = Observable.range(1, 1000);
        source.toFlowable(BackpressureStrategy.BUFFER)
                .observeOn(Schedulers.io())
                .subscribe(System.out::println);
        sleep(10000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable<Integer> integers =
                Flowable.range(1, 1000)
                        .subscribeOn(Schedulers.computation());
        Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
                .flatMap(s -> integers.map(i -> i + "-" +
                        s).toObservable())
                .subscribe(System.out::println);
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.interval(1, TimeUnit.MILLISECONDS)
                .onBackpressureBuffer()
                .observeOn(Schedulers.io())
                .subscribe(i -> {
                    sleep(5);
                    System.out.println(i);
                });
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.interval(1, TimeUnit.MILLISECONDS)
                .onBackpressureBuffer(10,
                        () -> System.out.println("overflow!"),
                        BackpressureOverflowStrategy.DROP_LATEST)
                .observeOn(Schedulers.io())
                .subscribe(i -> {
                    sleep(5);
                    System.out.println(i);
                });
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.interval(1, TimeUnit.MILLISECONDS)
                .onBackpressureLatest()
                .observeOn(Schedulers.io())
                .subscribe(i -> {
                    sleep(5);
                    System.out.println(i);
                });
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.interval(1, TimeUnit.MILLISECONDS)
                .onBackpressureDrop(i ->
                        System.out.println("Dropping " + i))
                .observeOn(Schedulers.io())
                .subscribe(i -> {
                    sleep(5);
                    System.out.println(i);
                });
        sleep(5000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        randomGenerator(1, 10000)
                .subscribeOn(Schedulers.computation())
                .doOnNext(i -> System.out.println("Emitting " +
                        i))
                .observeOn(Schedulers.io())
                .subscribe(i -> {
                    sleep(50);
                    System.out.println("Received " + i);
                });
        sleep(10000);
    }
    static Flowable<Integer> randomGenerator(int min, int max) {
        return Flowable.generate(emitter ->
                emitter.onNext(ThreadLocalRandom.current().nextInt(min, max))
        );
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        rangeReverse(100, -100)
                .subscribeOn(Schedulers.computation())
                .doOnNext(i -> System.out.println("Emitting " +
                        i))
                .observeOn(Schedulers.io())
                .subscribe(i -> {
                    sleep(50);
                    System.out.println("Received " + i);
                });
        sleep(50000);
    }
    static Flowable<Integer> rangeReverse(int upperBound, int
            lowerBound) {
        return Flowable.generate(() -> new
                        AtomicInteger(upperBound + 1),
                (state, emitter) -> {
                    int current = state.decrementAndGet();
                    emitter.onNext(current);
                    if (current == lowerBound)
                        emitter.onComplete();
                }
        );
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 999_999_999)
                .map(MyItem::new)
                .observeOn(Schedulers.io())
                .subscribe(myItem -> {
                    sleep(50);
                    System.out.println("Received MyItem " +
                            myItem.id);
                });
        sleep(Long.MAX_VALUE);
    }
    static void sleep(long milliseconds) {
        try {
            Thread.sleep(milliseconds);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    static final class MyItem {
        final int id;
        MyItem(int id) {
            this.id = id;
            System.out.println("Constructing MyItem " + id);
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.range(1, 999_999_999)
                .map(MyItem::new)
                .observeOn(Schedulers.io())
                .subscribe(myItem -> {
                    sleep(50);
                    System.out.println("Received MyItem " +
                            myItem.id);
                });
        sleep(Long.MAX_VALUE);
    }
    static void sleep(long milliseconds) {
        try {
            Thread.sleep(milliseconds);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    static final class MyItem {
        final int id;
        MyItem(int id) {
            this.id = id;
            System.out.println("Constructing MyItem " + id);
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.range(1, 1000)
                .doOnNext(s -> System.out.println("Source pushed "
                        + s))
                .observeOn(Schedulers.io())
                .map(i -> intenseCalculation(i))
                .subscribe(s -> System.out.println("Subscriber received " + s),
                        Throwable::printStackTrace,
                        () -> System.out.println("Done!")
                );
        sleep(20000);
    }
    public static <T> T intenseCalculation(T value) {
//sleep up to 200 milliseconds
        sleep(ThreadLocalRandom.current().nextInt(200));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.range(1, 1000)
                .doOnNext(s -> System.out.println("Source pushed "
                        + s))
                .observeOn(Schedulers.io())
                .map(i -> intenseCalculation(i))
                .subscribe(new Subscriber<Integer>() {
                    @Override
                    public void onSubscribe(Subscription
                                                    subscription) {
                        subscription.request(Long.MAX_VALUE);
                    }
                    @Override
                    public void onNext(Integer s) {
                        sleep(50);
                        System.out.println("Subscriber received " +
                                s);
                    }
                    @Override
                    public void onError(Throwable e) {
                        e.printStackTrace();
                    }
                    @Override
                    public void onComplete() {
                        System.out.println("Done!");
                    }
                });
        sleep(20000);
    }
    public static <T> T intenseCalculation(T value) {
//sleep up to 200 milliseconds
        sleep(ThreadLocalRandom.current().nextInt(200));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.range(1, 1000)
                .doOnNext(s -> System.out.println("Source pushed "
                        + s))
                .observeOn(Schedulers.io())
                .map(i -> intenseCalculation(i))
                .subscribe(new Subscriber<Integer>() {
                    Subscription subscription;
                    AtomicInteger count = new AtomicInteger(0);
                    @Override
                    public void onSubscribe(Subscription
                                                    subscription) {
                        this.subscription = subscription;
                        System.out.println("Requesting 40 items!");
                        subscription.request(40);
                    }
                    @Override
                    public void onNext(Integer s) {
                        sleep(50);
                        System.out.println("Subscriber received " +
                                s);
                        if (count.incrementAndGet() % 20 == 0 &&
                                count.get() >= 40)
                            System.out.println("Requesting 20 more !");
                        subscription.request(20);
                    }
                    @Override
                    public void onError(Throwable e) {
                        e.printStackTrace();
                    }
                    @Override
                    public void onComplete() {
                        System.out.println("Done!");
                    }
                });
        sleep(20000);
    }
    public static <T> T intenseCalculation(T value) {
//sleep up to 200 milliseconds
        sleep(ThreadLocalRandom.current().nextInt(200));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Flowable.range(1, 1000)
                .doOnNext(s -> System.out.println("Source pushed "
                        + s))
                .observeOn(Schedulers.io())
                .map(i -> intenseCalculation(i))
                .subscribe(new Subscriber<Integer>() {
                    Subscription subscription;
                    AtomicInteger count = new AtomicInteger(0);
                    @Override
                    public void onSubscribe(Subscription
                                                    subscription) {
                        this.subscription = subscription;
                        System.out.println("Requesting 40 items!");
                        subscription.request(40);
                    }
                    @Override
                    public void onNext(Integer s) {
                        sleep(50);
                        System.out.println("Subscriber received " +
                                s);
                        if (count.incrementAndGet() % 20 == 0 &&
                                count.get() >= 40)
                            System.out.println("Requesting 20 more !");
                        subscription.request(20);
                    }
                    @Override
                    public void onError(Throwable e) {
                        e.printStackTrace();
                    }
                    @Override
                    public void onComplete() {
                        System.out.println("Done!");
                    }
                });
        sleep(20000);
    }
    public static <T> T intenseCalculation(T value) {
//sleep up to 200 milliseconds
        sleep(ThreadLocalRandom.current().nextInt(200));
        return value;
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable<Integer> source = Observable.create(emitter -> {
            for (int i = 0; i <= 1000; i++) {
                if (emitter.isDisposed())
                    return;
                emitter.onNext(i);
            }
            emitter.onComplete();
        });
        source.observeOn(Schedulers.io())
                .subscribe(System.out::println);
        sleep(1000);
    }

```


```java


    public static void main(String[] args) {
        Flowable<Integer> source = Flowable.create(emitter -> {
            for (int i = 0; i <= 1000; i++) {
                if (emitter.isCancelled())
                    return;
                emitter.onNext(i);
            }
            emitter.onComplete();
        }, BackpressureStrategy.BUFFER);
        source.observeOn(Schedulers.io())
                .subscribe(System.out::println);
        sleep(1000);
    }
    public static void sleep(long millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .compose(toImmutableList())
                .subscribe(System.out::println);
        Observable.range(1, 10)
                .compose(toImmutableList())
                .subscribe(System.out::println);
    }
    public static <T> ObservableTransformer<T, ImmutableList<T>>
    toImmutableList() {
        return upstream ->
                upstream.collect(ImmutableList::<T>builder,
                        ImmutableList.Builder::add)
                        .map(ImmutableList.Builder::build)
                        .toObservable(); // must turn Single into Observable
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
                .toList()
                .compose(toUnmodifiable())
                .subscribe(System.out::println);
    }
    public static <T> SingleTransformer<Collection<T>,
            Collection<T>> toUnmodifiable() {
        return singleObserver ->
                singleObserver.map(Collections::unmodifiableCollection);
    }

```


```java


    public static void main(String[] args) {
        Observable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .compose(joinToString("/"))
                .subscribe(System.out::println);
    }
    public static ObservableTransformer<String, String>
    joinToString(String separator) {
        return upstream -> upstream
                .collect(StringBuilder::new, (b, s) -> {
                    if (b.length() == 0)
                        b.append(s);
                    else
                        b.append(separator).append(s);
                })
                .map(StringBuilder::toString)
                .toObservable();
    }

```


```java


    public static void main(String[] args) {
        Flowable.just("Alpha", "Beta", "Gamma", "Delta",
                "Epsilon")
                .compose(toImmutableList())
                .subscribe(System.out::println);
        Flowable.range(1, 10)
                .compose(toImmutableList())
                .subscribe(System.out::println);
    }
    public static <T> FlowableTransformer<T, ImmutableList<T>>
    toImmutableList() {
        return upstream ->
                upstream.collect(ImmutableList::<T>builder,
                        ImmutableList.Builder::add)
                        .map(ImmutableList.Builder::build)
                        .toFlowable(); // must turn Single into Flowable
    }

```


```java


    public static void main(String[] args) {
        Observable<IndexedValue<String>> indexedStrings =
                Observable.just("Alpha", "Beta", "Gamma", "Delta",
                        "Epsilon")
                        .compose(withIndex());
        indexedStrings.subscribe(v ->
                System.out.println("Subscriber 1: " + v));
        indexedStrings.subscribe(v ->
                System.out.println("Subscriber 2: " + v));
    }
    static <T> ObservableTransformer<T, IndexedValue<T>>
    withIndex() {
        final AtomicInteger indexer = new AtomicInteger(-1);
        return upstream -> upstream.map(v -> new
                IndexedValue<T>(indexer.incrementAndGet(), v));
    }
    static final class IndexedValue<T> {
        final int index;
        final T value;
        IndexedValue(int index, T value) {
            this.index = index;
            this.value = value;
        }
        @Override
        public String toString() {
            return index + " - " + value;
        }
    }

```


```java


    @Override
    public void start(Stage stage) throws Exception {
        VBox root = new VBox();
        Label label = new Label("");
// Observable with second timer
        Observable<String> seconds =
                Observable.interval(1, TimeUnit.SECONDS)
                        .map(i -> i.toString())
                        .observeOn(JavaFxScheduler.platform());
// Turn Observable into Binding
        Binding<String> binding =
                JavaFxObserver.toBinding(seconds);
//Bind Label to Binding
        label.textProperty().bind(binding);
        root.setMinSize(200, 100);
        root.getChildren().addAll(label);
        Scene scene = new Scene(root);
        stage.setScene(scene);
        stage.show();
    }

```


```java


    @Override
    public void start(Stage stage) throws Exception {
        VBox root = new VBox();
        Label label = new Label("");
// Turn Observable into Binding
        Binding<String> binding =
                Observable.interval(1, TimeUnit.SECONDS)
                        .map(i -> i.toString())
                        .observeOn(JavaFxScheduler.platform())
                        .to(JavaFxObserver::toBinding);
//Bind Label to Binding
        label.textProperty().bind(binding);
        root.setMinSize(200, 100);
        root.getChildren().addAll(label);
        Scene scene = new Scene(root);
        stage.setScene(scene);
        stage.show();
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 5)
                .lift(doOnEmpty(() ->
                        System.out.println("Operation 1 Empty!")))
                .subscribe(v -> System.out.println("Operation 1: "
                        + v));
        Observable.<Integer>empty()
                .lift(doOnEmpty(() ->
                        System.out.println("Operation 2 Empty!")))
                .subscribe(v -> System.out.println("Operation 2: "
                        + v));
    }
    public static <T> ObservableOperator<T, T> doOnEmpty(Action
                                                                 action) {
        return new ObservableOperator<T, T>() {
            @Override
            public Observer<? super T> apply(Observer<? super T>
                                                     observer) throws Exception {
                return new DisposableObserver<T>() {
                    boolean isEmpty = true;
                    @Override
                    public void onNext(T value) {
                        isEmpty = false;
                        observer.onNext(value);
                    }
                    @Override
                    public void onError(Throwable t) {
                        observer.onError(t);
                    }
                    @Override
                    public void onComplete() {
                        if (isEmpty) {
                            try {
                                action.run();
                            } catch (Exception e) {
                                onError(e);
                                return;
                            }
                        }
                        observer.onComplete();
                    }
                };
            }
        };
    }

```


```java


    public static void main(String[] args) {
        Observable.range(1, 5)
                .lift(myToList())
                .subscribe(v -> System.out.println("Operation 1: "
                        + v));
        Observable.<Integer>empty()
                .lift(myToList())
                .subscribe(v -> System.out.println("Operation 2: "
                        + v));
    }
    public static <T> ObservableOperator<List<T>, T> myToList() {
        return observer -> new DisposableObserver<T>() {
            ArrayList<T> list = new ArrayList<>();
            @Override
            public void onNext(T value) {
//add to List, but don't pass anything downstream
                list.add(value);
            }
            @Override
            public void onError(Throwable t) {
                observer.onError(t);
            }
            @Override
            public void onComplete() {
                observer.onNext(list); //push List downstream
                observer.onComplete();
            }
        };
    }

```


```java


    public static void main(String[] args) {
        Flowable.range(1, 5)
                .lift(doOnEmpty(() ->
                        System.out.println("Operation 1 Empty!")))
                .subscribe(v -> System.out.println("Operation 1: "
                        + v));
        Flowable.<Integer>empty()
                .lift(doOnEmpty(() ->
                        System.out.println("Operation 2 Empty!")))
                .subscribe(v -> System.out.println("Operation 2: "
                        + v));
    }
    public static <T> FlowableOperator<T, T> doOnEmpty(Action
                                                               action) {
        return new FlowableOperator<T, T>() {
            @Override
            public Subscriber<? super T> apply(Subscriber<? super
                    T> subscriber) throws Exception {
                return new DisposableSubscriber<T>() {
                    boolean isEmpty = true;
                    @Override
                    public void onNext(T value) {
                        isEmpty = false;
                        subscriber.onNext(value);
                    }
                    @Override
                    public void onError(Throwable t) {
                        subscriber.onError(t);
                    }
                    @Override
                    public void onComplete() {
                        if (isEmpty) {
                            try {
                                action.run();
                            } catch (Exception e) {
                                onError(e);
                                return;
                            }
                        }
                        subscriber.onComplete();
                    }
                };
            }
        };
    }

```


