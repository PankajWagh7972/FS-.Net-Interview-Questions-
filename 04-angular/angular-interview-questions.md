# 🚀 Angular Interview Questions & Answers (50+)

**Senior Full Stack / Frontend Engineer Level**

---

# Table of Contents

1. Angular Basics
2. Components & Architecture
3. Data Binding & Directives
4. Services & Dependency Injection
5. Routing & Navigation
6. RxJS & Observables
7. Forms (Reactive & Template-driven)
8. Performance Optimization
9. Security
10. Advanced & Scenario-Based Questions

---

# 1️⃣ Angular Basics

---

## 1. What is Angular?

Angular is a **TypeScript-based frontend framework** for building **scalable, maintainable, enterprise-grade single-page applications (SPA)**.

### Key Features:

* Component-based architecture
* Dependency Injection
* RxJS (Reactive programming)
* Built-in routing
* Strong typing with TypeScript

---

## 2. Difference between AngularJS and Angular?

| AngularJS                  | Angular            |
| -------------------------- | ------------------ |
| JavaScript                 | TypeScript         |
| MVC                        | Component-based    |
| Two-way binding everywhere | Controlled binding |
| Slower                     | Much faster        |
| No mobile focus            | Mobile-first       |

---

## 3. What is SPA?

SPA (Single Page Application) loads **one HTML page** and dynamically updates the UI without full page reload.

**Benefits:**

* Faster performance
* Better UX
* Reduced server load

---

# 2️⃣ Components & Architecture

---

## 4. What is a Component?

Component is the **basic building block of Angular UI**.

```ts
@Component({
  selector: 'app-user',
  templateUrl: './user.component.html'
})
export class UserComponent {}
```

---

## 5. Explain Angular Architecture

Angular follows **component-based layered architecture**:

```
Module → Component → Template → Service → API
```

---

## 6. What is NgModule?

NgModule groups related components, directives, pipes, and services.

```ts
@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  bootstrap: [AppComponent]
})
```

---

## 7. Lifecycle Hooks

| Hook            | Purpose                  |
| --------------- | ------------------------ |
| ngOnInit        | Component initialization |
| ngOnChanges     | Input changes            |
| ngAfterViewInit | View initialization      |
| ngOnDestroy     | Cleanup                  |

---

# 3️⃣ Data Binding & Directives

---

## 8. Types of Data Binding

| Type          | Syntax      |
| ------------- | ----------- |
| Interpolation | {{ value }} |
| Property      | [value]     |
| Event         | (click)     |
| Two-way       | [(ngModel)] |

---

## 9. Example of Two-Way Binding

```html
<input [(ngModel)]="name" />
<p>{{ name }}</p>
```

---

## 10. What are Directives?

Directives manipulate DOM.

### Types:

* Structural → *ngIf, *ngFor
* Attribute → ngClass, ngStyle

---

## 11. Custom Directive Example

```ts
@Directive({ selector: '[appHighlight]' })
export class HighlightDirective {
  constructor(el: ElementRef) {
    el.nativeElement.style.background = 'yellow';
  }
}
```

---

# 4️⃣ Services & Dependency Injection

---

## 12. What is Dependency Injection (DI)?

DI is a **design pattern that provides objects instead of creating them manually**.

---

## 13. Why use Services?

* Business logic separation
* Code reuse
* Centralized API handling

---

## 14. Service Example

```ts
@Injectable({ providedIn: 'root' })
export class UserService {
  getUsers() {
    return this.http.get('/api/users');
  }
}
```

---

# 5️⃣ Routing & Navigation

---

## 15. What is Angular Routing?

Routing enables **SPA navigation without page reload**.

---

## 16. Lazy Loading Example (Very Important)

```ts
{ 
  path: 'admin',
  loadChildren: () => import('./admin/admin.module')
    .then(m => m.AdminModule)
}
```

**Benefits:** Faster load + better performance.

---

## 17. Route Guard Example

```ts
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  canActivate() {
    return !!localStorage.getItem('token');
  }
}
```

---

# 6️⃣ RxJS & Observables (Most Important)

---

## 18. Observable vs Promise

| Observable      | Promise |
| --------------- | ------- |
| Multiple values | Single  |
| Cancelable      | Not     |
| Lazy            | Eager   |

---

## 19. Observable Example

