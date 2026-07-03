# Сырые материалы для скилла: создание страниц захвата / лендингов

Дата: 2026-07-01  
Проекты по материалам чата: **Golden Connect** и **SolidarTrading**  
Назначение файла: зафиксировать практические правила, технические решения, ошибки и чеклисты, чтобы при следующем создании страницы захвата не забыть важные элементы.

---

## 1. Главный принцип страницы захвата

Страница захвата должна продавать **клик / интерес / действие**, а не сразу сухо объяснять весь продукт.

Для внешнего превью, Telegram-поста, Facebook-поста и первого экрана лучше использовать формулу:

```text
боль / интрига → короткое обещание → переход внутрь
```

Не надо начинать с сухой справки:

```text
Golden Connect — рекламная площадка с инструментами продвижения...
SolidarTrading — проект по...
```

Это раскрывает механику до клика и снижает интригу.

Лучше:

```text
Хочешь зарабатывать больше?
В интернете зарабатывают не лучшие, а те, кого постоянно видят.
Открой секрет рекламы, которая ещё и платит.
```

Или для SolidarTrading:

```text
Большие деньги всегда были рядом
Сегодня мы показываем вход в другой мир. Загляните внутрь.
```

---

## 2. Рабочий порядок создания лендинга

Правильный порядок:

```text
1. Согласовать смысл и структуру лендинга.
2. Согласовать тексты блоков.
3. Подготовить визуальные блоки / изображения.
4. Собрать dev-проект.
5. Проверить десктоп.
6. Проверить мобильное отображение.
7. Настроить ссылки, ref-коды и UTM.
8. Настроить счётчики и цели.
9. Настроить title / description / Open Graph preview.
10. Проверить локально.
11. Сделать npm run build.
12. Опубликовать содержимое dist.
13. Проверить опубликованную версию.
14. Проверить Telegram/Facebook preview.
15. Проверить цели в Метрике.
```

Важно: не переносить аналитику и публикационные правки в конец как второстепенные. Они обязательная часть лендинга.

---

## 3. Типы архивов: dev и upload-ready

Это критично не путать.

### 3.1. Dev-архив

Нужен для разработки и последующей самостоятельной сборки.

Должен содержать:

```text
index.html
src/
public/assets/
package.json
package-lock.json
vite.config.js
tailwind.config.js
postcss.config.js
README.md
```

Не должен содержать:

```text
node_modules/
dist/
```

Рабочий порядок:

```bash
npm install
npm run dev
npm run build
```

### 3.2. Upload-ready / dist-архив

Нужен только для прямой загрузки на сервер.

Должен содержать только результат сборки:

```text
index.html
assets/
```

Его загружают прямо в папку публикации.

### 3.3. Ошибка, которую нельзя повторять

Если пользователь будет сам делать подготовку публикации, ему нужен **dev-архив**, а не готовый `dist`.

Перед выдачей файла надо явно писать:

```text
Это dev-архив для разработки.
```

или:

```text
Это upload-ready dist для прямой загрузки на сервер.
```

---

## 4. Публикация Vite-лендинга в подпапку сайта

Если лендинг публикуется не в корень домена, а в папку, например:

```text
https://invest-expert.info/landings/goldenconnect/index.html
https://invest-expert.info/landings/solidartrading/index.html
```

то в `vite.config.js` обязательно:

```js
import { defineConfig } from 'vite';

export default defineConfig({
  base: './',
  server: {
    host: '127.0.0.1',
    port: 5173,
    open: true
  }
});
```

Иначе после публикации браузер будет искать файлы не там:

```text
Неправильно:
https://invest-expert.info/assets/...

Правильно:
https://invest-expert.info/landings/goldenconnect/assets/...
```

### 4.1. Что загружать на сервер

После:

```bash
npm run build
```

появится папка:

```text
dist/
```

На сервер загружается **содержимое** `dist`, а не сама папка `dist`:

```text
dist/index.html
dist/assets/...
```

Например в:

```text
/public_html/landings/goldenconnect/
```

должны попасть:

```text
index.html
assets/
```

---

## 5. Проверка после публикации

После загрузки сделать:

```text
Ctrl + F5
```

Проверить:

```text
1. Страница открывается.
2. Все картинки видны.
3. Верхнее меню работает.
4. Внутренние переходы скроллят к нужным блокам.
5. Telegram-ссылка содержит правильный ref-код.
6. Ссылка на сайт содержит правильный ref-код.
7. UTM-метки пробрасываются в ссылку сайта.
8. Счётчики стоят в index.html.
9. Цели Метрики срабатывают.
10. OG preview подтягивает нужный title, description и image.
```

Если картинки не отображаются после публикации, первым делом проверить пути к ассетам и `base: './'`.

---

## 6. Open Graph / Telegram / Facebook preview

### 6.1. Зачем это важно

Когда ссылка отправляется в Telegram, Facebook и другие соцсети, пользователь видит карточку:

```text
домен
title
description
картинка
```

Эта карточка может сильно влиять на кликабельность. Для страницы захвата она должна сохранять интригу.

### 6.2. Главный принцип

