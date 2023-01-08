# Sliced File Structure

**Sliced FS** — методология разработки файловой структуры проектов. Основана на опыте разработки приложений на React, но подходит для любых проектов на JavaScript.

> #### Перевод на другие языки
>
> Если у вас есть возможность, пожалуйста, внесите свой вклад и помогите перевести этот документ на другие языки, в первую очередь на английский!

## Проблема

Чаще всего в проектах используют **простую файловую структуру**, которая выглядит так — в корневой директории (`src`) создают поддиректории, соответствующие типам ресурсов, используемых в проекте.

Для примера, файловая структура приложения на React может выглядеть так:

> ```sh
> src
> |-- api
> |-- assets
> |-- components
> |-- contexts
> |-- helpers
> |-- hooks
> |-- values
> ```

Такие ресурсы, как компоненты, помещаются в `components`, хелперы в `helpers`, значения в `values` и т.д.

В **маленьких проектах** такая файловая структура обычно подходит и не вызывает проблем.

Но в **средних-больших**, с увеличением количества ресурсов, начинают возникать сложности:

- **Необходимо бороться с коллизиями имён ресурсов**

  Поскольку в средних-больших проектах много ресурсов, рано или поздно возникает проблема коллизий имён. Обычно её решают двумя способами.

  Первый способ — держать ресурсы на одном уровне вложенности, но усложнить их имена. Например, добавить префиксы. 
  
  Недостаток этого подхода заключается в том, что со временем формируются длинные списки ресурсов. Их **долго листать**, в них **сложно найти** нужное. А сложные имена **менее удобно читать** и **использовать**.
  
  > ```sh
  > components
  > |-- basic-feature-content
  > |-- basic-feature-viewer
  > |-- (...ещё 30 директорий, начинающихся с "basic-feature")
  > |-- experimental-feature-info
  > |-- experimental-feature-view-grid
  > |-- experimental-feature-view-list
  > |-- (...ещё 15 директорий, начинающихся с "experimental-feature")
  > |-- pro-feature-editor
  > |-- (...ещё 10 директорий, начинающихся с "pro-feature")
  > ```

  Второй способ — использовать несколько уровней вложенности.
  
  Недостаток этого подхода заключается в том, что возникает неконтролируемая вложенность. Она **усложняет навигацию** по проекту и **оценку комплексности проекта и отдельного функционала**.

  > ```sh
  > # с виду довольно просто, да?
  > 
  > components
  > |-- basic-feature
  > |-- experimental-feature
  > |-- pro-feature
  > ```

  > ```sh
  > # не тут-то было!
  >
  > components
  > |-- basic-feature
  > |   |-- content
  > |   |-- viewer
  > |-- experimental-feature
  > |   |-- info
  > |   |-- view
  > |       |-- grid
  > |       |-- list
  > |-- pro-feature
  >     |-- editor
  > ```

- **Сложно отследить связи между ресурсами** 

  Если вы работаете над некоторым функционалом в средних-крупных проектах, **понять, какие ресурсы относятся к этому функционалу, и где они располагаются — сложно**. Это происходит из-за слабой группировки ресурсов. Чаще всего, единственный вариант — изучить исходный код и составить представление в голове.

  Эта проблема в разной степени усугубляется, в зависимости от того, какой способ борьбы с коллизиями имён вы выбрали — сложные имена и длинные списки или вложенные директории.

## Решение

**Sliced FS** вводит несколько терминов (**супергруппы**, **группы**) и определяет некоторые правила, чтобы решить эту проблему. Подходит для любых проектов на JavaScript.

Что это даёт:

* в значительной степени решается проблема коллизий имён ресурсов
* без сложных имён и длинных списков
* без неконтролируемой вложенности
* более сильная группировка ресурсов
* возможность визуально изучить, какие ресурсы относятся к определённому функционалу, без изучения исходного кода (приблизительно)
* возможность визуально оценить комплексность проекта в целом и отдельного фунционала (приблизительно)

Файловая структура проекта на React, разработанная с применением **Sliced FS**, может выглядеть так:

> ```sh
> # в src содержатся супергруппы
> 
> src
> |-- basic-feature
> |-- basic-feature.content
> |-- basic-feature.shared
> |-- basic-feature.viewer
> |-- experimental-feature
> |-- experimental-feature.info
> |-- experimental-feature.shared
> |-- experimental-feature.view.grid
> |-- experimental-feature.view.list
> |-- pro-feature
> |-- pro-feature.editor
> ```

> ```sh
> # каждая супергруппа содержит обычные группы
> 
> experimental-feature
> |-- api
> |-- assets
> |-- components
> |-- components.editor
> |-- components.content.audio
> |-- components.content.image
> |-- components.content.video
> |-- contexts
> |-- helpers
> |-- hooks
> |-- values
> |-- index.js
> ```

## Концепция

### Супергруппа

Директория, которая содержит в себе обычные **группы**, любые ресурсы вне групп и индексный модуль. Создаёт новое пространство имён.

> ```sh
> # пример супергруппы
> 
> src
> |-- basic-feature
>     |-- api
>     |-- assets
>     |-- components
>     |-- contexts
>     |-- helpers
>     |-- hooks
>     |-- values
>     |-- config.js
>     |-- index.js
> ```

#### Расположение

Супергруппы располагаются в корневой директории (`src`).

