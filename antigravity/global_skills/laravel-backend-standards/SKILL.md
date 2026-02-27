---
name: laravel-backend-standards
description: ATIVAR SEMPRE que o assunto for Laravel, aqui contem codigo de exemplo para ser seguido e melhores praticas, tanto recomendadas como escolhas pessoais.
---

# 🚀 Padrões Backend Laravel (Consolidado)

Este guia é a referência técnica definitiva para o projeto. Ativar sempre que o assunto for Laravel.

## 1. Princípios de Arquitetura e Clean Code

### **Single Responsibility (SRP)**
- **Bad:** Validação e lógica no Controller.
- **Good:** Use FormRequests para validação e Services para lógica.
  ```php
  public function update(UpdateRequest $request, Post $post): JsonResponse {
      $this->postService->update($post, $request->validated());
      return response()->json(['message' => __('posts.updated')]);
  }
  ```

### **Fat Models, Skinny Controllers**
Centralize lógica de DB (queries complexas) em **Scopes** ou métodos da Model.
```php
// Model Client
public function scopeActive($q) => $q->where('active', true)->whereNotNull('verified_at');
public function getWithNewOrders() => $this->active()->with(['orders' => fn($q) => $q->where('created_at', '>', now()->subWeek())])->get();

// Controller
return view('index', ['clients' => $client->getWithNewOrders()]);
```

### **Business Logic em Services**
```php
class ArticleService {
    public function handleUpload($image): void {
        if ($image) $image->move(public_path('images/temp'));
    }
}
```

## 2. Preferências de Codificação e Sintaxe

### **Tipagem Explícita (PHPDoc)**
Realize a declaração de tipagem para retornos genéricos para suporte total da IDE.
```php
/** @var \App\Models\User $user */
$user = Auth::user();

/** @var \Illuminate\Database\Eloquent\Collection<int, \App\Models\Post> $posts */
$posts = Post::all();
```

### **Legibilidade e Formatação**
- **Sintaxe Curta:** `session('cart')`, `$request->name`, `now()`, `back()`, `optional($obj)->id`.
- **Métodos Complexos:** 5+ parâmetros ou tipos `Type|null` devem ser multilinhas.
  ```php
  public function action(
      User $user,
      string $task,
      array $data,
      string|null $note = null,
      bool $force = false
  ): bool|null { ... }
  ```
- **Proibido FQCN:** Use sempre `use` no topo do arquivo.

## 3. Fluxo de Banco de Dados (Obrigatório)
Toda alteração estrutural deve seguir este fluxo:
1. **Migration:** Crie a migration para qualquer mudança (snake_case). FKs com `constrained()`.
2. **Seeder/Factory:** Sempre inclua ou atualize o seeder/factory com dados de exemplo.
3. **Model:** Atualize a Model, inclua PHPDoc de campos no topo e ajuste relações (`hasOne`, `belongsTo`, etc.).

### **Queries e Performance**
- **Eager Loading:** Sempre use `with()` para evitar o problema N+1.
- **Data-heavy:** Use `chunk(500, fn...)` para processamento de grandes volumes.
- **Config:** Nunca use `env()` fora de arquivos em `config/`.

## 4. Validação e Controle de Erros

### **ApiRequest (Validação JSON)**
Estenda `ApiRequest` para formatar erros automaticamente em JSON.
```php
namespace App\Http\Validation;
class ApiRequest extends FormRequest {
    protected function failedValidation(Validator $validator) {
        $errors = collect($validator->errors()->toArray())->map(fn($msgs, $field) => ['field' => $field, 'message' => $msgs[0]])->values();
        throw new HttpResponseException(response()->json(['message' => 'Validation failed', 'errors' => $errors], 422));
    }
}
```

### **Exceptions Globais**
Use `ApiJsonException` para erros padronizados. **Lance exceções**, não retorne erros manuais.
```php
if (!$item) throw new NotFoundException($item, 'Não encontrado');
```

## 5. Autorização e Middlewares
- **Policies:** Obrigatório (evite Gates/Middlewares manuais). `$this->authorize('view', $post);`
- **Logging:** Middleware para monitorar I/O da API.
  ```php
  public function handle(Request $request, Closure $next) {
      \Log::info('API Request', ['method' => $request->method(), 'url' => $request->fullUrl(), 'data' => $request->all()]);
      return $next($request);
  }
  ```

## 6. Convenções de Nomenclatura (PSR-12)

| Item | Padrão | Exemplo |
| :--- | :--- | :--- |
| **Controller / Model** | Singular | `UserController` / `User` |
| **Tabelas / Rotas** | Plural + snake_case | `user_profiles` / `users/1` |
| **Métodos / Vars** | camelCase | `getLatestItems()` / `$userName` |
| **Campos DB / FK** | snake_case | `created_at` / `user_id` |
| **Pivot** | Singular Alpha | `article_user` |

## 7. Repositórios (Exemplo Base Completo)
Use para centralizar o acesso a dados e facilitar mocks em testes.
```php
abstract class Repository {
    protected string $model;
    public function get(int $id, array $with = []): ?Model => $this->model::with($with)->find($id);
    public function all(array $with = []): Collection => $this->model::with($with)->get();
    public function create(array $attr): Model => $this->model::create($attr);
    public function delete(int $id): void => $this->model::destroy($id);
    public function bulkDelete(array $ids): void => $this->model::destroy($ids);
    public function bulkCreate(array $items): void => $this->model::insert($items);
    public function bulkUpdate(array $ids, array $attr): void => $this->model::whereIn('id', $ids)->update($attr);

    public function update(int|Model|Authenticatable $item, array $attr): Model {
        if (is_int($item)) {
            $this->model::whereId($item)->update($attr);
            return $this->model::find($item);
        }
        $item->update($attr); return $item;
    }
}
```