`title`, `description` и картинка должны быть не сухим описанием проекта, а продолжением первого экрана.

Плохо:

```text
Golden Connect — рекламная площадка с инструментами продвижения, Monar и возможностью кэшбэка до 200%.
```

Лучше:

```text
Хочешь зарабатывать больше?
В интернете зарабатывают не лучшие, а те, кого постоянно видят. Открой секрет рекламы, которая ещё и платит.
```

Для SolidarTrading:

```text
Большие деньги всегда были рядом
Сегодня мы показываем вход в другой мир. Загляните внутрь.
```

### 6.3. Полный набор meta-тегов

Пример для Golden Connect:

```html
<meta name="description" content="В интернете зарабатывают не лучшие, а те, кого постоянно видят. Открой секрет рекламы, которая ещё и платит." />
<link rel="canonical" href="https://invest-expert.info/landings/goldenconnect/index.html" />

<meta property="og:type" content="website" />
<meta property="og:locale" content="ru_RU" />
<meta property="og:site_name" content="Golden Connect" />
<meta property="og:title" content="Хочешь зарабатывать больше?" />
<meta property="og:description" content="В интернете зарабатывают не лучшие, а те, кого постоянно видят. Открой секрет рекламы, которая ещё и платит." />
<meta property="og:url" content="https://invest-expert.info/landings/goldenconnect/index.html" />
<meta property="og:image" content="https://invest-expert.info/landings/goldenconnect/assets/golden-connect-preview-v2.jpg" />
<meta property="og:image:secure_url" content="https://invest-expert.info/landings/goldenconnect/assets/golden-connect-preview-v2.jpg" />
<meta property="og:image:type" content="image/jpeg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta property="og:image:alt" content="Хочешь зарабатывать больше? Стань заметнее." />

<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Хочешь зарабатывать больше?" />
<meta name="twitter:description" content="В интернете зарабатывают не лучшие, а те, кого постоянно видят. Открой секрет рекламы, которая ещё и платит." />
<meta name="twitter:image" content="https://invest-expert.info/landings/goldenconnect/assets/golden-connect-preview-v2.jpg" />
<title>Хочешь зарабатывать больше? | Golden Connect</title>
```

Пример для SolidarTrading:

```html
<meta name="description" content="Сегодня мы показываем вход в другой мир. Загляните внутрь." />
<link rel="canonical" href="https://invest-expert.info/landings/solidartrading/index.html" />

<meta property="og:type" content="website" />
<meta property="og:locale" content="ru_RU" />
<meta property="og:site_name" content="SolidarTrading" />
<meta property="og:title" content="Большие деньги всегда были рядом" />
<meta property="og:description" content="Сегодня мы показываем вход в другой мир. Загляните внутрь." />
<meta property="og:url" content="https://invest-expert.info/landings/solidartrading/index.html" />
<meta property="og:image" content="https://invest-expert.info/landings/solidartrading/assets/solidar-preview-v1.jpg" />
<meta property="og:image:secure_url" content="https://invest-expert.info/landings/solidartrading/assets/solidar-preview-v1.jpg" />
<meta property="og:image:type" content="image/jpeg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />

<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Большие деньги всегда были рядом" />
<meta name="twitter:description" content="Сегодня мы показываем вход в другой мир. Загляните внутрь." />
<meta name="twitter:image" content="https://invest-expert.info/landings/solidartrading/assets/solidar-preview-v1.jpg" />
<title>Большие деньги всегда были рядом | SolidarTrading</title>
```

---

## 7. OG-картинка

### 7.1. Размер

Стандартный размер:

```text
1200×630 px
```

Формат:

```text
JPG
```

Путь для Vite dev-проекта:

```text
public/assets/project-preview-v1.jpg
```

После сборки файл попадёт в:

```text
dist/assets/project-preview-v1.jpg
```

### 7.2. Что лучше использовать

Для страницы захвата лучше всего использовать **скрин первого экрана**, обрезанный под 1200×630.

Почему:

```text
- текст уже утверждён;
- не будет кривой генерации букв;
- картинка совпадает с лендингом;
- Telegram/Facebook показывают понятную карточку;
- сохраняется интрига первого экрана.
```

Не надо заново генерировать картинку с текстом через ИИ, если уже есть хороший экран/скрин. ИИ часто портит читаемость текста, буквы и композицию.

### 7.3. Золотое правило

Если пользователь дал скрин первого экрана и сказал “просто обрежь” — не перерисовывать, не улучшать, не добавлять новый текст. Только обрезать под 1200×630.

### 7.4. Проблемы кэша

Telegram и Facebook могут кэшировать:

```text
- саму страницу;
- картинку по URL;
- старую неудачную попытку получения картинки.
```

Если title/description обновились, а картинка старая или отсутствует, возможно, кэшируется именно URL картинки.

Тогда корректный способ:

```text
1. Переименовать картинку:
   golden-connect-preview.jpg → golden-connect-preview-v2.jpg

2. Обновить все meta-теги:
   og:image
   og:image:secure_url
   twitter:image

3. Пересобрать npm run build.
4. Залить dist.
5. Проверить прямую ссылку на картинку.
6. Проверить ссылку на страницу с новым параметром ?v=2 или ?a0.
```

