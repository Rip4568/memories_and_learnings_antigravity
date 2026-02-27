---
name: modern-php-laravel-standards
description: Best practices for modern PHP (8.4+) and Laravel (11+) development, including explicit type hinting, strict typing, and project-specific conventions.
---

# **Modern PHP & Laravel Standards**

This document defines the high-level standards and personal preferences for PHP and Laravel development within this environment.

---

## **1. Type System & Modern PHP (8.4+)**

### **Explicit Nullability**
- **Preference**: Use Union Types for nullability instead of the short `?` syntax.
- **Why**: It is more explicit and consistent with other union types.
- **Example**:
  ```php
  // DO
  public function setAge(int|null $age): void
  
  // AVOID
  public function setAge(?int $age): void
  ```

### **Strict Typing**
- Always include `declare(strict_types=1);` at the top of every PHP file to ensure type safety.

### **Constructor Property Promotion**
- Use constructor property promotion to reduce boilerplate in Services, DTOs, and Actions.
  ```php
  public function __construct(
      protected string $name,
      protected int|null $age = null,
  ) {}
  ```

### **Readonly Properties**
- Use `readonly` for DTOs and value objects to guarantee immutability.

---

## **2. Laravel Architecture**

### **Validation (FormRequests)**
- All validation must reside in **FormRequest** classes (extending `ApiRequest` if standardizing JSON responses).
- Location: `app/Http/Validation` or `app/Http/Requests`.

### **Authorization (Policies)**
- Use **Policies** for all authorization logic. Avoid inline `Gate` checks or complex middleware logic for permissions.

### **Database & Migrations**
- **Migrations**: Required for all structural changes.
- **Seeders/Factories**: Must be updated alongside migrations to ensure local and testing environments stay functional.

### **Controllers**
- Keep controllers "thin".
- Direct model usage for simple CRUD is acceptable (e.g., `User::paginate()`).
- Use **Repositories** only when abstraction is truly beneficial for complex data access.
- **Standard Response**: `return response()->json($data);`

---

## **3. Naming Conventions**

- **Database/Storage Level**: `snake_case` (column names, table names).
- **Application Level (Service/Controller/Logic)**: `camelCase` (variables, methods).
- **Communication**: Always respond to the user in **pt-br**.

---

## **4. Error Handling**

- Prefer throwing specific exceptions (e.g., `ApiJsonException`, `NotFoundException`) over conditional return values in controllers.
- Centralize error formatting in the Exception Handler or dedicated base classes.