> ```sh
> src
> |-- basic-feature
> |-- basic-feature.content
> |-- basic-feature.shared
> |-- basic-feature.viewer
> |-- experimental-feature
> |-- experimental-feature.info
> |-- experimental-feature.shared
> |-- experimental-feature.view.grid
> |-- experimental-feature.view.list
> |-- pro-feature
> |-- pro-feature.editor
> ```

#### Вложенность

Супергруппы могут быть вложенными. Вложенность выражается на уровне **имён директорий**, через перечисление цепочки родительских супергрупп через точку.

> ```sh
> src
> |-- experimental-feature
> |-- experimental-feature.info
> |-- experimental-feature.shared
> |-- experimental-feature.view.grid
> |-- experimental-feature.view.list
> ```

#### Ограничения

Супергруппы имеют ограничение на **импорт своих внутренних ресурсов** — любые ресурсы, которые будут использоваться снаружи супергруппы, должны экспортироваться через индексный модуль. **Нельзя обращаться к ресурсам напрямую**, в обход индексного модуля.

> `basic-feature/components/layout`:
>
> ```js
> // ✅ так можно
> 
> import ExperimentalFeature from '../../../experimental-feature';
> 
> // ❌ так нельзя, обращение к внутренним ресурсам
> 
> import Layout from '../../../experimental-feature/components/layout';
> ```

Также, супергруппы имеют ограничение на **импорт из других супергрупп** — нельзя импортировать в дочерней супергруппе ресурсы из родительской супергруппы.

> `experimental-feature/components/layout`:
>
> ```js
> // ✅ так можно
> 
> import Content from '../../../experimental-feature.content';
> ```

> `experimental-feature.content/components/content`:
> 
> ```js
> // ❌ так нельзя, обращение к ресурсам родительской супергруппы из дочерней супергруппы
> 
> import ExperimentalFeature from '../../../experimental-feature';
> ```

#### Супергруппы `shared`

Есть исключение — супергруппы `shared`. 

Из такой супергруппы можно импортировать ресурсы напрямую, в обход индексного модуля, но только в её **родительской супергруппе**, а также во всех её **вложенных супергруппах**.

Из этого следует:

- нельзя импортировать ресурсы в супергруппах выше родителя
- ресурсы супергруппы `shared` в корне можно импортировать везде

Супергруппы `shared` **не могут иметь вложенные супергруппы**.

> `basic-feature/components/layout`:
> 
> ```js
> // ✅ так можно
> 
> import Input from '../../../basic-feature.shared/components/input';
> ```

> `basic-feature.content/components/content`:
> 
> ```js
> // ✅ так можно
> 
> import Input from '../../../basic-feature.shared/components/input';
> ```

> `experimental-feature/components/layout`:
> 
> ```js
> // ❌ так нельзя, обращение к ресурсам чужой shared-супергруппы
> 
> import Input from '../../../basic-feature.shared/components/input';
> ```

### Группа

Директория, которая содержит в себе ресурсы. Создаёт новое пространство имён.

> ```sh
> # супергруппа "experimental-feature" содержит обычные группы
> 
> experimental-feature
> |-- api
> |-- assets
> |-- components
> |-- components.editor
> |-- components.content.audio
> |-- components.content.image
> |-- components.content.video
> |-- contexts
> |-- helpers
> |-- hooks
> |-- values
> |-- index.js
> ```

#### Расположение

Группы располагаются внутри супергрупп.

#### Вложенность

Группы могут быть вложенными. Вложенность выражается на уровне **имён директорий**, через перечисление цепочки родительских групп через точку.

> ```sh
> experimental-feature
> |-- components
> |-- components.editor
> |-- components.content.audio
> |-- components.content.image
> |-- components.content.video
> ```

#### Ограничения

Группы имеют ограничение на **импорт из других групп** — нельзя импортировать в дочерней группе ресурсы из родительской группы.

> `experimental-feature/components/content`:
> 
> ```js
> // ✅ так можно
> 
> import Editor from '../../components.editor/editor';
> ```

> `experimental-feature/components.editor/editor`:
> 
> ```js
> // ❌ так нельзя, обращение к ресурсам родительской группы из дочерней группы
> 
> import Content from '../../components/content';
> ```

#### Группы `shared`

Есть исключение — группы `shared`. 

Из такой группы можно импортировать ресурсы как в **родительской группе**, так и во всех её **вложенных группах**.

Из этого следует:

- нельзя импортировать ресурсы в группах выше родителя

Группы `shared` **не могут иметь вложенные группы**.

> `experimental-feature/components/layout`:
> 
> ```js
> // ✅ так можно
> 
> import Button from '../../components.shared/button';
> ```

> `experimental-feature/components.editor/editor`:
> 
> ```js
> // ✅ так можно
> 
> import Button from '../../components.shared/button';
> ```

> `experimental-feature/helpers/render-actions.js`:
> 
> ```js
> // ❌ так нельзя, обращение к ресурсам чужой shared-группы
> 
> import Button from '../../components.shared/button';
> ```

## FAQ

#### Можно ли использовать Sliced FS с TypeScript?

Да!

#### Можно ли использовать Sliced FS с CommonJS?

Да! Только синтаксис модулей будет отличаться.

#### Можно ли использовать Sliced FS с другими языками?

Полагаю, что да! Только система модулей в вашем языке может значительно отличаться. Если у вас получится, пожалуйста, поделитесть опытом в issues.

## Обратная связь

Буду рад любой обратной связи! Пожалуйста, отправляйте её в discussions или issues.

## Поддержать автора

[Boosty](https://boosty.to/austrokhart)