Важно: параметр у страницы не меняет URL картинки внутри meta-тегов. Если соцсеть кэширует картинку, нужно менять именно имя файла картинки.

### 7.5. Прямая проверка картинки

Перед проверкой в Telegram/Facebook открыть напрямую:

```text
https://invest-expert.info/landings/goldenconnect/assets/golden-connect-preview-v2.jpg
```

или:

```text
https://invest-expert.info/landings/solidartrading/assets/solidar-preview-v1.jpg
```

Если картинка не открывается напрямую, соцсети её тоже не подтянут.

---

## 8. Telegram/Facebook preview: диагностика

### 8.1. Если не обновился текст

Проверить исходник страницы:

```text
view-source:https://invest-expert.info/landings/goldenconnect/index.html
```

Найти:

```text
og:title
og:description
```

Если в `view-source` старый текст — на сервер залит старый `index.html`.

Если в `view-source` новый текст, а Telegram показывает старый — это кэш.

### 8.2. Если текст новый, но картинки нет

Проверить:

```text
1. Есть ли og:image в view-source.
2. Абсолютный ли URL картинки.
3. Открывается ли картинка напрямую.
4. Есть ли og:image:secure_url.
5. Правильный ли тип image/jpeg.
6. Не закэширован ли старый URL картинки.
```

### 8.3. Что делать для кэша

```text
1. Добавить параметр к URL страницы:
   index.html?v=2

2. Если не помогает — сменить имя файла картинки:
   preview-v1.jpg → preview-v2.jpg
```

---

## 9. Метрика и счётчики

### 9.1. Использованные счётчики

В проектах использованы реальные счётчики из SolidarTrading:

```text
1. Яндекс.Метрика
   ID: 109739859

2. LiveInternet
   counter.yadro.ru
```

Яндекс.Метрика ставится в `<head>`.

LiveInternet — скрыто внизу страницы перед `</body>`.

### 9.2. Код Яндекс.Метрики

```html
<!-- Yandex.Metrika counter -->
<script type="text/javascript">
  (function(m,e,t,r,i,k,a){
      m[i]=m[i]||function(){(m[i].a=m[i].a||[]).push(arguments)};
      m[i].l=1*new Date();
      for (var j = 0; j < document.scripts.length; j++) {if (document.scripts[j].src === r) { return; }}
      k=e.createElement(t),a=e.getElementsByTagName(t)[0],k.async=1,k.src=r,a.parentNode.insertBefore(k,a)
  })(window, document,'script','https://mc.webvisor.org/metrika/tag_ww.js?id=109739859', 'ym');

  ym(109739859, 'init', {ssr:true, webvisor:true, clickmap:true, ecommerce:"dataLayer", referrer: document.referrer, url: location.href, accurateTrackBounce:true, trackLinks:true});
</script>
<!-- /Yandex.Metrika counter -->
```

В `<body>` сразу после начала:

```html
<noscript><div><img src="https://mc.yandex.ru/watch/109739859" style="position:absolute; left:-9999px;" alt="" /></div></noscript>
```

### 9.3. LiveInternet

```html
<!-- LiveInternet counter: hidden -->
<div aria-hidden="true" style="position:absolute; left:-9999px; top:-9999px; width:1px; height:1px; overflow:hidden;">
  <script type="text/javascript">
    document.write("<a href='https://www.liveinternet.ru/click' target='_blank' rel='noopener noreferrer'><img src='//counter.yadro.ru/hit?t19.7;r" +
      escape(document.referrer) + ((typeof(screen) == "undefined") ? "" :
      ";s" + screen.width + "*" + screen.height + "*" + (screen.colorDepth ?
      screen.colorDepth : screen.pixelDepth)) + ";u" + escape(document.URL) +
      ";" + Math.random() +
      "' alt='' border='0' width='88' height='31'><\/a>");
  </script>
</div>
<!-- /LiveInternet counter: hidden -->
```

### 9.4. Счётчик один, цели разные

Если несколько лендингов лежат на одном домене в разных папках, один счётчик может собирать всё. Это нормально.

Пример:

```text
/landings/solidartrading/
/landings/goldenconnect/
```

Метрика видит полный URL, поэтому в отчётах можно сегментировать:

```text
URL содержит /landings/goldenconnect/
URL содержит /landings/solidartrading/
```

Но цели лучше называть уникально по проекту, чтобы не смешивать.

---

## 10. Цели Яндекс.Метрики

### 10.1. Не считать внутренние переходы конверсиями

Не надо считать конверсиями клики по внутренним блокам:

```text
Показать как
Хочу 200%
ХОЧУ ВСЁ
Занять место
Решай быстро
Хочу попробовать
```

Это не настоящие конверсии, а навигация по лендингу.

Основные конверсии — только внешние действия:

```text
1. Переход в Telegram
2. Переход на сайт/регистрацию
```

### 10.2. Golden Connect: цели

В Метрике создать цели типа:

```text
Целевое событие / JS-событие
```

