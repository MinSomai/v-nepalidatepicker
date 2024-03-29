# Nepali Date Picker Vue 2

An easy-to-use and customizable nepali date picker component for vue2.

[![npm version](https://badge.fury.io/js/@minsomai%2Fv-nepalidatepicker.svg)](https://badge.fury.io/js/@minsomai%2Fv-nepalidatepicker)

##### Demo:

https://codesandbox.io/s/v-nepalidatepicker-z0sw3h

![Screenshot](screenshot.png)

## Install

```shell
npm install @minsomai/v-nepalidatepicker

OR

yarn add @minsomai/v-nepalidatepicker
```

## Quick Start

```javascript
import VNepaliDatePicker from "@minsomai/v-nepalidatepicker";

export default {
  components: {
    VNepaliDatePicker,
  },
  // rest of the component
};
```

## Customizable Properties

The following customizable properties can be added to the component

1. classValue
2. calendarType
3. placeholder
4. format
5. value
6. yearSelect
7. monthSelect
8. showToday
9. width

Events

1. @change - date string, emitted whenever the date changes
2. @ad - nepali to english converted date, emited whenever the date changes

## Examples - replace the input field to suit your needs

Here, we're using [vuetifyjs](https://vuetifyjs.com/) input field to replace the default input field with the scoped slot feature of vuejs.

```javascript
<template>
  <VNepaliDatePicker v-slot="scope">
    <v-text-field
      v-model="scope.formatedValue"
      @focus="scope.show"
      readonly
      solo
      hide-details
      flat
      outlined
      dense
      label="Select Date"
    ></v-text-field>
  </VNepaliDatePicker>
</template>

<script>
import VNepaliDatePicker from '@minsomai/v-nepalidatepicker';

export default {
  components: {
    VNepaliDatePicker,
  },
}
<script>
```

## Examples - classValue

This works exactly as class properties. Eg: classValue="form-control" (boostrap class)
(Note : In class="form-control", input will be rendered inside another input.)

```vue
<template>
  <v-nepalidatepicker classValue="datepicker" />
</template>
<style>
.datepicker {
  width: 50px;
  height: 20px;
}
</style>
```

## Examples - calendarType

Date picker is present in nepali language and English nepali language.
Default type will be English nepali.

For nepali language :

```vue
<template>
  <v-nepalidatepicker calendarType="Nepali" />
</template>
```

## Examples - placeholder

```vue
<template>
  <v-nepalidatepicker placeholder="YYYY-MM-DD" />
</template>
```

## Examples - format

Format the date to provide various output based on format string

```md
'yyyy-mm-dd' => २०७५-०२-०१
'YYY-MM-DD' => 075-02-01
'mmmm d, yyyy ddd' => जेष्ठ १, २०७५ मंगल
'MMM D, YYYY DDD' => Jes 1, 2075 Tue
```

```md
YYYY - 4 digit of year (2075)
yyyy - 4 digit of year in nepali unicode (२०७५)
YYY - 3 digit of year (075)
yyy - 3 digit of year (०७५)
YY - 2 digit of year
yy - 2 digit of year in nepali unicode (७५)
M - month number (1 - 12)
m - month number (१ - १२) in nepali unicode
MM - month number with 0 padding (01 - 12)
mm - month number in nepali unicode with 0 padding - (०१-१२)
MMM - short month name (Bai, Jes, Asa, Shr, etc.)
mmm - short month name in nepali unicde (ब‍ै, जे, अ, श्रा, etc)
MMMM - full month name (Baisakh, Jestha, Asar, ...)
mmmm - full month name nepali (बैसाख, जेष्ठ, ...)
D - Day of Month (1, 2, ... 31, 32)
d - Day of Month in Nepali unicode (१, २, ३ ... ३१, ३२)
DD - Day of Month with zero padding (01, 02, ...)
dd - Day of Month with zero padding in Nepali unicode (०१, ०२, ...)
DDD - Day of Week short form (Sun, Mon, Tue, ...)
ddd - Day of week in short form nepali (आइत, सोम, ...)
DDDD - Day of week full form (Sunday, Monday, Tuesday, ...)
dddd - Day of week full form nepali (आइतबार, सोमबार, ...)
```

```vue
<template>
  <v-nepalidatepicker format="YYYY-MM-DD" />
</template>
```

## Examples - value

Initial value for the datepicker.

```vue
<template>
  <v-nepalidatepicker value="2053-09-19" />
</template>
```

## Examples - yearSelect

The dropdown year select can be turned off using boolean type to yearSelect

```vue
<template>
  <v-nepalidatepicker :yearSelect="false" />
</template>
```

## Examples - monthSelect

The dropdown month select can be turned off using boolean type to monthSelect

```vue
<template>
  <v-nepalidatepicker :monthSelect="false" />
</template>
```

## Examples - events

You can listen to `@change`, `@event`

```vue
// example to listen @ad event
<template>
  <v-nepalidatepicker @ad="dateInAD">
</template>

<script>
import VNepaliDatePicker from '@minsomai/v-nepalidatepicker';

export default {
  components: {
    VNepaliDatePicker,
  },
  methods:{
    dateInAD(engDate){
      console.log(engDate)
    }
  }
}
<script>
```

## Examples - All in one

```vue
<template>
  <v-nepalidatepicker
    calendarType="Nepali"
    placeholder="YYYY-MM-DD"
    format="YYYY-MM-DD"
    value="2053-09-19"
    :yearSelect="false"
    :monthSelect="false"
    :width="265"
    :showToday="true"
    @change=""
    @ad=""
  />
</template>
```
