# 🚀 Advanced RxJS Problem Set with Solutions

> Covers **real-world interview-grade RxJS challenges** for Senior Angular Developers

---

# 📌 Table of Contents

1. Cold vs Hot Observables
2. Subject vs BehaviorSubject vs ReplaySubject
3. Debounce Search API Calls
4. Cancel Previous HTTP Requests
5. Retry with Exponential Backoff
6. API Polling with Stop Condition
7. Sequential API Calls
8. Parallel API Calls
9. Caching HTTP Requests
10. Share API Response Across Components
11. Handle Race Conditions
12. Combine Multiple Streams
13. Form Validation Async
14. WebSocket Reconnection
15. Lazy Load Data
16. Pagination with RxJS
17. Infinite Scroll
18. Auto Refresh Token
19. Progress Bar Loader
20. File Upload with Progress
21. Memory Leak Prevention
22. Dynamic Polling
23. Search Suggestion Engine
24. Error Handling Strategy
25. State Store using RxJS

---

# 1️⃣ Cold vs Hot Observable

### Problem:

Create **cold** and **hot** observable examples.

### Solution:

```ts
// Cold Observable
const cold$ = new Observable(observer => {
  console.log('API Called');
  observer.next(Math.random());
});

cold$.subscribe(v => console.log('Sub1', v));
cold$.subscribe(v => console.log('Sub2', v));

// Hot Observable
const hot$ = cold$.pipe(share());

hot$.subscribe(v => console.log('Hot1', v));
hot$.subscribe(v => console.log('Hot2', v));
```

✅ Cold → each subscriber triggers execution
✅ Hot → shared execution

---

# 2️⃣ Subject vs BehaviorSubject vs ReplaySubject

```ts
const subject = new Subject<number>();
const behavior = new BehaviorSubject<number>(0);
const replay = new ReplaySubject<number>(2);
```

| Type            | Remembers last value | Use Case     |
| --------------- | -------------------- | ------------ |
| Subject         | ❌                    | Events       |
| BehaviorSubject | ✅                    | State        |
| ReplaySubject   | ✅(N)                 | Event replay |

---

# 3️⃣ Debounced Search API Calls (Very Common Interview Q)

```ts
this.search.valueChanges.pipe(
  debounceTime(400),
  distinctUntilChanged(),
  switchMap(q => this.api.search(q))
).subscribe(result => this.data = result);
```

✅ Prevents unnecessary API calls
✅ Improves UX

---

# 4️⃣ Cancel Previous HTTP Requests (SwitchMap)

```ts
this.search$.pipe(
  switchMap(q => this.http.get(`/api/search?q=${q}`))
).subscribe();
```

**Why switchMap?**

> Cancels previous HTTP request if new value comes.

---

# 5️⃣ Retry with Exponential Backoff

```ts
this.http.get('/api/data').pipe(
  retryWhen(errors =>
    errors.pipe(
      scan((count) => count + 1, 0),
      delayWhen(count => timer(count * 1000))
    )
  )
)
```

---

# 6️⃣ API Polling With Stop Condition

```ts
timer(0, 5000).pipe(
  switchMap(() => this.api.getStatus()),
  takeWhile(res => res.status !== 'DONE', true)
).subscribe();
```

---

# 7️⃣ Sequential API Calls (Dependent APIs)

```ts
this.api.getUser().pipe(
  concatMap(user => this.api.getOrders(user.id))
).subscribe();
```

---

# 8️⃣ Parallel API Calls

```ts
forkJoin({
  users: this.api.users(),
  orders: this.api.orders()
}).subscribe(res => console.log(res));
```

---

# 9️⃣ HTTP Caching (Very Important Interview Topic)

```ts
private cache$!: Observable<any>;

getProducts() {
  if (!this.cache$) {
    this.cache$ = this.http.get('/api/products').pipe(
      shareReplay(1)
    );
  }
  return this.cache$;
}
```