Цель 1:

```text
Название: GC Telegram
Идентификатор: gc_telegram_click
```

Метрика покажет код:

```js
ym(109739859,'reachGoal','gc_telegram_click')
```

Цель 2:

```text
Название: GC Site
Идентификатор: gc_site_click
```

Код:

```js
ym(109739859,'reachGoal','gc_site_click')
```

### 10.3. SolidarTrading: существующие цели

Для SolidarTrading в памяти проекта были цели:

```text
registration_click
portal_enter_click
```

Их не трогать, если добавляется Golden Connect. Новые цели Golden Connect должны быть с префиксом `gc_`.

### 10.4. Код отправки цели

Пример из Golden Connect:

```js
const YANDEX_METRIKA_ID = 109739859;

function trackConversion(goalName, params = {}) {
  const payload = {
    ref: activeRefCode,
    page: window.location.pathname,
    ...params
  };

  if (typeof window.ym === 'function') {
    window.ym(YANDEX_METRIKA_ID, 'reachGoal', goalName, payload);
  }

  if (window.location.hostname === '127.0.0.1' || window.location.hostname === 'localhost') {
    console.info('[GC conversion]', goalName, payload);
  }
}
```

Привязка к ссылкам:

```html
<a href="..." data-conversion="gc_telegram_click" data-location="topbar">Telegram</a>
<a href="..." data-conversion="gc_site_click" data-location="final">На сайте</a>
```

Обработчик:

```js
function setupTracking() {
  document.addEventListener('click', (event) => {
    const conversionLink = event.target.closest('[data-conversion]');
    if (!conversionLink) return;

    const goalName = conversionLink.dataset.conversion;
    if (goalName !== 'gc_telegram_click' && goalName !== 'gc_site_click') return;

    trackConversion(goalName, {
      location: conversionLink.dataset.location || 'unknown',
      href: conversionLink.getAttribute('href') || ''
    });
  });
}
```

---

## 11. Ref-код и UTM

### 11.1. Зачем

Для трафика одних счётчиков недостаточно. Без UTM часть переходов из Telegram, пересылок и внешних источников уйдёт в “прямые заходы”.

Нужно:

```text
1. Принимать ref-код в URL.
2. Подставлять его в ссылки Telegram и сайта.
3. Сохранять ref-код.
4. Сохранять UTM-метки.
5. Пробрасывать UTM дальше в ссылку сайта.
6. Считать клики по внешним CTA.
```

### 11.2. Базовый ref-код Golden Connect

```text
ge4ktgaf
```

Базовые ссылки:

```text
Telegram:
https://t.me/GoldenConnect_top_bot?start=ref_ge4ktgaf

Сайт:
https://golden-connect.top/?ref=ge4ktgaf
```

### 11.3. Логика ref-кода

Страница должна принимать код из URL. Если код есть и валиден — использовать его. Если кода нет или он невалидный — использовать базовый.

Допустимые параметры:

```text
ref
r
partner
referral
code
start
```

Для `start` убрать префикс:

```text
ref_код
ref-код
```

Паттерн в Golden Connect:

```js
const DEFAULT_REF_CODE = 'ge4ktgaf';
const REF_STORAGE_KEY = 'gc_ref_code';
const REF_CODE_PATTERN = /^[a-zA-Z0-9_-]{4,64}$/;
```

Функция:

```js
function isValidRefCode(value) {
  return typeof value === 'string' && REF_CODE_PATTERN.test(value.trim());
}

function getRefCodeFromUrl() {
  const params = new URLSearchParams(window.location.search);
  const keys = ['ref', 'r', 'partner', 'referral', 'code'];

  for (const key of keys) {
    const rawValue = params.get(key);
    if (isValidRefCode(rawValue)) return rawValue.trim();
  }

  const startValue = params.get('start');
  if (typeof startValue === 'string') {
    const normalized = startValue.trim().replace(/^ref[_-]/i, '');
    if (isValidRefCode(normalized)) return normalized;
  }

  return '';
}
```

### 11.4. Сохранение ref-кода

```js
function getActiveRefCode() {
  const refFromUrl = getRefCodeFromUrl();

  if (isValidRefCode(refFromUrl)) {
    try {
      localStorage.setItem(REF_STORAGE_KEY, refFromUrl);
    } catch {
      // localStorage может быть недоступен в приватном режиме.
    }
    return refFromUrl;
  }

  try {
    const savedRef = localStorage.getItem(REF_STORAGE_KEY);
    if (isValidRefCode(savedRef)) return savedRef.trim();
  } catch {
    // localStorage может быть недоступен в приватном режиме.
  }

  return DEFAULT_REF_CODE;
}
```

### 11.5. Формирование ссылок

