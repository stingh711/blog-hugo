+++
date = "2017-04-08T19:10:23+08:00"
title = "Javascript library notes"
Description = "Some notes on a few javascript libraries I am using"
+++

Some notes on a few popular javascript libraries. 

## bootstrap-daterangepicker

### How to get startDate and endDate?

```javascript
var range = $("#range").daterangepicker();
var start = range.data("daterangepicker").startDate;
var end = range.data("daterangepicker").endDate;
```

## lodash

### map

```javascript
var data = [{label: '4/1', value: 10}, {label: '4/2', value: 14}];
var labels = _.map(data, function(item) { return item.label; });
var values = _.map(data, function(item) { return item.value; });
```

It is especially useful when preparing data for some chart libraries.

### convert array to object

```javascript
var data = [{value: 0, name: 'Working'}, {value: 1, name: 'Idle'}];
var result = _.chain(data).keyBy('value').mapValues('name').value();
//The result will be {0: 'Working', 1: 'Idle'}
```

## jquery

### How to pass array when using $.get

```javascript
$.get("/api", {"name[]": ["George", "Jacky"]}, function(data) {});
```

If you are using Spring MVC for the server side, here is the relative code to receive multiple parameters.

```java
@GetMapping("/api")
@ResponseBody
public YourResponseEntity api(@RequestParam("name") String[] names) {
  //deal with names
}
```