```ts
this.http.get('/api/users')
  .subscribe(data => console.log(data));
```

---

## 20. What is Subject?

Subject acts as **Observable + Observer**.

---

## 21. BehaviorSubject Example

```ts
private data$ = new BehaviorSubject<string>('default');

this.data$.next('new value');
this.data$.subscribe(val => console.log(val));
```

---

## 22. What is debounceTime?

Used for **delay-based optimization**.

```ts
this.search.valueChanges
  .pipe(debounceTime(300))
  .subscribe(val => this.searchApi(val));
```

---

## 23. switchMap vs mergeMap

| switchMap        | mergeMap        |
| ---------------- | --------------- |
| Cancels previous | Keeps all       |
| Search API       | Background jobs |

---

# 7️⃣ Forms

---

## 24. Template-driven vs Reactive Forms

| Template      | Reactive        |
| ------------- | --------------- |
| Simple        | Complex         |
| Less scalable | Highly scalable |

---

## 25. Reactive Form Example

```ts
this.form = this.fb.group({
  email: ['', [Validators.required, Validators.email]]
});
```

---

## 26. Custom Validator Example

```ts
passwordMatch(control: AbstractControl) {
  return control.value === 'admin' ? null : { invalid: true };
}
```

---

# 8️⃣ Performance Optimization (VERY IMPORTANT)

---

## 27. How to optimize Angular performance?

* Lazy loading
* OnPush change detection
* TrackBy
* Pure pipes
* Bundle optimization

---

## 28. ChangeDetectionStrategy.OnPush

```ts
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

---

## 29. TrackBy Example

```html
<li *ngFor="let user of users; trackBy: trackById">
```

```ts
trackById(i, user) { return user.id; }
```

---

# 9️⃣ Security

---

## 30. How does Angular prevent XSS?

* Auto sanitization
* Safe DOM rendering

---

## 31. JWT Interceptor Example

```ts
intercept(req: HttpRequest<any>, next: HttpHandler) {
  const token = localStorage.getItem('token');
  return next.handle(req.clone({
    setHeaders: { Authorization: `Bearer ${token}` }
  }));
}
```

---

## 32. What is Route Guard?

Prevents unauthorized route access.

---

# 🔟 Advanced & Scenario-Based Questions

---

## 33. How to share data between components?

* Input/Output
* Service with Subject
* State management

---

## 34. Parent → Child Communication

```html
<app-child [data]="value"></app-child>
```

---

## 35. Child → Parent Communication

```ts
@Output() notify = new EventEmitter();
```

---

## 36. What is State Management?

Centralized application state handling (NgRx, Akita).

---

## 37. When to use NgRx?

* Large apps
* Shared state
* Complex workflows

---

## 38. How to handle memory leaks?

* Unsubscribe observables
* Async pipe
* TakeUntil pattern

---

## 39. takeUntil Pattern

```ts
private destroy$ = new Subject<void>();

ngOnDestroy() {
  this.destroy$.next();
  this.destroy$.complete();
}
```

---

## 40. What is Zone.js?

Manages async operations and triggers change detection.

---

## 41. What is Ahead-of-Time (AOT) compilation?

Pre-compiles templates → **faster load + smaller bundle**

---

## 42. What is Ivy Renderer?

Angular's **modern rendering engine**.

---

## 43. What is SSR (Server Side Rendering)?

Rendering UI at server → better SEO + faster first load.

---

## 44. What is Angular Universal?

Angular framework for **server-side rendering (SSR)**.

---

## 45. How to optimize large Angular apps?

* Micro frontend
* Lazy loading
* Modular architecture
* State management

---

## 46. What is Web Worker?

Background thread for heavy processing.

---

## 47. How to cancel HTTP requests?

Using **switchMap** or **unsubscribe**.

---

## 48. How to handle API errors globally?

Using **HTTP Interceptors**.

---

## 49. What is PWA in Angular?

Progressive Web Apps → Offline + installable apps.

---

## 50. Angular Project Folder Structure (Best Practice)

```
core/
shared/
modules/
services/
models/
```

---

# ⭐ Bonus Senior-Level Questions

---

## 51. How do you design enterprise Angular architecture?

* Core module
* Shared module
* Feature modules
* Lazy loading
* State management

---

## 52. How to improve Angular load time?

* Lazy loading
* Tree shaking
* Compression
* CDN

---