```js
const activeRefCode = getActiveRefCode();

const LINKS = {
  site: `https://golden-connect.top/?ref=${encodeURIComponent(activeRefCode)}`,
  telegram: `https://t.me/GoldenConnect_top_bot?start=ref_${encodeURIComponent(activeRefCode)}`
};
```

### 11.6. Проверка ref

Локально:

```text
http://127.0.0.1:5173/?ref=testpartner
```

Должно получиться:

```text
Telegram → https://t.me/GoldenConnect_top_bot?start=ref_testpartner
Сайт → https://golden-connect.top/?ref=testpartner
```

Если открыть без ref:

```text
http://127.0.0.1:5173/
```

должно подставиться:

```text
ge4ktgaf
```

---

## 12. UTM-метки

### 12.1. Стандартные UTM

```text
utm_source
utm_medium
utm_campaign
utm_content
utm_term
```

Примеры:

```text
?utm_source=telegram&utm_medium=post&utm_campaign=gc_launch&utm_content=post_01
```

Для Telegram-канала:

```text
?utm_source=telegram&utm_medium=channel&utm_campaign=gc_launch&utm_content=channel_name
```

Для Facebook рекламы:

```text
?utm_source=facebook&utm_medium=ads&utm_campaign=gc_test&utm_content=creative_01
```

### 12.2. Куда пробрасывать UTM

UTM пробрасывать только в ссылку сайта Golden Connect:

```text
https://golden-connect.top/?ref=ge4ktgaf&utm_source=...
```

В Telegram-ссылку обычные UTM не пробрасывать:

```text
https://t.me/GoldenConnect_top_bot?start=ref_ge4ktgaf
```

Причина: Telegram-бот стандартно читает `start`, а не обычные UTM. Если бот поддержит разные `start`-коды, можно делать отдельные start-коды под источники.

### 12.3. Сохранение UTM

```js
const UTM_STORAGE_KEY = 'gc_utm';

function getUtmParams() {
  const params = new URLSearchParams(window.location.search);
  const keys = ['utm_source', 'utm_medium', 'utm_campaign', 'utm_content', 'utm_term'];
  const result = {};

  keys.forEach((key) => {
    const value = params.get(key);
    if (value) result[key] = value;
  });

  return result;
}

function setupAttribution() {
  const currentUtm = getUtmParams();
  const hasUtm = Object.keys(currentUtm).length > 0;

  if (hasUtm) {
    localStorage.setItem(UTM_STORAGE_KEY, JSON.stringify(currentUtm));
  }

  let savedUtm = {};
  try {
    savedUtm = JSON.parse(localStorage.getItem(UTM_STORAGE_KEY) || '{}');
  } catch {
    savedUtm = {};
  }

  Object.entries(savedUtm).forEach(([key, value]) => {
    document.querySelectorAll(`a[href^="${LINKS.site}"]`).forEach((link) => {
      const url = new URL(link.href);
      if (!url.searchParams.has(key)) url.searchParams.set(key, value);
      link.href = url.toString();
    });
  });
}
```

---

## 13. Мобильная проверка

### 13.1. Как включить мобильную эмуляцию

В Chrome:

```text
F12
Ctrl + Shift + M
```

### 13.2. Важно: масштаб интерфейса DevTools

В SolidarTrading проверка делалась не просто размером окна, а через мобильный режим DevTools и **Zoom интерфейса**.

Не менять масштаб страницы через `Ctrl + -`.

Нужно менять поле **Zoom** в мобильной панели DevTools:

```text
Responsive | 393 | 852 | 50% | DPR 3
```

`Zoom: 50%` в DevTools нужен, чтобы весь экран телефона помещался на мониторе. CSS-ширина телефона остаётся реальной.

### 13.3. Основные размеры

Сначала тестировать:

```text
393 × 852
Zoom: 50%
DPR: 3
```

Потом:

```text
390 × 844 — iPhone 12/13/14
360 × 800 — узкий Android
375 × 667 — старый маленький iPhone
430 × 932 — большой iPhone / Pro Max
```

Если на `360×800` нормально, на большинстве телефонов будет нормально.

### 13.4. Что проверять на мобильном

```text
1. Нет горизонтального скролла.
2. Верхнее меню не ломает экран.
3. Текст не вылезает за края.
4. Кнопки не обрезаются.
5. Кнопки удобно нажимаются пальцем.
6. Главный объект каждого блока виден.
7. Финальный блок читается без зума.
8. Telegram / сайт кнопки видны и работают.
9. Внутренние переходы ведут к правильным блокам.
10. Нет лишней пустоты сверху/снизу.
```

---

## 14. Верхняя панель и кнопки

### 14.1. Что сработало

В Golden Connect после экспериментов лучше оставить простую CSS-панель без картинки:

```text
- старая панель без фоновой картинки;
- простые круглые жёлтые кнопки;
- фон панели сделать светлее и заметнее;
- не использовать размытую/растянутую картинку панели.
```

### 14.2. Что не сработало

Генерированная широкая UI-панель с картинкой создала проблемы:

```text
- растягивалась;
- сжималась;
- становилась мутной;
- ломалась при изменении ширины экрана;
- выглядела не как оригинал.
```

Если когда-либо использовать картинку-панель:

```text
- ставить оригинал без растягивания;
- не масштабировать шире исходника;
- на широком экране центрировать;
- при узком экране пусть обрезается viewport-ом;
- кнопки накладывать поверх;
- не пытаться растягивать картинку до 100vw, если это портит качество.
```

Но текущий вывод: для таких лендингов безопаснее использовать CSS-панель и обычные кнопки.

### 14.3. “Хочу попробовать”

Если последний блок не представлен отдельным пунктом меню, можно не добавлять новый пункт, а сделать кликабельным существующий текст:

```text
Хочу попробовать:
```

Он ведёт на финальный блок:

```text
#join
```

Но это не считается конверсией. Это внутренний переход.

---

## 15. Внутренние переходы по блокам

Верхнее меню Golden Connect:

```text
Секрет → #secret
Кэшбэк → #cashback
Возможности → #features
X2 → #monar
Почему сейчас → #growth
Хочу попробовать: → #join
Telegram → внешняя ссылка Telegram
Сайт → внешняя ссылка сайта
```

Внутренние CTA по логике:

```text
Показать как → #cashback
Хочу 200% → #features
ХОЧУ ВСЁ / И СРАЗУ → #monar
Занять место → #growth
Решай быстро → #join
```

Финальный блок всё равно должен иметь две кнопки “защита от дурака”:

```text
В Telegram
На сайте
```

Причина: даже если кнопки есть в верхнем меню, в финале пользователь должен видеть прямое действие.

---

## 16. Эффекты секций

Пользователь хотел эффекты на **весь блок**, а не на отдельные элементы.

Правильная логика:

```text
- секции имеют класс .section-effect;
- активной становится одна секция;
- эффект применяется к секции целиком;
- не надо анимировать каждый текст/картинку отдельно без необходимости.
```

Пример JS:

```js
const effectSections = Array.from(document.querySelectorAll('.section-effect'));
let activeSection = null;
let sectionTicking = false;