---

# 🔟 Share API Response Across Components

```ts
products$ = this.api.getProducts().pipe(shareReplay(1));
```

---

# 11️⃣ Race Condition Handling

```ts
merge(this.click1$, this.click2$).pipe(
  exhaustMap(() => this.api.call())
)
```

---

# 12️⃣ Combine Multiple Streams

```ts
combineLatest([
  this.user$,
  this.cart$
]).subscribe(([user, cart]) => console.log(user, cart));
```

---

# 13️⃣ Async Form Validation

```ts
validate(control: AbstractControl) {
  return this.api.checkUser(control.value).pipe(
    map(res => res.exists ? { taken: true } : null)
  );
}
```

---

# 14️⃣ WebSocket Reconnection Strategy

```ts
webSocket(url).pipe(
  retryWhen(err =>
    err.pipe(delay(2000))
  )
).subscribe();
```

---

# 15️⃣ Lazy Load Data On Scroll

```ts
fromEvent(window, 'scroll').pipe(
  debounceTime(300),
  filter(() => window.scrollY > 500),
  switchMap(() => this.api.getMore())
)
```

---

# 16️⃣ Pagination using RxJS

```ts
this.page$.pipe(
  switchMap(p => this.api.getPage(p))
).subscribe();
```

---

# 17️⃣ Infinite Scroll

```ts
this.scroll$.pipe(
  throttleTime(300),
  exhaustMap(() => this.api.loadMore())
)
```

---

# 18️⃣ Auto Refresh JWT Token

```ts
timer(0, 14 * 60 * 1000).pipe(
  switchMap(() => this.auth.refresh())
).subscribe();
```

---

# 19️⃣ Global Loader Service

```ts
loading$ = new BehaviorSubject(false);

show() { this.loading$.next(true); }
hide() { this.loading$.next(false); }
```

---

# 20️⃣ File Upload with Progress Tracking

```ts
this.http.post(url, formData, {
  reportProgress: true,
  observe: 'events'
}).pipe(
  map(event => {
    if (event.type === HttpEventType.UploadProgress) {
      return Math.round(event.loaded / event.total! * 100);
    }
  })
)
```

---

# 21️⃣ Memory Leak Prevention (takeUntil)

```ts
private destroy$ = new Subject<void>();

this.api.get().pipe(
  takeUntil(this.destroy$)
).subscribe();

ngOnDestroy() {
  this.destroy$.next();
  this.destroy$.complete();
}
```

---

# 22️⃣ Dynamic Polling

```ts
interval(this.pollTime).pipe(
  switchMap(() => this.api.get())
)
```

---

# 23️⃣ Search Suggestion Engine

```ts
this.search$.pipe(
  debounceTime(300),
  distinctUntilChanged(),
  switchMap(q => this.api.suggest(q))
)
```

---

# 24️⃣ Global Error Handling Strategy

```ts
catchError(err => {
  this.toast.error(err.message);
  return EMPTY;
})
```

---

# 25️⃣ State Store using RxJS (Mini Redux)

```ts
private store$ = new BehaviorSubject<AppState>(initialState);

select(selector) {
  return this.store$.pipe(map(selector));
}

update(newState) {
  this.store$.next({...this.store$.value, ...newState});
}
```

---

# 🧠 BONUS: Tricky Interview Questions

### Q: Difference between switchMap vs concatMap vs mergeMap vs exhaustMap?

| Operator   | Behavior                        |
| ---------- | ------------------------------- |
| switchMap  | Cancels previous                |
| concatMap  | Executes sequentially           |
| mergeMap   | Executes parallel               |
| exhaustMap | Ignores new until old completes |

---

# 🎯 Interview Mastery Topics

* State Management using RxJS
* Performance Optimization
* HTTP Caching
* Concurrency control
* Memory leak prevention
* WebSocket handling
* Token refresh strategies

---
