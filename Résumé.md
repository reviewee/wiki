This document explains how HTML, CSS, and a bit of JavaScript were used to create a web page with a résumé in it. Keep in mind that most technical decisions were made to get a 100 points in [**Lighthouse**](https://developers.google.com/web/tools/lighthouse/)'s Performance, Progressive Web App, Accessibility, Best Practices, and SEO audits. Hence, as much as possible is inlined, everything else is cached by both browser itself (via a service worker) and GitHub Pages CDN (even though `max-age` there is only 10 minutes), some elements have attributes that aren't commonly used (like ARIA-labels, sadly), and the whole document tries to be _very_ semantic (e.g. `<h1>` element would be _the most important one_, not _the biggest one_).
 Okay, go — start with **index.html**. It's allowed to have one `<head>` element, followed by one `<body>` element, right?

# `<head>`

It includes `<meta>` and `<link>` tags that can be better understood by looking at the source code of [**index.html**](https://github.com/volodymyr-kushnir/volodymyrkushnir.com/blob/develop/index.html#L3-L46) and using an excellent [**HEAD**](https://github.com/joshbuchea/HEAD) for reference.

⚠️ Pay attention to the `<base>` tag, because external resources will be fetched relatively to the path defined in the `<base>` tag, even if the website is being served from the local development machine.

⚠️ [**base.css**](https://volodymyrkushnir.com/assets/stylesheets/base.css) can be included on any webpage with `<link rel="stylesheet" href="https://volodymyrkushnir.com/assets/stylesheets/base.css">` and it's okay for development, but it'd be better to copy the file and host it yourself before publishing the site to production, simply because backwards compatibility is not guaranteed and next versions may potentially break something.

# `<body>`

In this particular case `<body>` consists of `<dialog>`, `<header>`, `<main>`, `<footer>`, and `<script>`. **Only `<main>` tag is required** (it renders the actual résumé), whereas everything else is optional and may or may not be included on the page depending on the developer's choice, e.g. [**volodymyrkushnir.com**](https://volodymyrkushnir.com) uses `<dialog>` for slideshow, includes credits in the `<header>`, uses `<footer>` to explain the purpose of the website, and makes the résumé interactive by running JavaScript from the `<script>` tag.

``` html
  <!-- <dialog> -->
  <!-- <header> -->
  <!-- <main> -->
  <!-- <footer> -->
  <!-- <script> -->
```

## `<dialog>`

## `<header>`

## `<main>`

It all starts with the `main` container which wraps the `article` block that is positioned in the center of the screen so that left, right, and top margins are equal. It may have one or many `.page` blocks. Page will have a frame of a specific color if its `style` attribute includes `border-color` property. Pages will reduce their width and padding relative to the browser viewport width.

``` html
<main>
  <article>
    <div class="page" style="border-color: midnightblue;">
      <!-- Content -->
    </div>
  </article>
</main>
```

### Content

Content can be structured using the `.grid`, which is based on the [16 column grid implementation](https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/components/grid.css) from the excellent [Semantic UI](https://semantic-ui.com) framework. **Grid** consists of `.row`s and `.column`s, as expected. **Rows** are groups of columns which are aligned horizontally. Rows can either be _explicit_, marked with an additional `.row` element, or _implicit_, automatically occurring when no more space is left in a previous row. **Columns** divide horizontal space into indivisible units. All columns in a grid must specify their width as proportion of the total available row width, e.g. `.four.wide`, `.eleven.wide`, etc.

`.stackable` grids will stack their columns on top of each other on mobile devices. `.divider` is useful to visually separate stacked columns. `.mobile.only` grids, rows, or columns would be visible only on mobile devices. `.center.aligned` grids, rows, or columns will attempt to center their content. There're no `.left.aligned` or `.right.aligned` classes, because everything is aligned to the left by default, and aligning to the right is almost never a good idea.  Grids can be nested.

``` html
<div class="stackable grid">
  <div class="row">
    <div class="three wide center aligned column">
      <!-- Avatar -->
    </div>
    <div class="thirteen wide column">
      <div class="stackable grid">
        <div class="sixteen wide column">
          <!-- Name, job, contacts -->
        </div>
        <div class="sixteen wide mobile only column">
          <div class="divider"></div>
        </div>
        <div class="sixteen wide column">
          <!-- Intro -->
        </div>
      </div>
    </div>
  </div>
  <div class="row">
    <div class="sixteen wide column">
      <div class="fat divider"></div>
    </div>
  </div>
  <div class="row">
    <div class="ten wide column">
      <!-- Experience -->
    </div>
    <div class="six wide column">
      <!-- Languages -->
      <!-- Skills -->
      <!-- Character -->
      <!-- Likes -->
      <!-- Dislikes -->
      <!-- Wants -->
      <!-- Education -->
      <!-- Articles -->
    </div>
  </div>
</div>
```

#### Avatar

Avatar is a SVG image loaded into an [`<object>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/object) tag. If SVG specified in `data` attribute is unavailable for some reason, then `<object>` will render its content, which is actually nothing in this particular case (screen readers will read the `aria-label`, though).

``` html
<div class="avatar-wrapper">
  <object class="avatar" data="./assets/images/avatar.svg" role="img" aria-label="My profile picture"></object>
</div>
```

#### Name, job, contacts

`.name` and `.job` are pretty trivial, except maybe that `<h1>` is the largest heading on the page usually, but that's not the case with this website. `.contacts` is simply a list of links (may contain plain text as well) that consist of an icon and a caption. Icons can be obtained from [Font Awesome](https://fontawesome.com/icons).

``` html
<h1 class="name">Volodymyr Kushnir</h1>
<div class="job">full stack developer</div>
<ul class="contacts">
  <li>
    <a href="https://www.fb.com/volodyakushnir" rel="author" class="facebook">
      <svg class="icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><path d="M448 56.7v398.5c0 13.7-11.1 24.7-24.7 24.7H309.1V306.5h58.2l8.7-67.6h-67v-43.2c0-19.6 5.4-32.9 33.5-32.9h35.8v-60.5c-6.2-.8-27.4-2.7-52.2-2.7-51.6 0-87 31.5-87 89.4v49.9h-58.4v67.6h58.4V480H24.7C11.1 480 0 468.9 0 455.3V56.7C0 43.1 11.1 32 24.7 32h398.5c13.7 0 24.8 11.1 24.8 24.7z"/></svg>
      <span>volodyakushnir</span>
    </a>
  </li>
  <li>
    <a href="mailto:volodymyr.kushnir@me.com" rel="author" class="mail">
      <svg class="icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M502.3 190.8c3.9-3.1 9.7-.2 9.7 4.7V400c0 26.5-21.5 48-48 48H48c-26.5 0-48-21.5-48-48V195.6c0-5 5.7-7.8 9.7-4.7 22.4 17.4 52.1 39.5 154.1 113.6 21.1 15.4 56.7 47.8 92.2 47.6 35.7.3 72-32.8 92.3-47.6 102-74.1 131.6-96.3 154-113.7zM256 320c23.2.4 56.6-29.2 73.4-41.4 132.7-96.3 142.8-104.7 173.4-128.7 5.8-4.5 9.2-11.5 9.2-18.9v-19c0-26.5-21.5-48-48-48H48C21.5 64 0 85.5 0 112v19c0 7.4 3.4 14.3 9.2 18.9 30.6 23.9 40.7 32.4 173.4 128.7 16.8 12.2 50.2 41.8 73.4 41.4z"></path></svg>
      <span>volodymyr.kushnir@me.com</span>
    </a>
  </li>
  <li>
    <a href="https://github.com/volodymyr-kushnir" rel="author" class="github">
      <svg class="icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 496 512"><path d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"/></svg>
      <span>volodymyr-kushnir</span>
    </a>
  </li>
  <li>
    <a href="skype:volodymyr_kushnir" rel="author" class="skype">
      <svg class="icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><path d="M424.7 299.8c2.9-14 4.7-28.9 4.7-43.8 0-113.5-91.9-205.3-205.3-205.3-14.9 0-29.7 1.7-43.8 4.7C161.3 40.7 137.7 32 112 32 50.2 32 0 82.2 0 144c0 25.7 8.7 49.3 23.3 68.2-2.9 14-4.7 28.9-4.7 43.8 0 113.5 91.9 205.3 205.3 205.3 14.9 0 29.7-1.7 43.8-4.7 19 14.6 42.6 23.3 68.2 23.3 61.8 0 112-50.2 112-112 .1-25.6-8.6-49.2-23.2-68.1zm-194.6 91.5c-65.6 0-120.5-29.2-120.5-65 0-16 9-30.6 29.5-30.6 31.2 0 34.1 44.9 88.1 44.9 25.7 0 42.3-11.4 42.3-26.3 0-18.7-16-21.6-42-28-62.5-15.4-117.8-22-117.8-87.2 0-59.2 58.6-81.1 109.1-81.1 55.1 0 110.8 21.9 110.8 55.4 0 16.9-11.4 31.8-30.3 31.8-28.3 0-29.2-33.5-75-33.5-25.7 0-42 7-42 22.5 0 19.8 20.8 21.8 69.1 33 41.4 9.3 90.7 26.8 90.7 77.6 0 59.1-57.1 86.5-112 86.5z"/></svg>
      <span>volodymyr_kushnir</span>
    </a>
  </li>
  <li>
    <a href="tel:+380952600797" rel="author" class="phone">
      <svg class="icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><path d="M272 0H48C21.5 0 0 21.5 0 48v416c0 26.5 21.5 48 48 48h224c26.5 0 48-21.5 48-48V48c0-26.5-21.5-48-48-48zM160 480c-17.7 0-32-14.3-32-32s14.3-32 32-32 32 14.3 32 32-14.3 32-32 32z"></path></svg>
      <span>+38 (095) 2600797</span>
    </a>
  </li>
  <li>
    <a href="https://volodymyrkushnir.com/" rel="author" class="website">
      <svg class="icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 496 512"><path fill="currentColor" d="M336.5 160C322 70.7 287.8 8 248 8s-74 62.7-88.5 152h177zM152 256c0 22.2 1.2 43.5 3.3 64h185.3c2.1-20.5 3.3-41.8 3.3-64s-1.2-43.5-3.3-64H155.3c-2.1 20.5-3.3 41.8-3.3 64zm324.7-96c-28.6-67.9-86.5-120.4-158-141.6 24.4 33.8 41.2 84.7 50 141.6h108zM177.2 18.4C105.8 39.6 47.8 92.1 19.3 160h108c8.7-56.9 25.5-107.8 49.9-141.6zM487.4 192H372.7c2.1 21 3.3 42.5 3.3 64s-1.2 43-3.3 64h114.6c5.5-20.5 8.6-41.8 8.6-64s-3.1-43.5-8.5-64zM120 256c0-21.5 1.2-43 3.3-64H8.6C3.2 212.5 0 233.8 0 256s3.2 43.5 8.6 64h114.6c-2-21-3.2-42.5-3.2-64zm39.5 96c14.5 89.3 48.7 152 88.5 152s74-62.7 88.5-152h-177zm159.3 141.6c71.4-21.2 129.4-73.7 158-141.6h-108c-8.8 56.9-25.6 107.8-50 141.6zM19.3 352c28.6 67.9 86.5 120.4 158 141.6-24.4-33.8-41.2-84.7-50-141.6h-108z"></path></svg>
      <span>https://volodymyrkushnir.com/</span>
    </a>
  </li>
</ul>
```

#### Intro

Introduction is a beginning section which states the purpose and goals of the résumé and gives insight into the author's motivations and personality. Here's an example of an introduction that starts with a friendly greeting (in this particular case an em dash is used to denote a direct speech, even though it is uncommon in English):

``` html
<h2>Résumé</h2>
<p>&mdash; Hello! I’m a skilled frontend developer<strong style="margin-left: 0.0625em;">*</strong> with 7 years of experience in UI/UX design, application development, and database modeling. I strive to craft precise, responsive, fast, easy-to-use environments with both strong purpose and great looks.</p>
<p style="opacity: 0.75;"><strong style="margin-right: 0.0625em;">*</strong>I feel like it's 40-60 by <a href="https://css-tricks.com/the-great-divide/" rel="external">The Great Divide</a> into JavaScript engineer and UX engineer</p>
```

#### Experience

Experience is knowledge or skill in a particular job or activity, which you have gained because you have done that job or activity for a long time. Apart from that, even a smallest encounter could change your life forever, therefore it probably has to be mentioned as well, if you feel like it. So this encounters, jobs, and activities are essentially events — short (something that happened in a day) or long (something that took days, months or years to complete). They are organized in a timelines. There can be as many timelines as it is necessary.

``` html
<h3>Experience</h3>
<h6>Recent</h6>
<!-- Timeline (recent events) -->
<h6>Back then</h6>
<!-- Timeline (older events) -->
```

##### Timeline

`.timeline` is a list that groups events together.

``` html
<ul class="timeline">
  <!-- Events -->
</ul>
```

###### Event

Events vary in their complexity depending on how much information is shown about them (dates, places, job description, tasks performed, lessons learned, etc.). They can be simple, detailed, collapsible, or even have their own events. Simple event is a list item with one or two paragraphs. Former is usually the date or period of the event, latter is a description. For example,

``` html
<li>
  <p><em>10<sup>th</sup> September 2016</em></p>
  <p><strong>Relocated</strong> to <strong><a href="https://www.google.com/maps/place/Lviv,+Lviv+Oblast,+Ukraine,+79000/@49.8327787,23.9421957,12z/data=!3m1!4b1!4m5!3m4!1s0x473add7c09109a57:0x4223c517012378e2!8m2!3d49.839683!4d24.029717" rel="external">Lviv</a></strong> <sup><em id="lviv-rent-months"></em></sup></p>
</li>
```

That said, nothing stops developer from adding more paragraphs with more information on the event, and that would make a detailed event. It is better to use lists instead of extra paragraphs, as it would improve the readability. Here's an example of details listed in an unordered list (would draw a black circle ● before each list item): 

``` html
<li>
  <p><em>1<sup>st</sup> October 2016 &ndash; 31<sup>st</sup> December 2016</em></p>
  <p><strong>Full Stack Developer</strong> at <strong><a href="https://www.dutchstar.com/" rel="external">Dutchstar</a></strong></p>
  <ul>
    <li>built, tested, and deployed applications written with React + Redux and Phalcon + MySQL</li>
  </ul>
</li>
```

Sometimes there are events that aren't that important, but still are worthwhile to mention. To save space on the screen and keep readers' attention to important bits some events may be made collapsible (would draw a triangle ▶ before each item) with _closed_ state by default (set `checked` attribute on the `input` to change panels default state to _open_):

``` html
<li class="details">
  <input type="checkbox" id="other">
  <label for="other" class="summary">I also do some other things from time to time &#x1f389;</label>
  <div>
    <ul>
      <li>occasionally I give lectures in <strong><a href="https://academy.binary-studio.com/" rel="external">Binary Studio Academy</a></strong></li>
      <li>sometimes I'm being <em>mentored</em> by those whom I <em>mentor</em></li>
      <li>I'm good at kitchen talks about pretty much anything</li>
      <li>I can play video games with you so that we become a better team</li>
    </ul>
  </div>
</li>
```

[`<details>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details) tag _could_ be used to create collapsible panels, but it is buggy on iOS, hence a cross-platform alternative was built which uses `.details > input:checked ~ div` rule to toggle the panel on and off. Note that `for` attribute of the `label` must match the `id` attribute of the `input`.

Sometimes an event that spawns across longer period of time might have its own events or projects. Of course, those could be listed in a detailed event manner, but it is probably better to use ordered lists instead (would draw a number **1...X** before each item). `.inverted` ordered list would draw a white number in a black square before each item. Enclosed events may also be detailed and/or collapsible. `.highlighted` event will have a subtle gray background. It's possible to create as complex and detailed event as this:

``` html
<li class="highlighted">
  <p><em>24<sup>th</sup> January 2017 – present</em></p>
  <p><strong>Full Stack Developer</strong> at <strong><a href="https://binary-studio.com/" rel="external">Binary Studio</a></strong><br><sup><em>(here's my <a href="./posts/introduction-letter-to-binary-studio/" rel="external"><strong>introduction letter</strong></a>)</em></sup></p>
  <ol class="inverted">
    <li>
      <p><strong>Full Stack Developer</strong> at <strong><a href="https://binary-studio.com/" rel="external">ScreenCloud</a></strong></p>
      <ul>
        <li class="details">
          <input type="checkbox" id="project-2">
          <label for="project-2" class="summary">built, tested, and deployed to Amazon Web Services applications written with Node.js, React, GraphQL, and PostgreSQL</label>
          <div class="labels">
            <a rel="external" class="label" href="https://github.com/apollographql/apollo-client">apollo-client</a>
            <a rel="external" class="label" href="https://github.com/jaredpalmer/formik">formik</a>
            <a rel="external" class="label" href="https://github.com/graphql/graphql-js">graphql</a>
            <a rel="external" class="label" href="https://github.com/apollographql/graphql-tag">graphql-tag</a>
            <a rel="external" class="label" href="https://github.com/ianstormtaylor/slate">slate</a>
            <a rel="external" class="label" href="https://github.com/transloadit/uppy">uppy</a>
            <a rel="external" class="label" href="https://github.com/jquense/yup">yup</a>
            <a rel="external" class="label" href="https://github.com/storybooks/storybook">storybook</a>
            <a rel="external" class="label" href="https://github.com/GoogleChrome/puppeteer">puppeteer</a>
            <a rel="external" class="label" href="https://github.com/visionmedia/debug">debug</a>
            <a rel="external" class="label" href="https://github.com/motdotla/dotenv">dotenv</a>
            <a rel="external" class="label" href="https://github.com/expressjs/express">express</a>
            <a rel="external" class="label" href="https://github.com/hapijs/joi">joi</a>
            <a rel="external" class="label" href="https://github.com/expressjs/morgan">morgan</a>
            <a rel="external" class="label" href="https://github.com/jaredhanson/passport">passport</a>
            <a rel="external" class="label" href="https://github.com/brianc/node-postgres">pg</a>
            <a rel="external" class="label" href="https://github.com/request/request">request</a>
            <a rel="external" class="label" href="https://github.com/graphile/postgraphile">postgraphile</a>
            <a rel="external" class="label" href="https://github.com/chaijs/chai">chai</a>
            <a rel="external" class="label" href="https://github.com/mochajs/mocha">mocha</a>
            <a rel="external" class="label" href="https://github.com/standard/standard">standard</a>
            <a rel="external" class="label" href="https://github.com/standard/snazzy">snazzy</a>
          </div>
        </li>
        <li>configured caching on CloudFront and Elasticache, WebSockets on Elastic Beanstalk, set up API Gateways and Lambdas, etc.</li>
        <li>did code reviews, took care of documentation, planned sprints, participated in retrospectives and daily standups</li>
      </ul>
    </li>
  </ol>
</li>
```

#### Languages

This block can use `.small.tags` to list spoken languages either as an `<a>` link or a simple `<span>`, depending on whether the translation of the résumé is available or not. It's important to set `hreflang`, `lang`, and `rel` attributes to make it easier for the screen readers. Emoji flags would be useful as well. `.language` helps to set proper text decoration (emoji won't be underlined on hover, just the text).

``` html
<section>
  <h3>Languages</h3>
  <p class="tags small"><a href="/uk-ua" hreflang="uk" rel="alternate" class="language" lang="uk">&#x1f1fa;&#x1f1e6; <span>УКРАЇНСЬКА</span></a> <a href="/ru-ru" hreflang="ru" rel="alternate" class="language" lang="ru">&#x1f1f7;&#x1f1fa; <span>РУССКИЙ</span></a> <span class="language" lang="en">&#x1f1fa;&#x1f1f8; ENGLISH</span></p>
</section>
```

#### Skills

``` html
<section>
  <h3>Skills</h3>
  <p class="tags small">HTML CSS JS PHP BOILERPLATE BOOTSTRAP <span class="nowrap">SEMANTIC UI</span> LESS SASS JQUERY ANGULARJS REACT REDUX LODASH GRUNT GULP BOWER WEBPACK NODEJS NPM <span class="nowrap">MICROSOFT OFFICE</span> PHOTOSHOP ILLUSTRATOR DELPHI VB VBA VBSCRIPT LINUX WINDOWS MACOS MYSQL POSTGRESQL MICROSOFT SQL DB2 CCNA ITIL</p>
</section>
```

#### Character

``` html
<section>
  <h3>Character</h3>
  <p class="tags small">EXPLORER STRAIGHTFORWARD <a href="https://www.16personalities.com/entp-personality" rel="external">DEBATER</a> DEVOTED PEDANTIC AGNOSTIC CONTENTIOUS PROCRASTINATOR RELEVANT EMPATHETIC STORYTELLER CONFIDENT CONSISTENT CHALLENGING</p>
</section>
```

#### Likes

``` html
<section>
  <h3>Likes&#x1f603;</h3>
  <p class="tags small">HUMOUR PATIENCE DOGS MOVIES AAPL DEMOCRACY <span class="nowrap">FC BARCELONA</span> JÄGERMEISTER LVIV FRISBEE <button id="midnightblue" class="color midnightblue">MIDNIGHT BLUE</button> ELECTROPOP CUCINA ITALIANA ‘80S SELF-EDUCATION <button id="meh"><svg class="icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512"><path d="M576 256c0 100.586-53.229 189.576-134.123 239.04-7.532 4.606-17.385 2.241-21.997-5.304-4.609-7.539-2.235-17.388 5.304-21.997C496.549 424.101 544 345.467 544 256c0-89.468-47.452-168.101-118.816-211.739-7.539-4.609-9.913-14.458-5.304-21.997 4.608-7.539 14.456-9.914 21.997-5.304C522.77 66.424 576 155.413 576 256zm-96 0c0-66.099-34.976-124.572-88.133-157.079-7.538-4.611-17.388-2.235-21.997 5.302-4.61 7.539-2.236 17.388 5.302 21.998C418.902 152.963 448 201.134 448 256c0 54.872-29.103 103.04-72.828 129.779-7.538 4.61-9.912 14.459-5.302 21.998 4.611 7.541 14.462 9.911 21.997 5.302C445.024 380.572 480 322.099 480 256zm-138.14-75.117c-7.538-4.615-17.388-2.239-21.998 5.297-4.612 7.537-2.241 17.387 5.297 21.998C341.966 218.462 352 236.34 352 256s-10.034 37.538-26.841 47.822c-7.538 4.611-9.909 14.461-5.297 21.998 4.611 7.538 14.463 9.909 21.998 5.297C368.247 314.972 384 286.891 384 256s-15.753-58.972-42.14-75.117zM256 88.017v335.964c0 21.436-25.942 31.999-40.971 16.971L126.059 352H24c-13.255 0-24-10.745-24-24V184c0-13.255 10.745-24 24-24h102.059l88.971-88.954C230.037 56.038 256 66.551 256 88.017zm-32 19.311l-77.659 77.644A24.001 24.001 0 0 1 129.372 192H32v128h97.372a24.001 24.001 0 0 1 16.969 7.028L224 404.67V107.328z"/></svg> MEH
    <audio preload="none">
      <source src="./assets/sounds/meh.mp3" type="audio/mpeg">
      </audio>
    </button> <span class="nowrap">ATTENTION TO DETAILS</span> GRAMMAR SPELLCHECK SEMANTICS MINI-SERIES <span class="nowrap">RYAN GOSLING</span> <span class="nowrap">EMMA STONE</span> ​<span class="nowrap">CURAPROX 5460</span></p>
</section>
```

#### Dislikes

``` html
<section>
  <h3>Dislikes&#x1f61e;</h3>
  <p class="tags small"><span class="nowrap">OWN BAD HABITS</span> POLITICS RACISM ASS-KISSERS VODKA INCOMPETENCE ARROGANCE ENVY GOLF <button id="chartreuse" class="color chartreuse">CHARTREUSE</button> INSECTS METRO PREJUDICE REALITY-SHOWS JŪRMALA</p>
</section>
```

#### Wants

``` html
<section>
  <h3>Wants&#x1f97a;</h3>
  <p class="tags small bulleted"><span class="tag">TO BE FIT AND HEALTHY</span><span class="tag">TO OWN MY SCHEDULE</span><span class="tag">TO READ MORE</span><span class="tag">TO OPEN A CAFÉ</span><span class="tag">TO BE BRAVE</span><span class="tag">TO BECOME A SUPERHERO</span><span class="tag">TO LISTEN AND LEARN</span></p>
</section>
```

#### Education

This block lists all trainings and courses (completed and incomplete, in schools or universities and online, etc.), as well as favorite educational resources, blogs, and subscriptions. It is a combination of a `.timeline` and a `.bulleted.tags`.

``` html
<section>
  <h3>Education</h3>
  <ul class="timeline">
    <li>
      <div><em>Every day</em></div>
      <p class="tags bulleted">
        <span class="tag"><a href="https://frontendfront.com/" rel="external"><strong>Front-End Front</strong></a></span><span class="tag"><a href="https://dribbble.com/" rel="external"><strong>Dribbble</strong></a></span><span class="tag"><a href="https://goodui.org/" rel="external"><strong>GoodUI</strong></a></span><span class="tag"><a href="https://egghead.io/" rel="external"><strong>egghead.io</strong></a></span><span class="tag"><a href="https://theverge.com/" rel="external"><strong>The Verge</strong></a></span><span class="tag"><a href="https://uncrate.com/" rel="external"><strong>Uncrate</strong></a></span>
      </p>
    </li>
    <li>
      <div><em>December 2005 &ndash; April 2006</em></div>
      <div><strong>Certificates of CCNA Course Completion</strong> at <a href="https://www.netacad.com/" rel="external"><strong>Cisco Networking Academy Program</strong></a></div>
    </li>
    <li>
      <div><em>1<sup>st</sup> September 2003 &ndash; 30<sup>th</sup> June 2008</em></div>
      <div><strong>Bachelor's degree</strong> in <strong>computer science</strong> at <a href="http://tntu.edu.ua//?p=en/home" rel="external"><strong>Ternopil National Technical University</strong></a></div>
    </li>
    <li>
      <div><em>1<sup>st</sup> October 1995 &ndash; 23<sup>rd</sup> May 1999</em></div>
      <div><strong>Certificate of graduation</strong> at <strong>Ternopil Art School for Children</strong></div>
    </li>
    <li>
      <div><em>1<sup>st</sup> September 1993 &ndash; 21<sup>st</sup> June 2003</em></div>
      <div><strong>Certificate of graduation</strong> at <strong>№20 Middle School of General Education</strong></div>
    </li>
  </ul>
</section>
```

#### Articles

``` html
<section>
  <h3>Articles</h3>
  <p>None published <sup><em>yet</em></sup></p>
</section>
```

## `<footer>`

## `<script>`