function getActiveSection() {
  if (!effectSections.length) return null;

  const viewportAnchor = window.scrollY + window.innerHeight * 0.52;
  let closest = effectSections[0];
  let closestDistance = Number.POSITIVE_INFINITY;

  effectSections.forEach((section) => {
    const top = section.offsetTop;
    const bottom = top + section.offsetHeight;
    const center = top + section.offsetHeight / 2;

    if (viewportAnchor >= top && viewportAnchor < bottom) {
      closest = section;
      closestDistance = 0;
      return;
    }

    const distance = Math.abs(center - viewportAnchor);
    if (distance < closestDistance) {
      closest = section;
      closestDistance = distance;
    }
  });

  return closest;
}

function updateActiveSection() {
  sectionTicking = false;
  const nextActive = getActiveSection();

  if (nextActive === activeSection) return;

  effectSections.forEach((section) => {
    section.classList.toggle('is-visible', section === nextActive);
  });

  activeSection = nextActive;
}
```

Длительность эффектов в Golden Connect:

```css
--section-effect-duration: 3.2s;
```

---

## 17. Topbar reveal

В Golden Connect верхняя панель скрыта на первом экране и появляется после прокрутки.

Логика:

```js
function setupTopbarReveal() {
  const topbar = document.querySelector('[data-topbar]');
  const hero = document.querySelector('#secret');

  if (!topbar || !hero) return;

  const updateTopbar = () => {
    const trigger = Math.max(160, hero.offsetHeight * 0.72);
    topbar.classList.toggle('topbar-hidden', window.scrollY < trigger);
  };

  updateTopbar();
  window.addEventListener('scroll', updateTopbar, { passive: true });
  window.addEventListener('resize', updateTopbar);
}
```

На первом экране меню не должно закрывать главный заголовок/картинку.

---

## 18. Структура Golden Connect лендинга

Итоговая структура:

```text
1. Первый экран / секрет
   Хотите зарабатывать много?
   Просто станьте заметнее.

2. Кэшбэк
   Реклама, которая платит.
   Кэшбэк до 200%.

3. Всё внутри
   Визуальный блок с аудиторией и сферой.
   Кнопки: ХОЧУ ВСЁ / И СРАЗУ.

4. Monar / X2
   Общая очередь рекламных пакетов.
   Новые и повторные покупки становятся после вас.

5. Почему сейчас
   График роста.
   Пока вы думаете — аудитория растёт.

6. Финальный блок
   Всё уже внутри.
   Реклама работает. Аудитория растёт. Очередь движется.
   Кнопки: В Telegram / На сайте.
```

Финальный блок заменяет старый сухой CTA. Старый последний CTA-блок удалён.

---

## 19. Финальный блок Golden Connect

Финальный текст:

```text
Golden Connect

Всё уже внутри

Реклама работает.
Аудитория растёт.
Очередь движется.

Golden Connect уже собирает возможности в одном месте.

Зайдите и заберите своё:

