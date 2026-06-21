# Next.js SaaS Frontend Folder Structure Documentation

This is the folder structure I would recommend for a production-grade SaaS application. It follows a **Feature-Based Architecture** similar to what large companies use, while remaining manageable for a solo founder.

---

# Project Structure

```bash
src/
│
├── app/
├── modules/
├── components/
├── hooks/
├── services/
├── store/
├── providers/
├── lib/
├── utils/
├── constants/
├── types/
├── config/
├── styles/
├── assets/
└── tests/
```

---

# 1. app/

Contains all routes using the Next.js App Router.

```bash
app/
│
├── (auth)/
│   ├── login/
│   ├── register/
│   └── forgot-password/
│
├── (dashboard)/
│   ├── dashboard/
│   ├── users/
│   ├── bookings/
│   ├── payments/
│   └── settings/
│
├── api/
│
├── layout.tsx
├── loading.tsx
├── error.tsx
├── not-found.tsx
└── page.tsx
```

### Purpose

Only contains:

* Pages
* Layouts
* Route Groups
* Loading States
* Error Boundaries

Avoid business logic here.

---

# 2. modules/

Contains complete business features.

```bash
modules/
│
├── auth/
├── users/
├── bookings/
├── payments/
├── notifications/
└── analytics/
```

Each feature should be self-contained.

Example:

```bash
modules/
└── users/
    │
    ├── api/
    ├── components/
    ├── hooks/
    ├── schemas/
    ├── services/
    ├── types/
    ├── constants/
    ├── utils/
    └── store/
```

---

## users/api/

API calls specific to Users.

```bash
users/api/

get-users.ts
get-user.ts
create-user.ts
update-user.ts
delete-user.ts
```

Example:

```ts
export const getUsers = async () => {
  return api.get("/users");
};
```

---

## users/components/

Feature-specific components.

```bash
users/components/

UserCard.tsx
UserTable.tsx
UserForm.tsx
UserDrawer.tsx
UserFilters.tsx
```

Used only inside User module.

---

## users/hooks/

Custom hooks.

```bash
users/hooks/

useUsers.ts
useUser.ts
useCreateUser.ts
```

Example:

```ts
export const useUsers = () => {
  return useQuery({
    queryKey: ["users"],
    queryFn: getUsers,
  });
};
```

---

## users/schemas/

Validation schemas.

```bash
users/schemas/

user.schema.ts
```

Example:

```ts
export const userSchema = z.object({
  name: z.string(),
  email: z.email(),
});
```

---

## users/services/

Business logic.

```bash
users/services/

user.service.ts
```

Example:

```ts
export const mapUserResponse = (
  data: UserResponse
): UserModel => {
  return {
    id: data.id,
    name: data.name,
  };
};
```

---

## users/types/

Type definitions.

```bash
users/types/

user.types.ts
```

Example:

```ts
export interface User {
  id: number;
  name: string;
}
```

---

## users/constants/

Feature constants.

```bash
users/constants/

user-status.ts
```

Example:

```ts
export const USER_STATUS = {
  ACTIVE: "ACTIVE",
  INACTIVE: "INACTIVE",
};
```

---

## users/utils/

Feature helper functions.

```bash
users/utils/

user-helper.ts
```

Example:

```ts
export const getFullName = (
  firstName: string,
  lastName: string
) => {
  return `${firstName} ${lastName}`;
};
```

---

## users/store/

Feature state management.

```bash
users/store/

user.store.ts
```

Used if feature needs local state.

---

# 3. components/

Reusable components shared across the application.

```bash
components/
│
├── ui/
├── forms/
├── tables/
├── modals/
├── charts/
├── layouts/
└── common/
```

---

## ui/

Shadcn UI wrappers.

```bash
ui/

Button.tsx
Input.tsx
Select.tsx
Card.tsx
Dialog.tsx
```

---

## forms/

Reusable form components.

```bash
forms/

FormInput.tsx
FormSelect.tsx
FormCheckbox.tsx
```

---

## tables/

Reusable table components.

```bash
tables/

DataTable.tsx
TablePagination.tsx
TableFilters.tsx
```

---

## modals/

Common modal implementations.

```bash
modals/

ConfirmationModal.tsx
DeleteModal.tsx
```

---

## layouts/

Layout components.

```bash
layouts/

Sidebar.tsx
Navbar.tsx
Footer.tsx
DashboardLayout.tsx
```

---

# 4. hooks/

Global reusable hooks.

```bash
hooks/

useDebounce.ts
usePagination.ts
useLocalStorage.ts
useToggle.ts
```

---

# 5. services/

Global API communication layer.

```bash
services/

api-client.ts
axios.ts
```

Example:

```ts
import axios from "axios";

export const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
});
```

---

# 6. store/

Global state.

```bash
store/

auth.store.ts
theme.store.ts
user.store.ts
index.ts
```

Use Zustand unless Redux becomes necessary.

---

# 7. providers/

Application providers.

```bash
providers/

auth-provider.tsx
query-provider.tsx
theme-provider.tsx
```

Example:

```tsx
<QueryClientProvider>
  {children}
</QueryClientProvider>
```

---

# 8. lib/

Third-party configurations.

```bash
lib/

axios.ts
react-query.ts
auth.ts
```

Example:

```ts
lib/auth.ts
lib/query-client.ts
lib/logger.ts
```

---

# 9. utils/

Global helper functions.

```bash
utils/

date.ts
currency.ts
string.ts
file.ts
```

---

# 10. constants/

Application-wide constants.

```bash
constants/

routes.ts
permissions.ts
roles.ts
query-keys.ts
```

Example:

```ts
export const ROUTES = {
  LOGIN: "/login",
  DASHBOARD: "/dashboard",
};
```

---

# 11. types/

Global types.

```bash
types/

api.types.ts
common.types.ts
pagination.types.ts
```

---

# 12. config/

Configuration files.

```bash
config/

env.ts
navigation.ts
sidebar.ts
```

---

# 13. styles/

Styling files.

```bash
styles/

globals.css
variables.css
```

---

# 14. assets/

Static resources.

```bash
assets/

images/
icons/
svgs/
animations/
```

---

# 15. tests/

Testing structure.

```bash
tests/
│
├── unit/
├── integration/
└── e2e/
```

---

# Import Rules

### ✅ Good

```ts
import UserTable from "@/modules/users/components/UserTable";

import Button from "@/components/ui/Button";

import { ROUTES } from "@/constants/routes";
```

### ❌ Bad

```ts
import "../../../../components/Button";
```

Always use path aliases.

---

# Architecture Rules

1. `app/` → Routes only.
2. `modules/` → Business features.
3. `components/` → Shared UI.
4. `services/` → API communication.
5. `hooks/` → Reusable hooks.
6. `store/` → Global state.
7. `utils/` → Pure helper functions.
8. Never place API calls inside components.
9. Never place business logic inside pages.
10. Every feature must be self-contained inside `modules/`.

This structure scales cleanly from a small SaaS MVP to a large application with dozens of features without becoming the "everything inside components/" mess that many Next.js projects eventually turn into.
