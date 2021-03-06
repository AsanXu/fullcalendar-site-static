---
title: Vue Component
title_for_landing: Vue
---

FullCalendar seamlessly integrates with the [Vue] JavaScript framework. It provides a component that exactly matches the functionality of FullCalendar's standard API.

<div class='spec' markdown='1' style='font-family:inherit'>
This package is in **beta**. [Learn more &raquo;](https://fullcalendar.io/blog/2019/04/react-vue-and-angular-connectors)
</div>

This document does not go into depth about initializing a Vue project. However, we have provided an example project for you to consult, which this document roughly follows. It leverages [Webpack] and [Sass]. [View the example project &raquo;][example project]

The first step is to install the FullCalendar-related dependencies. You'll need the Vue adapter, the core package, and any additional plugins you plan to use:

```bash
npm install --save @fullcalendar/vue @fullcalendar/core @fullcalendar/daygrid
```

You may then begin to write a parent component that leverages the `<FullCalendar>` component ([DemoApp.vue]):

```html
<script>

import FullCalendar from '@fullcalendar/vue'
import dayGridPlugin from '@fullcalendar/daygrid'

export default {
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data() {
    return {
      calendarPlugins: [ dayGridPlugin ]
    }
  }
}

</script>

<template>
  <FullCalendar defaultView="dayGridMonth" :plugins="calendarPlugins" />
</template>
```

Notice how you must define the plugins in the data method, and then pass them into the `<FullCalendar>` component. This is the only way!


## CSS

You must manually include the stylesheets for FullCalendar's core and plugins. Assuming you have [Sass] set up, add a style block in the same `.vue` file:

```html
<style lang='scss'>

@import '~@fullcalendar/core/main.css';
@import '~@fullcalendar/daygrid/main.css';

</style>
```

The prefixed `~` tells Sass to look in the `node_modules` directory.


## Props

The `<FullCalendar>` component is equipped with [all of FullCalendar's options][docs toc]! Just pass them in as props. Please be aware of when to prefix `:` (otherwise known as `v-bind:`) to the prop names. If you need a bound JavaScript expression, use `:`. Otherwise, use a normal attribute. Example:

```html
<FullCalendar
  defaultView="dayGridMonth"
  :plugins="calendarPlugins"
  :weekends="false"
  :events="[
    { title: 'event 1', date: '2019-04-01' },
    { title: 'event 2', date: '2019-04-02' }
  ]"
  />
```


## Emitted Events

A listener can be passed into a Vue component that will be called when something happens. For example, the [dateClick](dateClick) handler is called whenever the user clicks on a date. The way you pass these into the `<FullCalendar>` component is different than props:

```html
<FullCalendar @dateClick="handleDateClick" :plugins="calendarPlugins" />
```

They are prefixed with `@` (otherwise known as `v-on:`). Then, in the JavaScript, write your handler method:

```js
methods: {
  handleDateClick(arg) {
    alert(arg.date)
  }
}
```

Don't mix up when to use props versus events! For help deciding which is which, consult the [giant list of props and events][component options] in the connector's source code.


## Accessing FullCalendar's API

Hopefully you won't need to do it often, but sometimes it's useful to access the underlying `Calendar` object for raw data and methods.

This is especially useful for controlling the current date. The [defaultDate](defaultDate) prop will set the *initial* date of the calendar, but to change it after that, you'll need to rely on the [date navigation methods](date-navigation).

To do something like this, you'll need to get ahold of the component's ref (short for "reference"). In the template:

```html
<FullCalendar ref="fullCalendar" :plugins="calendarPlugins" />
```

Once you have the ref, you can get the underlying `Calendar` object via the `getApi` method:

```js
let calendarApi = this.$refs.fullCalendar.getApi()
calendarApi.next()
```


## Kebab-case in Markup

Some people prefer to write component names and prop names in kebab-case when writing markup. This will work fine:

```html
<full-calendar default-view="dayGridMonth" :plugins="calendarPlugins" />
```

Notice how property *values* must remain in their original case.


## Scheduler

How do you use [FullCalendar Scheduler's](scheduler) premium plugins with Vue? They are no different than any other plugin. Just follow the same instructions as you did `dayGridPlugin` in the above example. Also, make sure to include your `schedulerLicenseKey`:

```html
<script>
import FullCalendar from '@fullcalendar/vue'
import resourceTimelinePlugin from '@fullcalendar/resource-timeline'

export default {
  components: {
    FullCalendar
  },
  data() {
    return {
      calendarPlugins: [ resourceTimelinePlugin ]
    }
  }
}

</script>

<template>
  <FullCalendar schedulerLicenseKey="XXX" :plugins="calendarPlugins" />
</template>

<style lang='scss'>

@import '~@fullcalendar/core/main.css';
@import '~@fullcalendar/timeline/main.css';
@import '~@fullcalendar/resource-timeline/main.css';

</style>
```


## Misc

We've adapted this example to run in a code playground. [View the runnable example &raquo;]({{ site.fullcalendar_vue_playground }})

This package is released under an MIT license, the same license the standard version of FullCalendar uses. Feel free to [browse the repo]({{ site.fullcalendar_vue_repo }}). Please don't forget the [bug report instructions]({{ site.baseurl }}/reporting-bugs).


[Vue]: https://vuejs.org/
[Webpack]: https://webpack.js.org/
[Sass]: https://sass-lang.com/
[example project]: https://github.com/fullcalendar/fullcalendar-example-projects/tree/master/vue
[DemoApp.vue]: https://github.com/fullcalendar/fullcalendar-example-projects/blob/master/vue/src/DemoApp.vue
[docs toc]: https://fullcalendar.io/docs#toc
[component options]: https://github.com/fullcalendar/fullcalendar-vue/blob/master/src/fullcalendar-options.js