[В Telegram] [На сайте]
```

Фон: светлая картинка “гипермаркет возможностей / Golden Connect”, чтобы контрастировать с тёмными секциями.

Смысл финала:

```text
человек уже не просто изучает идею — он должен перейти к действию.
```

---

## 20. Принципы изображений для блоков

### 20.1. Изображения должны быть готовыми ассетами

Для лендинга лучше использовать отдельные файлы:

```text
hero-background.png
cashback-podium.png
features-bg.png
features-sphere.png
monar-conveyor.png
growth-chart.png
final-market-bg.png
preview-v*.jpg
```

### 20.2. Если изображение в блоке неправильное

Заменять только конкретный ассет, не трогая всю структуру.

Пример:

```text
public/assets/monar-conveyor.png
```

был заменён новой картинкой “Блок 3.png”.

### 20.3. Не менять утверждённые изображения без запроса

Если пользователь сказал “замени картинку в блоке Monar”, не менять тексты, стиль, аналитику, OG и прочее.

---

## 21. Пост для Telegram о готовой странице

Telegram-пост должен быть оформлен как Telegram-пост, а не сухая статья:

```text
- сильный заголовок;
- ссылка обязательно видна;
- эмодзи;
- выделения;
- короткие блоки;
- можно использовать моноширинный блок для формулы/схемы;
- пустые строки лучше делать с U+2800, чтобы Telegram сохранял интервалы.
```

Важно: если пост объявляет страницу захвата, ссылка должна быть вверху.

Пример финальной логики:

```text
🚀 Готова страница захвата для Golden Connect
⠀
Создана новая воронка продаж / страница захвата по Golden Connect:
⠀
👉 https://invest-expert.info/landings/goldenconnect/index.html
⠀
Это готовый инструмент для привлечения людей в проект:
⠀
`внимание → интерес → объяснение → переход к действию`
⠀
Страница создана в рамках программы:
⠀
«Роботы продают сами себя»
⠀
Кому доступно сейчас:
✅ мои рефералы
✅ участники SolidarTrading
✅ партнёры из нашей структуры
⠀
Пишите мне в личку — дам доступ и объясню, как использовать страницу под себя.
⠀
Остальные участники Golden Connect — решайте вопрос доступа через своё руководство Golden Connect.
⠀
Буду благодарен за обратную связь:
💬 что улучшить
💬 где непонятно
💬 какие ошибки нашли
💬 какие блоки усилить
💬 что добавить для повышения конверсии
⠀
P.S. Лучшие источники трафика и первые результаты уже скоро. Следите за новостями.
```

---

## 22. Тест трафика

Перед тестами трафика должны быть готовы:

```text
1. Лендинг.
2. Превью ссылки.
3. Метрика.
4. Цели.
5. Ref-код.
6. UTM.
7. Проверка мобильной версии.
8. Отдельные ссылки под источники.
```

### 22.1. Примеры UTM-ссылок

Telegram пост:

```text
https://invest-expert.info/landings/goldenconnect/index.html?utm_source=telegram&utm_medium=post&utm_campaign=gc_launch&utm_content=post_01
```

Telegram канал:

```text
https://invest-expert.info/landings/goldenconnect/index.html?utm_source=telegram&utm_medium=channel&utm_campaign=gc_launch&utm_content=channel_name
```

Facebook реклама:

```text
https://invest-expert.info/landings/goldenconnect/index.html?utm_source=facebook&utm_medium=ads&utm_campaign=gc_test&utm_content=creative_01
```

Разные креативы:

```text
utm_content=creative_01
utm_content=creative_02
utm_content=creative_03
```

Разные посты:

```text
utm_content=post_01
utm_content=post_02
utm_content=post_03
```

### 22.2. Что смотреть

```text
1. Визиты по источникам.
2. Доля мобильных.
3. Время на странице.
4. Доскролл до финального блока.
5. gc_telegram_click.
6. gc_site_click.
7. Конверсия = внешние клики / визиты.
8. Разница по utm_source / utm_campaign / utm_content.
```

---

## 23. Проверка целей после публикации

Порядок:

```text
1. Залить сайт.
2. Открыть опубликованную страницу.
3. Кликнуть “В Telegram”.
4. Вернуться.
5. Кликнуть “На сайте”.
6. Подождать 5–15 минут.
7. Проверить цели в Метрике:
   - gc_telegram_click
   - gc_site_click
