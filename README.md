# Лабораторная работа 6-5

## Дедлайн сдачи работы без пенальти

???

## Как выполнять

- Создать ветку `mod-5` из ветки `mod-4`

- Открыть ветку `mod-5` в workspace

## Отправка формы создания заметки с валидацией

1. Добавить передачу данных в форме добавления заметки

```php
public function store(Request $request)
{
    $data = $request->validate([
        'note' => ['required', 'string']
    ]);

    $data['user_id'] = 1;
    $note = Note::create($data);

    return to_route('note.show', $note)->with('message', 'Note was created');
}
```

- store(Request $request) — метод принимает объект запроса.
- $data = $request->validate([...]) — валидация входных данных, note обязателен и должен быть строкой.
- $data['user_id'] = 1 — добавляется фиксированный user_id.
- $note = Note::create($data) — создаётся запись в базе данных.
- return to_route('note.show', $note)->with('message', 'Note was created') — редирект на страницу заметки с флеш-сообщением

_Для того, чтобы данные правильно передавались, удостоверьтесь, что ваша модель заметок выглядит следующим образом_

```php
class Note extends Model
{
    use HasFactory;

    protected $fillable = ['note', 'user_id'];
}
```

- class Note extends Model — определяет модель Note, которая наследует Model (Eloquent ORM).
- use HasFactory; — подключает трейт HasFactory, позволяющий использовать фабрики для создания тестовых данных.
- protected $fillable = ['note', 'user_id']; — разрешает массовое заполнение (create() и fill()) для полей note и user_id.

2. Добавьте директиву `@session` в `layout.blade.php` перед `slot` для отображения флеш сообщения при редиректе

```php
    @session('message')
        <div class="success-message">
            {{ session('message') }}
        </div>
    @endsession

    {{ @slot }}
```

- @session('message') — Blade-директива, проверяющая наличие флеш-сообщения message в сессии.
- &lt;div class="success-message"&gt; {{ session('message') }} &lt;/div&gt; — выводит сообщение внутри div, если оно есть.
- @endsession — закрывает блок @session.
- {{ @slot }} — попытка вывести слот в Blade, но @slot без @component некорректен. Вероятно, это ошибка.

3. Добавление CSRF токен

Добавьте `@csrf` внутри каждой формы внутри приложения

4. Проверьте, что функционал добавления заметки работает

> [!warning]
> Добавьте функционал добавления задач

## Для работы локально в колледже

В связи с тем, что у нас заблокированы CDN Amazon и Cloudflare мы не можем устанавливать npm зависимости. Они хранятся в вашей папке `31ИСР/_Кутиков`

1. Скачайте ваш код по кнопке `Code` и `Скачать ZIP`

2. Распакуйте _локально_ (не на удаленный диск)

3. Скопируйте файл из вашей папки `"фиксы и зависимости"` и распакуйте тоже локально. Затем перекиньте это в ваш проект с заменой

4. Также сделайте с папкой `"node_modules"`

5. Закомментируйте строчки `tailwindcss: {}` и `autoprefixer: {}` в `note-app/postcss.config.js`

6. Проверьте работоспособность проекта
