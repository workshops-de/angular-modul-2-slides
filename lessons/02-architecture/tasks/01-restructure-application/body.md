# Restructure Application to Match Architecture

Apply the architecture principles discussed in the slides to a given Angular application.

## Prerequisites

- Angular application to restructure (starter code from the repository)
- Node.js 18+
- Familiarity with Angular components, services, and routing

## Steps

### 1. Feature-driven Structure

Reorganize the app into feature folders by domain (e.g. `users/`, `orders/`, `shared/`). Each feature owns its components, services, and models. Follow Screaming Architecture: the folder structure should reveal what the application does.

---

### 2. Add public-api.ts for Shared Modules

For each shared library or feature, create a `public-api.ts` that re-exports only the intended public API. Internal implementation stays hidden.

```ts
// shared/users/public-api.ts
export { UserListComponent } from './user-list.component';
export { UserService } from './user.service';
```

Other modules import from `shared/users/public-api` instead of individual files.

---

### 3. Configure Boundary Rules with eslint-plugin-boundaries

Install and configure [eslint-plugin-boundaries](https://github.com/javierbrea/eslint-plugin-boundaries) so domains (features) cannot import from each other directly. Define element types and allow/disallow rules.

```bash
npm install eslint-plugin-boundaries --save-dev
```

Define elements (e.g. `users`, `orders`, `shared`) in your ESLint config and add rules so cross-domain imports are disallowed. References: [eslint-plugin-boundaries](https://github.com/javierbrea/eslint-plugin-boundaries), [Sheriff](https://www.berger-engineering.io/articles/scalable-architecture-with-sheriff).

---

### 4. Enforce Component → Facade → Services/Store

Refactor one feature so that:

- Components only talk to a Facade (no direct service/store access)
- The Facade calls services or a store
- Data flows: Component → Facade → Service/Store (and back)

```ts
// Component injects facade only
@Component({
  selector: 'app-user-list',
  template: `...`,
})
export class UserListComponent {
  private readonly userFacade = inject(UserFacade);
  readonly users = this.userFacade.users$;
}

// Facade injects services/store
@Injectable({ providedIn: 'root' })
export class UserFacade {
  private readonly userService = inject(UserService);
  readonly users$ = this.userService.getUsers();
}
```

## Success Criteria

- [ ] App reorganized into feature folders (feature-driven structure)
- [ ] `public-api.ts` exists for each shared/feature module
- [ ] eslint-plugin-boundaries configured and boundary violations reported
- [ ] At least one feature follows Component → Facade → Service/Store
- [ ] Application still runs and tests pass

## Troubleshooting

- **ESLint errors after boundaries config**: Ensure element patterns match your folder structure
- **Circular dependency**: Check that Facades don't import from Components; Components import Facades only
- **Missing exports in public-api**: Add any public symbols used by other features to the barrel file