```

В Метрике цели создаются в том же счётчике, рядом со старыми целями SolidarTrading.

Старые цели не трогать.

---

## 24. Диагностика локально

Локально цели можно логировать в консоль:

```js
if (window.location.hostname === '127.0.0.1' || window.location.hostname === 'localhost') {
  console.info('[GC conversion]', goalName, payload);
}
```

Локально можно проверить:

```text
- ref-код;
- UTM;
- href кнопок;
- внутренние переходы;
- мобильное отображение;
- отсутствие ошибок картинок.
```

Но полноценную проверку Метрики и LiveInternet делать после публикации.

---

## 25. Ошибки, которые уже были и не должны повторяться

### 25.1. Не добавлять лишнюю аналитику

Нельзя самовольно добавлять:

```text
GA4
Meta Pixel
пустые COUNTERS
лишние события внутренних переходов
```

Если пользователь просит счётчики как в SolidarTrading — использовать реальные счётчики из SolidarTrading.

### 25.2. Не считать внутренние переходы конверсиями

Конверсии только внешние:

```text
Telegram
Сайт / регистрация
```

### 25.3. Не выдавать dist вместо dev-архива

Если пользователь продолжает разработку — нужен dev-архив.

### 25.4. Не менять approved-тексты

Если текст блока утверждён, менять только по прямой просьбе.

### 25.5. Не генерировать кривую preview-картинку с текстом

Если нужен текст на preview, лучше использовать скрин первого экрана и обрезать.

### 25.6. Не забывать ссылку в Telegram-посте

Если пост объявляет новую страницу, ссылка должна быть в явном виде в верхней части поста.

### 25.7. Не добавлять новый пункт меню без необходимости

Если можно связать существующее “Хочу попробовать:” с финальным блоком — так лучше.

### 25.8. Не растягивать изображения интерфейса

Если UI-панель или ассет мутнеет от растяжения — не растягивать. Лучше CSS или новый ассет нужной ширины.

---

## 26. Мини-чеклист перед выдачей архива пользователю

Перед тем как отдать файл:

```text
1. Понять: нужен dev-архив или upload-ready dist.
2. Проверить, что в архиве нет node_modules.
3. Проверить, что в dev-архиве нет dist.
4. Проверить npm run build.
5. Проверить, что preview-картинка лежит в public/assets.
6. Проверить, что og:image ведёт на актуальное имя файла.
7. Проверить, что base: './' для публикации в папку.
8. Проверить, что ссылки ref/UTM работают.
9. Проверить, что цели имеют правильные имена.
10. Проверить, что внутренние кнопки не отправляют цели.
11. Проверить мобильную версию хотя бы 393×852 и 360×800.
12. В ответе явно написать, что именно в архиве.
```

---

## 27. Мини-чеклист после публикации

```text
1. Открыть страницу обычной ссылкой.
2. Ctrl + F5.
3. Открыть view-source и проверить OG-теги.
4. Открыть прямую ссылку на OG-картинку.
5. Отправить ссылку в Telegram с новым параметром:
   ?v=2
6. Проверить Facebook preview.
7. Кликнуть Telegram / Сайт.
8. Проверить href содержит нужный ref.
9. Проверить UTM пробрасываются в сайт.
10. Проверить цели Метрики через 5–15 минут.
```

---

## 28. Патч вместо полного архива

Иногда лучше отдавать не весь архив, а только изменённые файлы.

Например для SolidarTrading OG preview были нужны только:

```text
index.html
public/assets/solidar-preview-v1.jpg
```

Такой патч пользователь кладёт в dev-проект, затем сам делает:

```bash
npm run build
```

И публикует новый `dist`.

---

## 29. Итоговые важные URL и значения

### Golden Connect

Лендинг:

```text
https://invest-expert.info/landings/goldenconnect/index.html
```

Базовый ref-код:

```text
ge4ktgaf
```

Telegram:

```text
https://t.me/GoldenConnect_top_bot?start=ref_ge4ktgaf
```

Сайт:

```text
https://golden-connect.top/?ref=ge4ktgaf
```

Цели Метрики:

```text
gc_telegram_click
gc_site_click
```

Preview title:

```text
Хочешь зарабатывать больше?
```

Preview description:

```text
В интернете зарабатывают не лучшие, а те, кого постоянно видят. Открой секрет рекламы, которая ещё и платит.
```

Preview image:

```text
https://invest-expert.info/landings/goldenconnect/assets/golden-connect-preview-v2.jpg
```

### SolidarTrading

Лендинг:

```text
https://invest-expert.info/landings/solidartrading/index.html
```

Preview title:

```text
Большие деньги всегда были рядом
```

Preview description:

```text
Сегодня мы показываем вход в другой мир. Загляните внутрь.
```

Preview image:

```text
https://invest-expert.info/landings/solidartrading/assets/solidar-preview-v1.jpg
```

Старые/существующие цели:

```text
registration_click
portal_enter_click
```

---

## 30. Шаблон задачи для следующего лендинга

При создании нового лендинга сразу пройти этот список:

```text
1. Как называется проект?
2. Какой главный первый экран?
3. Какой главный крючок без раскрытия всей механики?
4. Какие блоки идут по порядку?
5. Где главный CTA?
6. Какие внешние действия считать конверсиями?
7. Какой базовый ref-код?
8. Какие ссылки надо собирать динамически?
9. Какие UTM сохранять и куда пробрасывать?
10. Какие счётчики использовать?
11. Какие цели создать в Метрике?
12. Какой title/description для preview?
13. Какая OG-картинка 1200×630?
14. Это dev-архив или upload-ready dist?
15. Где будет опубликовано: корень или подпапка?
16. Проверен ли base: './'?
17. Проверена ли мобильная версия через DevTools Zoom?
18. Проверена ли публикация?
19. Проверено ли Telegram/Facebook preview?
20. Проверены ли цели?
```

---

## 31. Главный вывод

Создание страницы захвата — это не только дизайн и текст. Полный рабочий результат включает:

```text
лендинг + мобильная версия + ссылки + ref + UTM + счётчики + цели + OG preview + публикация в правильную папку + проверка соцсетей
```

Если хотя бы один пункт пропустить, страница может выглядеть готовой, но трафик, превью или аналитика будут работать неправильно.
