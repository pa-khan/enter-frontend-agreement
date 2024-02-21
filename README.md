# Enter Systems: Frontend arguments
_Версия: 0.0.1_

_Последнее изменение: 21.02.2024_

Настоящий документ регламентирует общие принципы для работы. Любое отклонение является ошибкой и должно быть аргументировано, а также внесено в данный стандарт под версией, если выступает не в качестве частного случая.
## Стек

* NodeJS: 16.x
* Сборщик: [Vite](https://vitejs.dev/)
* TypeScript: [5.3](http://typescript.com/)+, повсеместный
* Фреймворк: [Vue](https://vuejs.org/) 3.4+, Composition API
* Style движок: [UnoCSS](https://unocss.dev/)
* Предпроцессор стилей: [SCSS](https://sass-lang.com/)
* Роутер: [vue-router](https://router.vuejs.org/)
* Store: [class Singleton](#пример-реализации-store)
* Валидация данных: [yup](https://yup-docs.vercel.app/)
* Работа с датами: [Luxon](https://moment.github.io/luxon/#/)
* Интернационализация: [vue-i18n](https://vue-i18n.intlify.dev/)
* Тестирование: [jest](https://jestjs.io/)
* Документация методов и функций: [JSDoc](https://jsdoc.app/)
* Документация и тестирование компонентов: [Storybook](https://storybook.js.org/)
* Рекомендуемая IDE: [WebStorm](https://www.jetbrains.com/webstorm/)
* Прочее: [Babel](https://babeljs.io/)


## EditorConfig
Конфигурация вынесена в[ отдельный файл](editorconfig/.editorconfig).

## Source Folding
```text
assets/
    fonts/ — шрифты
    images/ — изображения
    sprites/ — спрайты (SVG)
    icons/ — иконки
    
components/ — Vue компоненты
    base/ — базовые «строительные»
        BaseButton/
            BaseButton.vue — шаблон
            BaseButton.scss — стили
            BaseButton.test.ts — тесты
            BaseButton.story.ts — история
    single/ — общие, монтируемые лишь 1 раз
    form/ — компоненты манипуляции
    media/ — компоненты подключения изображений, спрайтов, иконок и видео
    typo/ — компоненты типографии
    ui/ — сложносоставные компоненты, дочерние не принимают название родителя
    
composables/ — Vue композиции
    useNet.ts — пример композиции
    
directives/ — Vue директивы
    clickOutside.ts — пример директивы
    
types/ — глобальные TS типы
    type.d.ts — пример файла с типом
    
net/ — взаимодействие с backend
    index.ts — пример файла клиента
    models/ — модели
    
stores/ — хранилища взаимодействия с компонентами
    sessionStore.ts — пример файла хранилища
    
router/ — роутинг
    index.ts — главный файл роутов
    authRoutes.ts — подроуты, разбиты на контекст
    
views/ — компоновки из компонентов для отображения на страницах
    pages/ — страницы
        MainPage.vue — главная страница
        Error4xxPage.page — страница с ошибкой
    Category/ — пример под папки
        CategoryItemPage.vue — пример страницы в подпапке
    layouts/ — шаблоны
        DefaultLayout.vue — шаблон по умолчанию
    modals/ — модальные окна
        LoginModal.vue — пример модального окна
        
styles/ — общие глобальные стили
    main.[s]css
    
i18n/ — интернационализация
    index.ts — конфигурация интернационализации
    locales — папки с локалями
        ru.json — локаль
        
main.ts — точка входа
```



## Линтеры
### Eslint
Вынесен в отдельный [файл](eslint/.eslintrc).

## Форматеры
### Prettier
Вынесен в отдельный [файл](prettierrc/.prettierrc.json).

## JavaScript / TypeScript

### import
Все импорты должны быть выполнены относительно корня проекта определенного алиасом «@».

## Vue
Общей стандарт описан на странице официальной документации: https://vuejs.org/style-guide/

### Пример базового шаблона
```vue
<template>
    <div class="BaseName --checked">
        <div class="BaseName__Wrap">

        </div>
    </div>
</template>
<script setup lang="ts">

</script>
<style src="./BaseName.scss" lang="scss" />
```
Используется модифицированный BEM, где сущности блока и элемента называются с большой буквы, а модификатор начинается с "```--```", пояснения ниже: 
```
BaseName - блок 
BaseName__Wrap - элемент
--checked - модификатор 
```

### Пример реализации Store
Для того чтобы не подключать внешние пакеты, используется class Singleton.
```typescript
import {Ref, ref} from 'vue';

let instance: Ref<SampleStore> | null = null

class SampleStore {
   
}

function useSampleStore() {
    if (!instance) instance = ref(new SampleStore())
    return instance.value
}


export default useSampleStore
```


### Пример группировки состояния
Группировка состояния нужна для определения контекста действий, а также, чтобы названия некаррелировали со значениями props.
```typescript
const state = reactive({
    isLoaded: false,
    isActive: false,
})
```


### Тестирование и документация компонентов
Тестированию и документации подвергаются компоненты, которые выполняют хотя бы одно из следующих требований:
1. Является базовым
2. Имеет более одного состояния в UI (active, focus, error, etc.)
3. Имеет хотя бы логическую перегрузку (поле принимающее ```"string"``` | ```"number"```, выбрасывает значение в зависимости от типа)
4. Имеет сложносоставную структуру, которая нуждается в пояснении

### Документация реализации
Документации подвергаются лишь те языковые конструкции, которые участвуют в бизнес логике или несамодокументриуемые, или нуждающиеся в пояснении.

### Интернационализация (i18n)
Если проект не формата MVP, то все используемые локали должны быть задекларированы в отдельном файле. 

## Bad experience
1. Использование jQuery, Bootstrap и прочих устаревших технологий.
2. Использование нескольких пакетов одного назначения от разных поставщиков.
3. Не использование всей силы TypeScript.
